---
title: React Native 0.66 Demo 集成 Code Push（iOS）
date: 2021-12-26 22:23:23
tags: [React Native,iOS,Code Push]
categories: 移动端开发
typora-copy-images-to: upload
---

## 安装 App Center CLI

打开终端执行以下命令：

```shell
# 安装 App Center CLI
npm install -g appcenter-cli
# 登录 App Center
appcenter login
```

<!-- more -->

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_855829_BF7jJ-J4YkgpOzjv_1634870369%5B1%5D.png)



## 在 App Center CLI 中创建应用并生成部署密钥

[React Native 0.66 Demo 集成 Code Push（Android） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/24/React-Native-0-66-Demo-集成-Code-Push（Android）/)

参见上述博文，之前在进行 Android 平台 Demo 构建时，已经创建好了应用，现在只需要将当前操作的应用设置为 `MyReactNativeDemo-iOS` 即可。

```shell
# 查看 appcenter 中创建的应用
appcenter apps list
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_737522_z3SQ3WC9ls_4gOiX_1634870627%5B1%5D.png)

```shell
# 查看当前操作的应用
appcenter apps get-current
# 设置当前操作的应用
appcenter apps set-current 3148441341-qq.com/MyReactNativeDemo-iOS
# 创建两个部署（Staging 和 Production）
appcenter codepush deployment add Staging
appcenter codepush deployment add Production
# 查看两个部署的部署密钥
appcenter codepush deployment list --displayKeys
```

当前 React Native App Demo 应用的两个部署的部署密钥

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_446224_qHrrGdDwflifUvWE_1634871028%5B1%5D.png)



## 集成 Code Push 插件

### 安装 `react-native-code-push`

```shell
# 进入项目目录
cd Documents/MyReactNativeDemo
# 安装 react-native-code-push
npm install --save react-native-code-push
```

### 项目配置

（1）运行以下命令安装所有必要的 `CocoaPods` 依赖项。（需要打开 `VPN`）

```shell
# 安装 CocoaPods 依赖项
cd ios && pod install && cd ..
```

（2）打开 `AppDelegate.m` 文件，并添加如下内容

文件路径：`ios/MyReactNativeDemo/AppDelegate.m`

```js
#import <CodePush/CodePush.h>
```

（3）同样是在 `AppDelegate.m` 文件，找到以下代码行（用于为生产版本设置 bridge 的源 URL）：

```js
return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
```

将其替换为以下代码：

```js
return [CodePush bundleURL];
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_113716_aJd7Z5WYICZ_fnLr_1634872486%5B1%5D.png)

（4）将部署密钥添加到 `Info.plist` 文件中

文件路径：`ios/MyReactNativeDemo/Info.plist`

这里直接使用 `Production: sKXbaBk46P7ito0QKuWKDa4r6B8aONFQccL8_`

```xml
<!-- ios/MyReactNativeDemo/Info.plist -->
<key>CodePushDeploymentKey</key>
<string>sKXbaBk46P7ito0QKuWKDa4r6B8aONFQccL8_</string>
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_9999_jD-YcdXyspWue2KE_1634881898%5B1%5D.png)



### HTTP 异常域配置

同样是在 `Info.plist` 文件，进行如下修改

```xml
<plist version="1.0">
  <dict>
    <!-- ...other configs... -->

    <key>NSAppTransportSecurity</key>
    <dict>
      <key>NSExceptionDomains</key>
      <dict>
        <key>codepush.appcenter.ms</key>
        <dict><!-- read the ATS Apple Docs for available options --></dict>
      </dict>
    </dict>

    <!-- ...other configs... -->
  </dict>
</plist>
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_441536_hPUJi7XatwUZ-syQ_1634873953%5B1%5D.png)



### 代码签名设置（可选） 

[React Native客户端 SDK 入门 - Visual Studio App Center  Microsoft Docs](https://docs.microsoft.com/zh-cn/appcenter/distribution/codepush/rn-get-started)

不影响热更新正常使用。



### 自定义配置（多部署测试）

[Using the React Native SDK with CodePush  – Distribution - Visual Studio App Center  Microsoft Docs](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-deployment)

（1）用 `Xcode` 打开项目

（2）如图所示，选中 `Info` 选项卡

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_284131_7Bzd8sKP0HkGBYG7_1634881844%5B1%5D.png)

（3）如图所示，点击 `Configurations` 部分的 `+` 按钮，然后选择 `Duplicate "Release" `

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_165523_7LOHypKz0H5vAAkC_1634882114%5B1%5D.png)

（4）将新出现的配置项命名为 `Staging`

（5）然后切换到 `Build Settings` 选项卡，点击工具栏上的 `+ ` 按钮，选择 `Add User-Defined Setting`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_220172_9Q8-sdNHOilZZq5b_1634882468%5B1%5D.png)

将新出现的配置项命名为 `MULTI_DEPLOYMENT_CONFIG`，然后给其设置值。

`Release` 设置为 `$(BUILD_DIR)/$(CONFIGURATION)$(EFFECTIVE_PLATFORM_NAME) `

`Staging` 设置为 `$(BUILD_DIR)/Release$(EFFECTIVE_PLATFORM_NAME)`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_137554_oxjlIjDXz8JKINAk_1634882765%5B1%5D.png)

配置结果如下图所示：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_709979_q1rqOZ0MIMVPRKKU_1634882877%5B1%5D.png)

（6）再次点击工具栏上面的 `+` 按钮，选择 `Add User-Defined Setting`

将新出现的配置项命名为 `CODEPUSH_KEY`，然后给其设置值。

`Release` 设置为 `sKXbaBk46P7ito0QKuWKDa4r6B8aONFQccL8_`

`Staging` 设置为 `pnXIXQ4NPcCT3YOAm_a05xp5u8oJbdWtK4JHH`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_917097_Tru3l-kd7GTZRYK6_1634883391%5B1%5D.png)

（7）打开 `Info.plist` 文件，将 `CodePushDeploymentKey` 值改为 `$(CODEPUSH_KEY)`

文件路径：`ios/MyReactNativeDemo/Info.plist`

```xml
<!-- ios/MyReactNativeDemo/Info.plist -->
<key>CodePushDeploymentKey</key>
<string>$(CODEPUSH_KEY)</string>
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_597334_azmf74hk90BJE7Hn_1634883587%5B1%5D.png)



### 在项目中使用 `react-native-code-push`

[React Native 0.66 Demo 集成 Code Push（Android） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/24/React-Native-0-66-Demo-集成-Code-Push（Android）/)

[React Native 集成 CodePush 指南 - 掘金](https://juejin.cn/post/6844904073309716494)

参照上述文章

```js
import React from 'react';
import {View, StyleSheet} from 'react-native';
import codePush from 'react-native-code-push';
import AwesomeButton from 'react-native-really-awesome-button';

const codePushOptions = {checkFrequency: codePush.CheckFrequency.MANUAL};

const App = () => {
  const checkForUpdate = () => {
    codePush.sync({
      updateDialog: true,
      installMode: codePush.InstallMode.IMMEDIATE,
    });
  };

  const clear = () => {
    codePush.clearUpdates();
  };

  return (
    <View style={styles.container}>
      <AwesomeButton type="secondary" onPress={checkForUpdate}>
        检查更新
      </AwesomeButton>
      <AwesomeButton type="secondary" onPress={clear}>
        清除更新
      </AwesomeButton>
    </View>   
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});

// 注意：这是可选的，完全可以不使用 codePush 这里包装
export default codePush(codePushOptions)(App);
```

安装 `react-native-really-awesome-button`

```shell
# 安装 react-native-really-awesome-button
npm install --save react-native-really-awesome-button
```



### 发布更新

（1）打包应用

```shell
# 进入项目目录
cd Documents/MyReactNativeDemo
# 打包应用
npx react-native run-ios --configuration Release
```

打包应用，尝试将应用安装到模拟器时报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_252333_MYBA5GGzbWcyYeTX_1634885059%5B1%5D.png)

Google 了一下

[xcode - Build error domain=com.apple.CoreSimulator.SimError, code=405 - Stack Overflow](https://stackoverflow.com/questions/69312343/build-error-domain-com-apple-coresimulator-simerror-code-405)

参照上述博文，清理掉 `Xcode` 的项目构建数据和索引。

操作路径：`苹果菜单 > 关于本机 > 选择储存空间 > 打开管理 > 选择开发者 > 删除项目构建数据和索引`

重新打包应用，运行 `npx react-native run-ios --configuration Release`

还是报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_745583_8n1BCtsx4oo-uxe1_1634885895%5B1%5D.png)

<hr/>

还是在之前的界面，清理掉 `Xcode` 缓存，重新打包，尝试有没有效果。

还是报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_252333_MYBA5GGzbWcyYeTX_1634885059%5B1%5D.png)

<hr />

尝试在 `Debug` 模式（是指命令行命令）下运行，验证是不是发布模式不允许在模拟器上安装？

执行 `npx react-native run-ios `

试了还是报错。

<hr/>

尝试指定模拟器运行，执行 `npx react-native run-ios --simulator="iPhone 11" `

还是报错。

<hr />

尝试在执行下面的命令之后，再执行 `npx react-native run-ios `

```shell
# 重启所有模拟器
sudo xcrun simctl shutdown all && sudo xcrun simctl erase all
```

还是报错。

<hr />

尝试清空开发者页面 `Xcode` 的所有缓存，包括项目归档。再执行 `npx react-native run-ios `。

还是报错。

<hr/>

可能是在配置 ”release scheme” 时将下拉的 `Build Configuration` 设置为了 `Release` 的缘故，将其改为 `Debug`，再次尝试。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_74303_GojiDobvCSikSo1o_1634817521%5B1%5D.png)

还是报错。没有其他办法了，暂时放弃，使用 `Xcode` 来进行应用打包。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_252333_MYBA5GGzbWcyYeTX_1634885059%5B1%5D.png)

[React Native 0.66 Demo 集成 Code Push（Android） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/24/React-Native-0-66-Demo-集成-Code-Push（Android）/)

参照上述博文中的 5.5、5.6 步骤，打包成功！

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_978554_28sdVuVxnte2KHH0_1634890606%5B1%5D.png)

然后参照 5.7 内测分发步骤，将打包好的应用安装到手机上。

（2）发布第一次更新

```shell
# 进入项目文件夹
cd Documents/MyReactNativeDemo
# 发布更新（Staging 模式部署）
appcenter codepush release-react -m --description "第一次热更新"
# 升级至 Production 模式部署
appcenter codepush promote -s Staging -d Production
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_686270_8l9Lov5A69pXH9rW_1634894390%5B1%5D.png)

发布成功!!!

（3）在手机上查看（检查更新）是否有更新

在手机上点击“检查更新”按钮，没有看到更新的弹窗。

<hr />

更改 `App.js`

```html
<View style={styles.container}>
      <AwesomeButton type="secondary" onPress={checkForUpdate}>
        检查更新
      </AwesomeButton>
      <!--添加“清除更新”按钮-->
      <AwesomeButton type="secondary" onPress={clear}>
        清除更新
      </AwesomeButton>
</View> 
```

通过内测分发安装打包好的应用，然后再次发布热更新。

```shell
# 发布更新（Staging 模式部署）
appcenter codepush release-react -m --description "第二次热更新"
```

在手机上点击“检查更新”按钮，没有看到更新弹窗。

升级至 `Production` 模式部署

```shell
# 升级至 Production 模式部署
appcenter codepush promote -s Staging -d Production
```

在手机上再次点击“检查更新”按钮 ，可以看到有更新弹窗

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_741644_do3lQHgxaCDFmkI9_1634897246%5B1%5D.png" style="zoom:25%;" />

点击 Continue 完成热更新之后，可以看到按钮居中了。

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_707603_uxjWfDGP-MHgedyD_1634897318%5B1%5D.png" style="zoom:25%;" />



集成 Code Push 成功 !!!



<hr />

参考博文：

[使用 CodePush 更新应用程序 - Visual Studio App Center  Microsoft Docs](https://docs.microsoft.com/zh-cn/appcenter/distribution/codepush/)

[React Native客户端 SDK 入门 - Visual Studio App Center  Microsoft Docs](https://docs.microsoft.com/zh-cn/appcenter/distribution/codepush/rn-get-started)

[Ionic5.x+Capacitor+Vue3.x 集成 Code Push（Android） - Admvli2016’s Blog](https://admvli2016.github.io/2021/12/12/Ionic5-x-Capacitor-Vue3-x-集成-Code-Push（Android）/)

[React Native 0.66 Demo 集成 Code Push（Android） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/24/React-Native-0-66-Demo-集成-Code-Push（Android）/)

