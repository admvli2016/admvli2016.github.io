---
title: React Native 0.66 Demo 集成极光推送（iOS）
date: 2021-12-27 01:06:59
tags: [React Native,iOS,极光推送]
categories: 移动端开发
typora-copy-images-to: upload
---

需要打开 ios 目录下的 `.xcworkspace` 文件修改包名。

因为之前集成 Code Push 时，已经修改过包名，这里就不需要再进行处理了，如下图所示

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_1640532178436.png)

<!--more-->

## 安装  jcore-react-native、jpush-react-native

```shell
# 安装 jcore-react-native
npm install --save jcore-react-native
# 安装 jpush-react-native
npm install --save jpush-react-native
```

安装完成后连接原生库，进入到项目目录执行以下命令

```shell
npx react-native link jpush-react-native
npx react-native link jcore-react-native
```



## 项目配置

（1）运行以下命令安装  `CocoaPods` 依赖项。（需要打开 `VPN`）

```shell
# 安装 CocoaPods 依赖项
cd ios && pod install && cd ..
```

如果项目使用 `pod` 安装过，先执行以下命令清除

```shell
pod deintegrate
```

（2）在 `Xcode` 中打开项目，在 `Libraries` 目录下添加下面两个文件

```
node_modules/jcore-react-native/ios/RCTJCoreModule.xcodeproj
node_modules/jpush-react-native/ios/RCTJPushModule.xcodeproj
```

`Libraries` 目录位置

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_599601_36PxfjK2dq__dOW8_1634901807%5B1%5D.png" style="zoom:50%;" />

右键点击 `Libraries` ，选择 `Add Files to "MyReactNativeDemo"...`，将上述两个文件添加到 `Libraries` 下。

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_667062_z4FMnxcoNrr9T1UV_1634901526%5B1%5D.png" style="zoom:50%;" />

（3）在 `Xcode` 中开启通知推送功能

[iOS Xcode12打开远程推送（Push Notifications）开关 - 简书](https://www.jianshu.com/p/3a83882acec8)

参照上述博文，开启通知推送功能

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_740993_A2Fuo1qheZs4ezJB_1634902113%5B1%5D.png)

（4）在 `Xcode` 的 `Build Settings` 选项卡下进行如下配置

操作位置：`Build Settings > All & Levels > Search Path > Header Search Paths `

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_802720_iw6XOUHc4hjk9yNn_1634902498%5B1%5D.png)

点击 `+` 按钮，然后添加如下内容：

```
$(SRCROOT)/../node_modules/jcore-react-native/ios/RCTJCoreModule/
$(SRCROOT)/../node_modules/jpush-react-native/ios/RCTJPushModule/
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_261168_AyydDn6fGUeJki5L_1634902888%5B1%5D.png)

如下图所示，点击 `+` 按钮添加上述内容

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_152396_fMSmFf3HVl3FdesN_1634902913%5B1%5D.png" style="zoom: 50%;" />

（5）在 `Xcode` 的 `Build Phases` 选项卡下进行如下配置

操作位置：`Build Phases > Link Binary With Libraries`

点击 `+` 按钮将以下内容添加进去

```
libz.tbd
libresolv.tbd
UserNotifications.framework
libRCTJCoreModule.a
libRCTJPushModule.a
```

（6）修改 `AppDelegate.m` 文件

文件位置：`ios/MyReactNativeDemo/AppDelegate.m`

进行如下修改（共4处）：

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_496093_WafAvvqleW7cRdeL_1634904175%5B1%5D.png" style="zoom: 33%;float:left" />

<hr style="clear:both"/>



<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_722288_M24JnYYxAh8wYF-n_1634904195%5B1%5D.png" style="zoom:33%;float:left" />

<hr style="clear:both"/>



<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_110388_KW4dDKEQi2ALHp9i_1634904265%5B1%5D.png" style="zoom: 33%;float:left" />

<hr style="clear:both"/>



<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_514851_hd_RQHdmKNxlNgMQ_1634904303%5B1%5D.png" style="zoom: 33%; float:left" />

<hr style="clear:both"/>



## 在项目中使用 jpush-react-native

直接使用官方的 `App.js` 例子

[jpush-react-native/App.js at dev · jpush/jpush-react-native](https://github.com/jpush/jpush-react-native/blob/dev/example/App.js)

稍作修改，添加如下内容：

```html
<!-- 给本机应用设置别名，其中 '190395486111268864' 是自己的因思云 userId -->
<Button 
    title="setAlias"
    onPress={() =>
      JPush.setAlias({sequence: 6, alias: '190395486111268864'})
    }/>
```



## 通过内测分发将项目安装到手机上

`Archive` 时报错：找不到此文件 `React/RCTBridge.h`

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_287134_3V_-Nex5JOL6mzF9_1634905298%5B1%5D.png)

重新执行 `pod install` ，还是报错。

<hr />

[React Native新项目启动报错'React/RCTBridgeDelegate.h' file not found_hahahhahahahha123456的博客-CSDN博客](https://blog.csdn.net/hahahhahahahha123456/article/details/103790646)

参照上述博文，执行以下命令

```shell
cd ios
pod deintegrate
pod install
```

还是报错。

<hr />

[ios - React Native build failed: 'React/RCTBridge.h' file not found - Stack Overflow](https://stackoverflow.com/questions/50453883/react-native-build-failed-react-rctbridge-h-file-not-found/50460552)

参照上述博文，执行以下操作

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211227000609305.png)

还是报错。

<hr />

尝试重新连接 jpush-react-native、jcore-react-native，再执行之前的步骤

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211227000609305.png)

还是报错。

<hr />

尝试修改 `AppDelegate.m` 文件，将 

```js
#import <RCTJPushModule.h>
```

移动到

```js
#import <React/RCTBridge.h>
```

的下方。

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_961904_uHOUo3-KIqDZGlTY_1634961364%5B1%5D.png" style="zoom:33%;" />

还是报错。

<hr />

[React/RCTBridge.h file not found 的解决方法_Slako的博客-CSDN博客](https://blog.csdn.net/fanzhiri/article/details/84833343)

[ReactNative的极光推送插件极成失败-极光社区](https://community.jiguang.cn/question/420785)

[ios build 失败 · Issue #778 · jpush/jpush-react-native · GitHub](https://github.com/jpush/jpush-react-native/issues/778)

参照上述博文，移除所有手动安装的内容，采用自动安装 `pod install`

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_571998_ThDSxoMyEgXhe7FC_1634961929%5B1%5D.png)

删除 Pods ，重新 pod install 的步骤：

```shell
cd ios
rm -rf Pods
pod deintegrate
pod install
```

打包成功！

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_4061__JQ8O9nqKaBpBvV0_1634962198%5B1%5D.png)

重新 `Archive` ，通过内测分发操作将打包好的应用安装到手机上。

<hr />

安装到手机上时出现闪退问题，本地运行一下 `yarn ios`

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_856186_QWYx5FlVthZsw3fz_1634963252%5B1%5D.png)

执行以下命令：

```shell
npx react-native unlink jcore-react-native
npx react-native unlink jpush-react-native
```

重新安装一下，还是会出现闪退问题。

<hr />

在 `Xcode` 上面 `Run` 一下，查看报错信息：

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_239608_lyeMgS3RD5IRLHIZ_1634968371%5B1%5D.png)

找到原因了，跟之前 Android Demo 集成极光推送时出现的报错一样，`addCustomMessagegListener` 拼写错误，多了一个 `g`，删掉之后 `Archive` 一下，重新安装到手机上。如下所示，可以正常使用。

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_100602_8OR1a0x_kXSWZduQ_1634969872%5B1%5D.png" style="zoom:25%;" />


然后点击 `setAlias` 按钮，给该 `App` 设置别名。

> 注意：**一定要打开手机的通知权限！**



## 验证极光推送是否集成成功

本人的经历：

尝试模拟推送通知，但是 iPhone 手机上未接收到推送通知

尝试发送 Android 平台的推送通知，Android 手机上可以接收到推送通知

连续发送 5 次通知，iOS 上都接收不到，极光后台报错：“获取 token 出错，token 不存在”

尝试删除 `App`，重新安装，不起作用

尝试删除 `App`，重启手机，然后再安装 `App`，还是不起作用

[Not get devicetoken，获取不到 devicetoken-极光社区](https://community.jiguang.cn/article/56461)

[集成 jpush-react-native 常见问题汇总 （ iOS 篇） - SegmentFault 思否](https://segmentfault.com/a/1190000009357351)

极光官方的回答无法解决问题，缺少具体的调试信息；尝试在 `Xcode` 上面调试一下（使用模拟器），但是发现 `Xcode` 无法打印相关的调试信息。

<hr />

[React Native 0.66 Demo 搭建全过程（iOS） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/25/React-Native-0-66-Demo-搭建全过程（iOS）/#运行项目（真机）)

参照上述博文中的步骤 3.2，进行真机联调来获取调试信息。`Packager` 打印的日志信息如下所示：

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_873767_Hl_xhYAwu6kFwHew_1635129043%5B1%5D.png)

[iOS设置alias一直返回6022的错误码, alias一直设置不成功.-极光社区](https://community.jiguang.cn/question/231320?fs=1)

[使用别名alias推送，有些可以收到，有些总是目标0成功0-极光社区](https://community.jiguang.cn/question/185600?fs=1)

参照上述博文，设置别名成功，但是推送失败（获取 token 出错，token 不存在）。

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_890443_7yu_VyBF5fFdtA5L_1635130407%5B1%5D.png)

<hr />

尝试按 `registerID` 推送，能够获取到 `registerID`，但是按 `registerID` 推送失败了。

获取到的 `registerID`

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_187383_6W7sfJTeJicXxpPd_1635129310%5B1%5D.png)

极光后台报错：

错误：没有满足条件的推送目标或推送目标超过 255 天不活跃，被排除在推送目标之外

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_241304_WwFOU1qktgJmTh_X_1635129276%5B1%5D.png)

发送失败(errcode:1011,errmsg:没有满足条件的推送目标)

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_64952_G_f2NBTy8eoyogLp_1635129373%5B1%5D.png)

[Errorcode 1011：cannot find user by this audience，没有满足条件的推送目标-极光社区](https://community.jiguang.cn/article/61611)

[Not get devicetoken，获取不到 devicetoken-极光社区](https://community.jiguang.cn/article/56461)

[react-native 集成极光推送iOS/andriod配置 - SegmentFault 思否](https://segmentfault.com/a/1190000018987000)

参照上述博文，猜测可能是自己的 `JPush_AppKey` 没有进行配置好。

需要更改 `AppDelegate.m` 文件中的相关配置。

文件路径：`ios/MyReactNativeDemo/AppDelegate.m`

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_101176_wrVI1XQiH7aPmHlU_1635133451%5B1%5D.png)

修改为以下内容：

```js
appKey:@"JPush_AppKey" // 在极光后台生成的JPush_AppKey
channel:@"dev"
```

`Archive` 一下，重新安装到手机上。发现能够接收到推送通知。

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16405380272484.png" style="zoom:67%;" />

极光推送集成成功!!!



<hr />

参考博文：

[RN集成极光推送 - 掘金](https://juejin.cn/post/6844904083678035975#heading-5)

[React Native集成极光推送_大灰狼的小绵羊哥哥的博客-CSDN博客](https://blog.csdn.net/sinat_17775997/article/details/81030597)

[GitHub - jpush/jpush-react-native: JPush's officially supported React Native plugin (Android & iOS). 极光推送官方支持的 React Native 插件（Android & iOS）。](https://github.com/jpush/jpush-react-native)