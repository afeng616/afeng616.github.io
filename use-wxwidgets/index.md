# Windows下通过CMake调用wxWidgets库+Hello World Demo

<!--more-->

使用CMake调用wxWidgets库的二进制文件，这种方式并没有在官方的文档中找到，但官方使用文档[^1]还是有很大帮助的。
本机不得已安装了VS，如果不是因为VS Code的一些局限，我可能都不会安装VS，既然已经装了就直接用它的编译环境吧MSVC。开发的时候依然还是想着使用VS Code，所以这里使用CMake编写编译时的一些细节。如果是想着用直接用VS开发可以研读官方文档[^1]尝试一下，也可以跟着我用CMake的方式生成一个VS的obj项目，然后手动点击`.sln`点击进入。
## 库文件下载

通过wxWidgets提供的下载网址[^2]下载库头文件、与编译环境对应的二进制文件，点击`Download Windows Binaries`选择指定文件即可下载。
![下载库][download-lib-files1]
![下载库][download-lib-files2]
根据本机环境选择需要下载的文件：
- Header Files（无论什么编译环境都需要下载）
- dll文件（根据系统编译环境选择下载，本机使用VS2019的编译器）
## 将库引入工程中

根据官方文档[^1]的说明，下载好库文件后需要将它们解压到同一个目录下，并且目录下应只有`include`文件夹和`lib`文件夹。
![解压库][unzip-lib-files]
本机将wxWidgets放在`3rdparty`目录下。
需要注意的是，要将目录`lib/vc14x_x64_dll`中vc的版本后缀删除掉，修改为`lib/vc_x64_dll`，不修改的话后续编译会报错无法找到xx文件（因为查找的目录不对）。
![引入库][import-lib-files]
## 编写Hello Demo 

使用官方Demo[^3]即可。
```cpp
// Start of wxWidgets "Hello World" Program
#include <wx/wx.h>
 
class MyApp : public wxApp
{
public:
    bool OnInit() override;
};
 
wxIMPLEMENT_APP(MyApp);
 
class MyFrame : public wxFrame
{
public:
    MyFrame();
 
private:
    void OnHello(wxCommandEvent& event);
    void OnExit(wxCommandEvent& event);
    void OnAbout(wxCommandEvent& event);
};
 
enum
{
    ID_Hello = 1
};
 
bool MyApp::OnInit()
{
    MyFrame *frame = new MyFrame();
    frame->Show(true);
    return true;
}
 
MyFrame::MyFrame()
    : wxFrame(nullptr, wxID_ANY, "Hello World")
{
    wxMenu *menuFile = new wxMenu;
    menuFile->Append(ID_Hello, "&Hello...\tCtrl-H",
                     "Help string shown in status bar for this menu item");
    menuFile->AppendSeparator();
    menuFile->Append(wxID_EXIT);
 
    wxMenu *menuHelp = new wxMenu;
    menuHelp->Append(wxID_ABOUT);
 
    wxMenuBar *menuBar = new wxMenuBar;
    menuBar->Append(menuFile, "&File");
    menuBar->Append(menuHelp, "&Help");
 
    SetMenuBar( menuBar );
 
    CreateStatusBar();
    SetStatusText("Welcome to wxWidgets!");
 
    Bind(wxEVT_MENU, &MyFrame::OnHello, this, ID_Hello);
    Bind(wxEVT_MENU, &MyFrame::OnAbout, this, wxID_ABOUT);
    Bind(wxEVT_MENU, &MyFrame::OnExit, this, wxID_EXIT);
}
 
void MyFrame::OnExit(wxCommandEvent& event)
{
    Close(true);
}
 
void MyFrame::OnAbout(wxCommandEvent& event)
{
    wxMessageBox("This is a wxWidgets Hello World example",
                 "About Hello World", wxOK | wxICON_INFORMATION);
}
 
void MyFrame::OnHello(wxCommandEvent& event)
{
    wxLogMessage("Hello world from wxWidgets!");
}
```

## 编写CMakeLists.txt

因为对C++程序的库依赖还不是很熟，所以折腾了一个下午才搞明白使用wxWidgets库与其他库无异。正常include头文件，如果需要`.lib`的话link一下。
本机使用官方已经编译好的binaries，所以需要指定`.lib`文件目录，当然头文件目录也是必须的。为了让`.exe`运行时查找到所需的dll，可以将`.dll`目录加入环境变量中，也可以通过`.bat`临时添加一下环境变量（环境变量只在本次`.bat`运行时有效）。

为了在链接wxWidgets时寻找到目录，需要设定`target_compile_definitions`。
由于wxWidgets使用`WinMain`作为函数入口，所以需要在`add_executable`时增加`WIN32`[^4]。
```txt
cmake_minimum_required(VERSION 3.24.1)

project(PCBRouter)

# set output path
set(INSTALL_PATH ${CMAKE_BINARY_DIR}/../)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${INSTALL_PATH}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${INSTALL_PATH}/bin)
set(LIBRARY_OUTPUT_PATH ${INSTALL_PATH}/lib)

# wxWidget3.2.2.1
set(WXWIDGETS_DIR ${CMAKE_SOURCE_DIR}/../3rdparty/wxWidgets)
set(WXWIDGETS_DLL_SUFFIX _vc14x_x64)

include_directories(${WXWIDGETS_DIR}/include)
include_directories(${WXWIDGETS_DIR}/include/msvc)

# source file
file(GLOB_RECURSE HEADER_FILES *.h*)
file(GLOB_RECURSE SOURCE_FILES *.c*)

add_executable(PCBRouter WIN32 ${HEADER_FILES} ${SOURCE_FILES})
# set compile mode for using wxWidgets normally
target_compile_definitions(PCBRouter PRIVATE WXUSINGDLL=1)
target_compile_definitions(PCBRouter PRIVATE _UNICODE=1)

target_link_directories(PCBRouter PUBLIC ${WXWIDGETS_DIR}/lib/vc_x64_dll)
```

CMakeLists.txt编写好了以后，简单写一个`.bat`脚本来方便build项目。
最后一样将直接启动VS加载该项目，我这里不需要使用VS所以就注释掉了。
```bat
@echo off

rem Please set the build type to desired one, Debug or Release
set BUILD_TYPE=Debug
rem Default for VS2022, using "Visual Studio 15 2017 Win64" if your generator is VS2017
set NMAKE_GENERATOR="Visual Studio 16 2019"

set SOURCE_DIR=%~dp0/src
set INSTALL_DIR=%~dp0/src_output
set TARGET_DIR=%INSTALL_DIR%/obj

call cmake --no-warn-unused-cli -DCMAKE_INSTALL_PREFIX:STRING=%INSTALL_DIR% -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE -S%SOURCE_DIR% -B%TARGET_DIR% -G %NMAKE_GENERATOR% -T host=x64 -A x64
call cmake --build %TARGET_DIR% --config %BUILD_TYPE% --target ALL_BUILD -j 6 --

@REM call cmake --open %TARGET_DIR%
```

运行以上这个脚本后，会在工程根目录生成`src_output`目录，目录下存在文件夹`obj`和`bin`分别存储build后的文件和生成的二进制文件。
![编译后的文件][build-path]
## 最终效果

到这里整个wxWidgets库的引入已经接近尾声了，但是在运行时会发现不把指定`.dll`文件copy到`.exe`文件目录下无法运行，会报错`.dll`无法找到。针对这个问题上面也说到了两个方法，其实都是针对环境变量的修改，这里我还是比较喜欢通过`.bat`临时增加环境变量的方式来解决。

如果不想让命令行在`.exe`运行时持续存在，可以在最后一行前增加`start`。
```bat
@echo off

set BUILD_TYPE=Debug
set PROJECT_NAME=PCBRouter
set INSTALL_DIR=%~dp0/src_output
set WXWIDGETS_DIR=%~dp0\3rdparty\wxWidgets

rem temporarily add wxWidgets LIBS to PATH
set PATH=%WXWIDGETS_DIR%\lib\vc_x64_dll;%PATH%

"%INSTALL_DIR%/bin/%BUILD_TYPE%/%PROJECT_NAME%.exe"
```

程序运行起来后，效果如下：
![运行效果][run-Hello-Demo-example]

整个项目目录如下：
![整体项目目录][project-dir]


[download-lib-files1]: /images/blog/20230922-download-lib-files1.png "下载wxWidgets库文件"
[download-lib-files2]: /images/blog/20230922-download-lib-files2.png "下载wxWidgets库文件"
[unzip-lib-files]: /images/blog/20230922-unzip-lib-files.png "解压wxWidgets库文件"
[import-lib-files]: /images/blog/20230922-import-lib-files.png "引入wxWidgets库文件"
[build-path]: /images/blog/20230922-build-path.png "build后生成的目录"
[run-Hello-Demo-example]: /images/blog/20230922-run-Hello-Demo-example.png "Demo运行效果"
[project-dir]: /images/blog/20230922-project-dir.png "整体项目目录"

[^1]: https://docs.wxwidgets.org/latest/plat_msw_binaries.html wxWidgets文档: How to use wxMSW binaries
[^2]: https://www.wxwidgets.org/downloads/ wxWidgets下载网址
[^3]: https://docs.wxwidgets.org/latest/overview_helloworld.html wxWidgets Demo
[^4]: https://www.jarvis73.com/2017/11/06/CMake-Learning/#add_executable 博客: CMake 学习笔记
