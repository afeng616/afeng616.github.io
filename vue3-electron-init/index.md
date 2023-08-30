# windows下vue3+electron的环境搭建

<!--more-->

## 项目初始化

基于vue3+electron开发桌面应用。  

### 环境准备

windows从零新建一个vue3+electron的项目说明，包括各种环境安装。

1. 安装npm  
- npm的安装可以直接使用官方的.exe文件[^1]。  
- 当然也可以使用nvm[^2]进行安装，相比起来更方便，还可以进行npm的版本管理。  

  nvm安装完毕以后，打开命令行，安装node版本V16（V18版本在安装electron时会有openssl-legacy-provider问题，比较难解决）。
  ```shell
  nvm install 16
  nvm use 16
  ```

2. 安装vue-cli  
   ```shell
   npm install -g @vue/cli
   ```

3. 安装electron  
   ```shell
   npm install electron --save-dev
   ```
   安装过程中可能会遇到下载失败的问题[^3]，解决方法如下：
   ```
   npm config edit
   ```
   修改`.npmrc`文件配置并保存。
   ```
   registry=https://registry.npmmirror.com
   electron_mirror=https://cdn.npmmirror.com/binaries/electron/
   electron_builder_binaries_mirror=https://npmmirror.com/mirrors/electron-builder-binaries/
   ```

### 新建架构vue3+electron

vue3+electron环境初始化[^4]步骤如下：

```shell
# 新建vue项目，myproject为项目名称，只能小写
vue create myproject
cd myproject
# 添加electron
vue add electron-builder
```

```shell
# 项目启动
npm run electron:serve
```

## 项目共享

当与他人共享源码时，不需要拷贝`/node_modules`和`/dist_electron`文件夹以及`package-lock.json`文件。  
由于npm环境不同，很可能导致不必要的错误，它们直接由以下命令生成。
```shell
# vue和electron安装完毕后
npm install
```

## References

[^1]: https://nodejs.org/zh-cn/download "nodejs官方下载网址"
[^2]: https://github.com/coreybutler/nvm-windows/releases "nvm-windows下载网址"
[^3]: https://www.cnblogs.com/makalochen/p/16154510.html "cnblog: npm 安装electron 失败的问题和解决办法"
[^4]: https://segmentfault.com/a/1190000038463122 "segmentfault: Vue+Electron项目简洁快速搭建教程"




