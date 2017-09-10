---
title: ionic项目环境配置
date: 2017-07-12 11:06:00
tags:
 - Ionic
 - Cordova
categories: 前端技术

---

Ionic + Cordova的环境配置

<!-- more -->

### 环境搭建

**安装Node环境与NPM包管理工具**

[官网下载](https://nodejs.org/en/download/)

**其他依赖**

+ Java_Home  C:\Program Files\Java\jdk1.8.0_101
+ JRE_HOME C:\Program Files\Java\jre1.8.0_101
+ ANDROID_HOME C:\Program Files\adt\sdk(安卓编译)
+ Xcode(ios编译)

**Windows安装陷阱:**  

+ **坑一:** 配置npm的全局模块的存放路径

`npm config set prefix 'C:\Program Files\nodejs\node_global'`
`npm config set cache 'C:\Program Files\nodejs\node_cache'`

在系统环境变量**`Path`** , 添加 `C:\Program Files\nodejs\node_global\`

改完配置以后，**重启cmd，重启cmd，重启cmd**, 这样就可以在命令行启用全局`npm`命令<br><br>


+ **坑二:** 在.npmrc这个文件中加一行`registry=https://registry.npm.taobao.org`或者`npm config set registry http://registry.npm.taobao.org`<br><br>

+ **坑三:** npm下标不停的闪 删除C:\Users\Administrator\下的npmrc文件

### 安装ionic和cordova
`npm install -g ionic cordova`, 将ionic和cordova包安装到全局环境中, 供命令行使用

### ionic与cordova常用命令集合

**创建ionic项目** `ionic start proj_name`

**添加平台** `ionic platform add android`, `ionic platform add ios` 

**生成android apk或ios xproj** `ionic build android`, `ionic build ios`

**生成android apk或ios xproj然后在真机或模拟器中运行** `ionic run android`, `ionic run ios` 

**在浏览器中预览** `ionic serve -p 8080`

**cordova插件安装** `cordova plugin add plugin_name`


### Cordova相关

[官网](https://cordova.apache.org)

[ngCordova](http://ngcordova.com)
