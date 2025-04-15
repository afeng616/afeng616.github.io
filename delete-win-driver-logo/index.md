# 删除设备和驱动器中的图标

<!--more-->

## 说明
情况是这样的  
![设备和驱动器多余图标][win-driver-logo]

## 操作
主要操作如下：
1. 打开注册表（<kbd>WIN</kbd><kbd>R</kbd>键入`regedit`）
2. 搜索路径
    ```path
    HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\
    ```
3. 删除不需要的图标项
    ![修改注册表][modify-reg]

[win-driver-logo]: /images/blog/20220401-win-driver-logo.png "设备和驱动器多余图标"
[modify-reg]: /images/blog/20220401-modify-reg.png "修改注册表"

