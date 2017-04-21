# Android打包APK
Android要求所有应用都有一个数字签名才会被允许安装在用户手机上，所以你需要先生成一个签名的APK包。
> ## 生成一个签名密钥
你可以用==keytool==命令生成一个私有密钥。在Windows上==keytool==命令放在JDK的bin目录中（比如==C:\Program Files\Java\jdkx.x.x_x\bin==），你可能需要在命令行中先进入那个目录才能执行此命令。
```
$ keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```
这条命令会要求你输入密钥库（keystore）和对应密钥的密码，然后设置一些发行相关的信息。最后它会生成一个叫做==my-release-key.keystore==的密钥库文件。

在运行上面这条语句之后，密钥库里应该已经生成了一个单独的密钥，有效期为10000天。--alias参数后面的别名是你将来为应用签名时所需要用到的，所以记得记录这个别名。

---
> ## 设置gradle变量
把==my-release-key.keystore==文件放到你工程中的==android/app==文件夹下。

编辑 用户目录（比如C:Users\用户名）\ .gradle\gradle.properties（没有这个文件你就创建一个）,添加如下代码（把****替换为相应密码）
```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```
上面的这些会作为全局的gradle变量，我们在后面的步骤中可以用来给应用签名。

---
> ## 添加签名到项目的gradle配置文件
编辑你项目目录下的==android/app/build.gradle==，添加如下的签名配置：
```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```
---

> ## 生成发行apk包
在Android目录下输入
```
gradlew installRelease
```
==注意==：在测试版本和发布版本间来回切换安装时可能会报错签名不匹配，此时需要先卸载前一个版本后再尝试安装。