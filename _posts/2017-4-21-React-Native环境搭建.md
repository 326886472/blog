# React-Native环境搭建
#### 安装必须的软件
##### Chocolatey
Chocolatey是一个Windows上的包管理器，类似于linux上的yum和 apt-get。 你可以在其官方网站上查看具体的使用说明。一般的安装步骤应该是下面这样：
```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

一般来说，使用Chocolatey来安装软件的时候，需要以管理员的身份来运行命令提示符窗口。译注：chocolatey的网站可能在国内访问困难，导致上述安装命令无法正常完成。请使用稳定的翻墙工具。 如果你实在装不上这个工具，也不要紧。下面所需的python2和nodejs你可以分别单独去对应的官方网站下载安装即可。

##### Python 2
打开命令提示符窗口，使用Chocolatey来安装Python 2.
```
choco install python2
```
##### Node
打开命令提示符窗口，使用Chocolatey来安装NodeJS。注意，目前已知Node 7.1版本在windows上无法正常工作，请避开这个版本！
或者可以直接到node官网（https://nodejs.org/en/）下载 v6.10.2 LTS
```
choco install nodejs.install
```
安装完node后建议设置npm镜像以加速后面的过程（或使用科学上网工具）。注意：不要使用cnpm！cnpm安装的模块路径比较奇怪，packager不能正常识别！
```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```
##### Yarn、React Native的命令行工具（react-native-cli）
Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载。React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。
```
npm install -g yarn react-native-cli
```
安装完yarn后同理也要设置镜像源：
```
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
```
如果你遇到EACCES: permission denied权限错误，可以尝试运行下面的命令（限linux系统）： sudo npm install -g yarn react-native-cli.
##### AndroidSDK和模拟器
模拟器可以下载Android Studio或者使用真机。
你需要开启USB调试才能在你的设备上安装你的APP。首先，确定你已经打开设备的USB调试开关

确保你的设备已经成功连接。可以输入adb devices来查看:
```
$ adb devices
List of devices attached
emulator-5554 offline   # Google模拟器
14ed2fcc device         # 真实设备
```
现在你可以运行react-native run-android来在设备上安装并启动应用了。


将AndroidSDK中platform-tools和tools的目录路径加入PATH环境变量，在命令行中运行adb
测试是否配置成功。
新建ANDROID_HOME环境变量，路径为AndroidSDK路径，如:C:USERS\joelm\AppDate\Local\Android\sdk

你需要关闭现有的命令符提示窗口然后重新打开，这样新的环境变量才能生效。
##### 安装JDK(1.7)
新建一个文件夹jdk（第一次路径是jdk的安装路径）安装jdk，之后在安装过程中，第二次选择路径时，在jdk中新建一个jdk(或是其他文件名)用来作为第二次的选择路径(第二次路径是jre路径)，如果两次的路径一样就会发生覆盖。

设置环境变量
新建JAVA_HOME 值为jdk的路径
添加jdk/bin到PATH
##### 测试安装
找到项目中package.json文件，把react-native的版本号改成0.38

把react-native-swipeout中的index.js文件中的开头替换为
```
var tweenState = require('react-tween-state')
var styles = require('./styles.js')
import React, { Component } from 'react';
import  {
    PanResponder, TouchableHighlight, StyleSheet, Text, View
} from 'react-native';
```

在项目的根目录下运行
```
npm install
```
根据package.json下载依赖项
```
react-native start
```
启动项目
```
react-native run-android
```
在设备上安装运行

==注意==：如果出现在10/12的时候卡主报错，可能是因为闪屏显示出错，可以尝试在android-app-src-java-com-realtimemedical-MainApplication.java中在new *RCTSplashScreenPackage(MainActivity.mainActivity)*后添加false参数跳过闪屏。

你可以使用--version参数创建指定版本的项目。例如react-native init MyApp --version 0.39.2。注意版本号必须精确到两个小数点。