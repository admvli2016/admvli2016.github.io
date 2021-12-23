---
title: Ionic5.x+Capacitor+Vue3.x 尝鲜（Android）
date: 2021-12-10 00:04:33
tags: [Ionic5.x,Capacitor,Vue3.x,Android]
categories: 移动端开发
---

发现 ionic 最新版本支持了  Vue3.x，于是赶紧下载来尝尝鲜!

**环境和版本**

```
Windows 10 64位
node v12.22.0
@ionic/cli v6.16.3
Android Studio v4.2.2
```

<!--more-->

## 创建 ionic5.x 项目

1.1 创建项目，使用 Ionic 官网的快捷创建方式，生成模板代码，并将代码推到自己的 github 上

https://ionicframework.com/start#basics

https://dashboard.ionicframework.com/app/1982d30d/getting-started/overview

```shell
# 安装 @ionic/cli 和 cordova-res
npm install -g @ionic/cli cordova-res
# 拉取刚才创建的 ionic 项目
git clone https://github.com/admvli2016/my-ionic-app-1.git my-ionic-app-1
# 进入项目目录 && 安装依赖 && 运行项目
cd my-ionic-app-1 && npm install && ionic serve
```

## Android 环境配置

SDK Manager 的 Sdk 安装路径是: `C:\Users\admvli2016\AppData\Local\Android\Sdk`

2.1 下载最新版本的 Android Studio ( v4.2.2 )，然后安装

2.2 在 Android Studio 中配置 Android Sdk（更改 Android Sdk 配置后需要重新运行、打包项目）

打开 Android Studio 并按照下列步骤操作：

![点击Configure](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_46223_qBWSIfH5zm9_Xsl0_1627538582%5B1%5D.png)

![打开SDK Manager](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_288955_fybGb59iupehM0YE_1627538589%5B1%5D.png)

![SDK Platforms](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_6363__R0YRWoK7bafGEjk_1627538653%5B1%5D.png)

出现以下弹窗，点击 OK 继续

![点击OK](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_229129_mKX0al6iAHOrhxhg_1627538675%5B1%5D.png)

出现如下页面，安装完成后，点击 Finish

![点击Finish](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_856869_epc79MBGMA2K9jcF_1627538692%5B1%5D.png)

切换到 SDK Tools 选项卡

![SDK Tools](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_585643_Z2AuYEcbvuPZera8_1627538719%5B1%5D.png)

同上，出现如下页面，安装完成后，点击 Finish

![点击Finish](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_120353_uz4_F5KIiWpSeHr8_1627538753%5B1%5D.png)

## 在 Android Studio 中打开项目（1）

```shell
# 安装 @capacitor/android 插件
npm install @capacitor/android
# 添加 android 平台
npx cap add android
# 在 Android Studio 中打开项目
npx cap open android   
```

运行报错：卡在这里了，只好选择重新创建项目，现在回想一下应该是没有运行 ` ionic build` 

```shell
Settings file 'D:\Github\my-ionic-app\android\settings.gradle' line: 5
A problem occurred evaluating settings 'android'.
> Could not read script 'D:\Github\my-ionic-app\android\capacitor.settings.gradle' as it does not exist.
......
Caused by: org.gradle.api.resources.MissingResourceException: Could not read script 'D:\Github\my-ionic-app\android\capacitor.settings.gradle' as it does not exist.
```

## 以命令行形式重新创建 ionic5.x 项目

执行以下命令

```shell
# 安装全局依赖 @ionic/cli@latest、native-run、cordova-res
npm install -g @ionic/cli@latest native-run cordova-res
# 创建项目（关闭vpn，否则会报错）
ionic start ionic5-vue3-demo tabs
# 运行项目
ionic serve
# 打包项目
ionic build
# 添加平台 ios/android 文件夹（本机项目）
ionic cap add ios
ionic cap add android
# 每次执行完 ionic build（有更新） 之后，都需要执行以下命令，将更改复制到本机项目
ionic cap copy || ionic cap update
# 对代码的本机部分进行更新（例如添加新插件）后，使用以下 sync 命令
ionic cap sync
```

## 在 Android Studio 中打开项目（2）

```shell
# 打开 android 项目  
ionic cap open android
```

系统 gradle 配置路径：C:\Users\admvli2016\.gradle\gradle.properties

 运行报错：

![运行报错](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_341377_Y5ha18NEhImSevOp_1627539160%5B11%5D.png)

![运行报错](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_68619_0_snrazINBVO9mQ2_1627539166%5B1%5D.png)

原因：Android Studio 存在版本问题，老版本的 Android Studio 可能需要代理设置才能够使用，所以前几年网上关于这个问题的博文都是说需要设置代理。但是由于自己安装的是最新版本的 Android Studio，不需要代理就能够使用，所以这里也算是踩了一个坑！

[(3条消息) 解决Android Studio 无法通过gradle 下载https://dl.google.com/android/repository/addons_list-3.xml 解决办法_Ansel_i的博客-CSDN博客](https://blog.csdn.net/Ansel_i/article/details/115521706)

解决方案：关闭 VPN，删除掉所有代理设置

在 Android Studio 中打开 "设置 HTTP Proxy" 页面的快捷键 Ctrl +Alt + S

## 运行项目并安装到真机上

6.1 在 Android Studio上选择要安装的设备

若找不到设备，选择 **Troubule Shoot Device Connections**，按步骤一步一步来即可找到设备

![选择要安装的设备](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_196601_xLc6Tuh_mk98EFyD_1627539300%5B1%5D.png)

6.2  点击运行 ▸

安装到真机上失败

```shell
Installation did not succeed.
The application could not be installed: INSTALL_FAILED_TEST_ONLY

List of apks:
[0] 'D:\Github\ionic5-vue3-demo\android\app\build\outputs\apk\debug\app-debug.apk'
Installation failed due to: 'null'
```

[(3条消息) Android 安装APP 失败 INSTALL_FAILED_TEST_ONLY_爱孔孟-CSDN博客](https://blog.csdn.net/aikongmeng/article/details/102589809)

解决方案：在 gradle.properties 文件中添加如下内容

```properties
android.injected.testOnly=false
```

重新运行，安装到真机成功！！！

## 在 Android Studio 中将程序打包成 APK

[(3条消息) 在Android Studio中如何将程序打包成APK_StevenAzy的博客-CSDN博客_android studio怎么打包成apk](https://blog.csdn.net/qq_33317126/article/details/79531438)

在 Android Studio 中打开项目，按照下列步骤进行操作

![Build](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_346912_e9EAtUZrdWsyQlIF_1627607335%5B1%5D.png)

![选择APK](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_566472_KhM0gPwqdDfcQAB0_1627607326%5B1%5D.png)

如果之前有编译成 APK 的话，就直接选择 Choose existing key

如果没有编译成 APK 的， 那就选择 Create new key

![选择Create new key](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_587951_Ao_6V_E7lt3yd6gS_1627607388%5B1%5D.png)

选择新的 key 的存放路径，点击 OK 继续

![选择key存放路径](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_547362_WdAqEfn26napiJ4N_1627607433%5B1%5D.png)

填上密码，其中 First and Last Name 填一下，其他的可不填。

点击 OK 继续下一步

![填写 New Key Store](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_638872_JyoBXkT3kLhkbOR9_1627607502%5B1%5D.png)



![填写 Comfirm](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_98758__1Z0mbDbmQEenRk9_1627607533%5B1%5D.png)

点击 Next 继续下一步（**这一步不能够记住密码，否则可能会导致之后 build 报错**）

![生成APK](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_545387_QjaM9mwnLTJ5UN5l_1627607595%5B1%5D.png)

点击 Finish，最终生成的 APK 文件就在下图所示的路径中

![点击Finish](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_70311_7HoTSeRj7AW_clU6_1627607627%5B1%5D.png)

打包成功！！！

![打包成功](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_515088_wTW1pYLYzTnXpBlA_1627607683%5B1%5D.png)



![生成的APK](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_324088_BVj9ozYSZCFBQLDH_1627607700%5B1%5D.png)

**注意：再次打包时需要将之前打包生成的 release 或 debug 文件夹删除，才能看到效果**

