# Win10自启动管理

<!--more-->

## 禁止自启动程序
1. 打开`任务管理器`
2. 在`启动`一栏中选择指定程序右键禁止其自启动

- 打开【任务管理器】的方法
    1. <kbd>Ctrl</kbd><kbd>Shift</kbd><kbd>Esc</kbd>
    2. <kbd>Ctrl</kbd><kbd>Alt</kbd><kbd>Del</kbd> -> `任务管理器`
    3. <kbd>Win</kbd><kbd>R</kbd> -> `taskmgr`

## 添加自启动程序
1. 将需要自启动的`程序快捷方式`添加到`自启动程序文件夹` ***推荐***  
可以通过命令启动，<kbd>Win</kbd><kbd>R</kbd> -> `shell:startup`。 
```path
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```

2. 编辑`注册表`，在指定目录下右键新建`字符串值` ***不建议***
```path
\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```
```path
\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
```



