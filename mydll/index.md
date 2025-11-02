# DLL调用方式测试

<!--more-->


## dll说明

dll（Dynamic Link Library，动态链接库），是一种可以被其它程序调用的程序。  
dll文件可以包含代码、数据和资源，允许多个程序同时访问同一个dll文件，这样可以节省内存空间，提高系统性能。

dll的调用方式主要两种，动态调用（显式链接）和静态调用（隐式链接）。  
由于调用方式不同，所以在代码编写时有有所不同，但是区别并不大。  
可以通过上面的名称看出他们差异[^1]体现在链接阶段。

所需文件如下：  
|   静态调用   | 动态调用  |
| :----------: | :-------: |
| .h .lib .dll | (.h) .dll |

在动态调用时，如果导出的只是函数则不需要.h。  
如果导出的是一个抽象的功能基类[^3]，则需要这个基类的头文件，具体实现类的头文件不需要。  

## 一些细节

首先明确，调用的是动态链接库dll而不是静态链接库lib，
虽然生成dll时同样会存在lib[^2]，但这个文件和前者有明显不同，完全是两码事。  
其次，不论是上述哪种调用方式使用dll，程序在运行时都需要用到dll文件。
而lib文件只有使用静态调用的方式在链接阶段需要，动态调用的方式从始至终都不需要lib文件。

一般情况下认为，静态调用需要`.h`、`.lib`、`.dll`，动态调用需要`.dll`。

dll导出需要使用关键字`__declspec(dllexport)`标识，一般对它进行宏定义。
dll在设计之初只是为了方便导出函数级的接口，所以最好避免导出变量或类，可能会出现莫名其妙的crash[^4]。  
使用动态调用时需要注意，在导出后，编译器会对函数名称进行修改，形如`?getInstance@@YA?AV?`，可以通过`Dependency Walker`等工具[^5]查看。
可以通过增加关键字`extern "C"`来避免这种情况，但这样会失去`C++`的重载功能。

静态调用时，在头部使用`#pragma comment(lib, "xxx.lib")`指定链接的lib文件。  
经过测试发现新的一点，如果是使用cmake对项目进行编译，使用`target_link_libraries`指定链接的lib文件也能达到相同效果。

## 代码示例

```cpp
// exportclass.h
#ifndef __EXPORTCLASS_H__
#define __EXPORTCLASS_H__

#ifndef MYDLL_EXPORTS
#define MYDLL_EXPORTS __declspec(dllexport)
#else
#define MYDLL_EXPORTS __declspec(dllimport)
#endif

class MYDLL_EXPORTS EX
{
public:
    EX();
    void test();
};

#endif // __EXPORTCLASS_H__
```
```cpp
// exportclass.cpp
#include <iostream>
#include "exportclass.h"

EX::EX() {}

void EX::test()
{
    std::cout << "test" << std::endl;
}
```
```cpp
// main.cpp
#include <iostream>
#include <Windows.h>
#include "module/exportclass.h"

#pragma comment(lib, "mydll.lib")  // unnecessary while dynamic load

void loadDllDyn(std::string funcName)
{
    // Load DLL
    HMODULE myDLL = LoadLibrary("mydll.dll");
    if (myDLL != NULL)
    {
        // Get function pointer
        typedef EX (*GetInstanceFunc)();
        GetInstanceFunc getInstance = (GetInstanceFunc)GetProcAddress(myDLL, funcName.c_str());
        if (getInstance == NULL)
        {
            std::cout << "Error: getInstance function not found" << std::endl;
        }
        else
        {
            // Call function
            EX ex = getInstance();
            ex.test();
        }
        // Unload DLL
        FreeLibrary(myDLL);
    }
    else
    {
        std::cout << "Error: no dll be found" << std::endl;
    }
}

int main()
{
    try
    {
        // dynamic load
        loadDllDyn("??0EX@@QEAA@XZ");

        // static load
        EX ex;
        ex.test();
    }
    catch(const char* msg)
    {
        std::cout << msg << std::endl;
    }
    std::cout << "exit" << std::endl;

    return 0;
}
```
为了偷懒，上面的例子中直接导出了整个实现类，并没有使用导出抽象基类或导出函数的方式，这点在编写代码时可优化。


## 参考资料
[^1]: https://www.cnblogs.com/xiangtingshen/p/10979419.html "博客园: 动态链接库dll的静态加载与动态加载"
[^2]: https://blog.csdn.net/qq_29007291/article/details/113439343 "CSDN: windows visual studio生成dll总是伴随着lib"
[^3]: https://blog.csdn.net/qiangzi4646/article/details/79628260 "CSDN: dll导出类比较好的方式"
[^4]: https://club.topsage.com/thread-497586-1-1.html "论坛: DLL导出类避免地狱问题的完美解决方案"
[^5]: https://github.com/JelinYao/dependency "GitHub: dependency"


