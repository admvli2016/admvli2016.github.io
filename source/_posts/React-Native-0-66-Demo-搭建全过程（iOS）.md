---
title: React Native 0.66 Demo 搭建全过程（iOS）
date: 2021-12-25 17:21:21
tags: [React Native,iOS]
categories: 移动端开发
typora-copy-images-to: upload
---

## 开发环境搭建（iOS）

本文 Demo 是在 `iMac` 上完成搭建的。

[搭建开发环境 · React Native 中文网](https://reactnative.cn/docs/environment-setup)

参照上述博文，需要安装的软件

```
Node          要求 v12 以上版本  
Watchman      stable 2021.10.18.00 (bottled)
Xcode         Version 13.0 (13A233)
CocoaPods     stable 1.11.2 (bottled)
```

<!-- more -->

[怎么查看 mac 是否安装了cocoapods_百度知道](https://zhidao.baidu.com/question/812214638379200812.html)

```shell
# 查看 CocoaPods 的版本号
pod --version
```

Mac 操作系统环境现状

```
Node         已安装 v8.11.3
Watchman     未安装 
Xcode        已安装 Version 13.0 (13A233)
CocoaPods    未安装   
```



### 安装 `Homebrew`

 [The Missing Package Manager for macOS (or Linux) — Homebrew](https://brew.sh/)

参照上述博文

```shell
# 安装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装失败。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_110930_2H4Zn8oeNqBPEm0M_1634724494%5B2%5D.png)

<hr/>

[MacOs M1安装Homebrew 在国内最简单方法_YD-10-NG的博客-CSDN博客](https://blog.csdn.net/sinat_38184748/article/details/114115441)

参照上述博文，重新安装。

```shell
# 主机是 intel 芯片
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

安装成功。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_684570_d49PN2YGVTay--PF_1634725347%5B2%5D.png)



### 安装 watchman

```shell
brew install watchman
```

安装失败，报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_624944_tpyXdOWG6r_nZkZB_1634725875%5B2%5D.png)

按照提示，尝试安装 python@3.9

```shell
brew install --build-from-source python@3.9
```

安装完成，再次运行 `brew install watchman`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_317438_Pvc1-SWA4b_Qn3D0_1634787753%5B2%5D.png)

安装成功！

`brew info watchman` 查看 `Homebrew` 安装的 watchman 详细信息。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_39027_MlT8_CgJZvj_S5eA_1634795206%5B2%5D.png)

```shell
# 验证是否安装成功
watchman --help
```



### 安装 `CocoaPods`

```shell
brew install cocoapods
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_103791_nybSG77OZ9M-Qcrr_1634795644%5B1%5D.png)

安装成功！

`brew info cocoapods` 查看 `Homebrew` 安装的 `cocoapods` 详细信息

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_196360_25HDZxTQByhU8KsZ_1634795727%5B1%5D.png)



### 安装 Node

可参照下面的博文安装 `nvm`，通过 `nvm` 来安装 node 、管理 node 版本

[nvm 安装与使用 - Admvli2016’s Blog](https://admvli2016.github.io/2021/12/08/nvm-安装与使用/#more)



### 安装 Yarn

```shell
# 安装 Yarn
npm install -g yarn
```



## 创建 React Native 项目

```shell
# 进入文稿目录
cd ~/Documents
# 创建新项目
npx react-native init MyReactNativeDemo --template react-native-template-typescript
```

创建 React Native 项目失败，报错:  Unexpected token {

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_275783__WmBZPIXQh1RmqeM_1634806627%5B1%5D.png)

估计是因为 node 的版本问题，切换到 `v12.22.0`。

发现确实是 node 的版本问题。

卡在 `CocoaPods` 安装依赖，需要 `VPN`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_333049_pnwQtcAmgDTUDA5S_1634807293%5B1%5D.png)

`CocoaPods` 安装依赖完成

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_245866_my-80B6oO7BS2NwV_1634807922%5B1%5D.png)



## 运行 React Native 项目

### 运行项目（模拟器）

```shell
# 进入项目目录
cd MyReactNativeDemo
# 运行 yarn ios
yarn ios
```

可以指定特定的模拟器运行

```shell
npx react-native run-ios --simulator "iPhone 4s"
```

可以在终端中运行 `xcrun simctl list devices` 来查看具体可用的设备名称。

很快可以看到 iOS 模拟器自动启动并运行项目。

在正常编译完成后，开发期间要保持 Metro 命令行窗口运行而不要关闭。以后需要再次运行项目时，

如果没有修改过 ios 目录中的任何文件，则只需单独启动 `yarn start` 命令。如果对 ios 目录中任何

文件有修改，则需要再次运行 `yarn ios` 命令完成原生部分的编译。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404142713104.png)

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_765539_947CzkuavglSPw6Z_1634808601[1].png" style="zoom:25%;" />

初次运行，启动时间较长，请耐心等待。

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_502098_Qm8Cd65IDdfILhOP_1634812088%5B1%5D.png" style="zoom:25%;" />

### 运行项目（真机）

[在设备上运行 · React Native 中文网](https://reactnative.cn/docs/running-on-device)

参照上述博文

（1）通过 `USB` 数据线连接设备

（2）用 `Xcode` 打开 `MyReactNativeDemo.xcworkspace`  文件

（3）如果这是第一次在 iOS 设备上运行 `APP`，需要注册开发设备。从 `Xcode` 菜单栏打开 Product 菜单，然后前

往 Destination。从列表中查找并选择设备，将其注册为开发设备。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_973097_4hN79CxZBG_hxPGs_1634974249%5B1%5D.png)

（4）配置代码签名

如图所示，点击 `MyReactNativeDemo`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_594888_RSj5c2ujmea5oVvw_1634814717%5B1%5D.png)

编辑 General 目录，将 `Bundle Identifier` 改为 `com.xxxxxxx.xxxxxx`，并进行以下更改。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_1640415176187.png)

（5）编译并运行应用

操作路径：`Product > Run`

`App` 将会被自动安装到真机上。

这一步的报错信息：

```shell
Unable to install "因思云"
Domain: com.apple.dt.MobileDeviceErrorDomain
Code: -402620394
User Info: {
    IDERunOperationFailingWorker = IDEInstalliPhoneLauncher;
}
--
The executable was signed with invalid entitlements.
```

[ iOS开发-真机调试遇到“The executable was signed with invalid entitlements.”  zhangzr's blog ](http://zhangzr.cn/2018/12/17/iOS开发-真机调试遇到"The-executable-was-signed-with-invalid-entitlements-"/)

参照上述博文，**将 Run 的模式 (Scheme) 由 Release 改为 Debug**，还是报错了。

```shell
Could not launch “因思云”
Domain: IDEDebugSessionErrorDomain
Code: 3
Failure Reason: failed to get the task for process 403
User Info: {
    DVTRadarComponentKey = 855031;
    IDERunOperationFailingWorker = DBGLLDBLauncher;
    RawUnderlyingErrorMessage = "failed to get the task for process 403";
}
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_475154_0Qt9jjlST8JU91s4_1634975637%5B1%5D.png)

可能是自己点击手机 `APP` 点早了，重试一次。

```shell
Could not launch “因思云”
Domain: IDEDebugSessionErrorDomain
Code: 3
Failure Reason: failed to get the task for process 282
User Info: {
    DVTRadarComponentKey = 855031;
    IDERunOperationFailingWorker = DBGLLDBLauncher;
    RawUnderlyingErrorMessage = "failed to get the task for process 282";
}
```

还是不行。

[Xcode7.3真机调试 报错:process launch failed: failed to get the task for process XXXX - 简书](https://www.jianshu.com/p/14ff10e453d2)

[Xcode failed to get the task for process XXX - 简书](https://www.jianshu.com/p/9b7ae6891ff7)

参照上述博文，真机调试时的证书必须是开发证书，所以还需要去配置开发证书。

<hr />

[极光推送 - iOS 证书设置指南 - 极光文档](https://docs.jiguang.cn/jpush/client/iOS/ios_cer_guide/)

参照上述文档，只需要重新生成一个 Developer 环境的 Provisioning Profile 文件即可。

生成之后，再在 `Xcode` 上面进行配置。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404198513622.png)

再次尝试真机联调（开发环境），安装到手机上成功！

> 注意记录一下 Developer 的钥匙串访问密码 ：`xx1234` （可能需要输入几次）



### 修改项目代码 

（1）使用 VS Code 打开 App.js 并进行修改

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_856529_G6z9AwQIjOS2ZPPV_1634812418%5B1%5D.png)

（2）在 VS Code 中按 Ctrl + S 保存，就可以看到最新的修改结果。

修改结果：

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_753314_lQbFpjIXrFxXQi5c_1634812438%5B1%5D.png" style="zoom:25%;" />



## 调试 React Native 项目

### 在 iOS 设备上运行应用

参照 3.2 中的步骤即可。



### 从设备上访问开发服务器

> 注意：调试时手机和电脑都必须在同一WIFI环境下，电脑不要连接网线 !

[React Native iOS 真机调试 - 简书](https://www.jianshu.com/p/25719e3102fa)

参照上述博文，在 Mac 上查看本机 IP 地址

```shell
# 在 Mac 上查看本机 IP 地址
ifconfig
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_177186_3vwIXyWfAq8Urvll_1635143874%5B1%5D.png)

断开 iMac 网线，然后连接 wifi

[苹果电脑如何连接网络wifi-百度经验](https://jingyan.baidu.com/article/948f592432c901990ef5f943.html)

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_863411_MT736lWYk2N2hcjA_1635143838%5B1%5D.png)

发现在打开的 Packager 页面中按 d 唤出开发菜单，手机并没有反应；但是摇一摇调试设备却可以打开开发菜单。

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_181541_V7rp_IUj2mQQwOdV_1635144249%5B1%5D.png" style="zoom:25%;" />

点击 `Debug with Chrome` 时，出现了如下的报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404189902782.png)

猜测可能是 `Xcode` 缓存的原因，因为我点击 Run 的时候还没有拔网线。

再次尝试，运行项目并安装到手机上，打开开发菜单，点击 `Debug with Chrome` ，结果成功了。

它将会自动在 Chrome 浏览器中新开一个标签页，内容如下所示：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_992939_OwuNzeCBKW84H6fI_1635144581%5B1%5D.png)

如此同时，手机上的 `App` 将会重新加载页面并刷新。



## React Native 项目打包上架

上架应用的过程和其它原生的 iOS 应用一样。

用 `Xcode` 打开项目 ios 文件夹下的 `MyReactNativeDemo.xcworkspace` 文件。

如图所示点击 `MyReactNativeDemo`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_594888_RSj5c2ujmea5oVvw_1634814717%5B1%5D.png)

### 编辑 General 目录

将 `Bundle Identifier` 改为 `com.xxxxxxx.xxxxxx`

并进行以下更改

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_1640415176187.png)



### 编辑 Signing & Capabilities 目录

`Team` 选择 `Shenyang Blower Works Group M &C Tech. Co, Ltd.`

并进行以下更改

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404199511722.png)

添加 `Background Modes`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_689204_eNBcoju4gB4Yza4U_1634815925%5B1%5D.png)

添加 `Push Notifications`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_331913_F4Op4LxAMtM7_186_1634816033%5B1%5D.png)



### 编辑 Info 目录

将 `Localization native development region` 值改为 `China`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_408302_V94k1Yr0aQHOjTzW_1634816204%5B1%5D.png)

将  `App Transport Security Settings > Exception Domains` 下的 `localhost` 移除



### 编辑 Build Settings 目录

将 `ProductName` 值改为 `因思云`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_837601_lmbtf4CEfdx1FPV3_1634816350%5B1%5D.png)



### 配置 release scheme

配置 `App` 使用 `Release` scheme 编译，前往 `Product > Scheme > Edit Scheme`。选择侧边栏的 Run 标签，然后设置下拉的 `Build Configuration` 为 `Release`。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_74303_GojiDobvCSikSo1o_1634817521%5B1%5D.png)



### 打包应用

从菜单栏选择 `Product > Build` 编译发布 `App`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_1640420516809.png)

另外还可以使用 React Native CLI  发布，执行以下命令

```shell
npx react-native run-ios --configuration Release
```



### 内测分发

使用 `蒲公英` 进行内测分发

[蒲公英-免费的苹果ios应用app内测分发托管|android安卓app内测分发托管](https://www.pgyer.com/)

如果之前没有创建过应用，需要自己手动去创建。

内测分发步骤如下：

（1）找到 `项目名称.xcodeproj` 或者 `项目名称.xcworkspace` 文件，使用 `Xcode` 打开

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_475214_elxMAUxG4t5_hwUL_1628585941%5B1%5D.png)

（2）选择 `Archive`

> 确保此时 `Signing & Capabilities > Signing（Release）> Provisioning Profile` 选择的是 `Ad Hoc`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404220184510.png)

（3）成功后点击 `Distribute App` 按钮

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_696035_8J5za4Z-GpBTJPSE_1628586356%5B1%5D.png)

（4）选择 `Ad Hoc`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_300005_4fnQ5UeGgii2jQFU_1628586308%5B1%5D.png)

（5）直接点击 Next

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_942091_E-q64f39bBkjmzUY_1628586421%5B1%5D.png)

（6）`项目名称.app` 处选择 `Ad Hoc`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_433652_vVdq5tzS6q9bobLW_1628586476%5B1%5D.png)

（7）最后选择 Export ，将文件导出到桌面

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404226063798.png)

（8）登录蒲公英网站 

[蒲公英-免费的苹果ios应用app内测分发托管|android安卓app内测分发托管](https://www.pgyer.com/)

（9）进入应用管理，选择 `发布/更新应用`，点击`更新`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404227888387.png)

（10）点击立刻上传

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_146669_wc5FbVMZVTAVOWwF_1628588272%5B1%5D.png)

（11）选择之前导出的 ipa 文件

选择之前导出的文件夹，导出来的文件夹都是有时间戳的，选择最近时间的文件夹即可。

然后选择该文件夹中的 `项目名称.ipa` 文件。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_595217_mml4cVQFhtV-Bj3L_1628588503%5B1%5D.png)

等待上传完成后，再进行下一步。

（12）点击 `发布应用`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_889561_Z_8M3ZWIwJz831YT_1628588761%5B1%5D.png)

（13）发布完应用之后，页面会自动跳转回 `发布/更新应用` 页面

点击版本列表中最上面的一项（刚才上传的），手机扫描二维码即可下载安装对应版本 `APP`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404227888387.png)

（14）跳转到当前页面

手机扫描二维码，输入密码 sg1234，即可下载安装对应版本 `APP`

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_303515_S-UCttOH0-xxxDEk_1628589931%5B1%5D.png" style="zoom: 33%;" />

安装后 `APP` 运行截图

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_247779_M5tn_OTwB83l5fhi_1634819467%5B1%5D.png" style="zoom:25%;" />

