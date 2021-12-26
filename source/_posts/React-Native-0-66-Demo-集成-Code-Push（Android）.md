---
title: React Native 0.66 Demo 集成 Code Push（Android）
date: 2021-12-24 23:33:44
tags: [React Native,Android,Code Push]
categories: 移动端开发
typora-copy-images-to: upload
---

## 安装 App Center CLI

```shell
# 安装 App Center CLI
npm install -g appcenter-cli
# 登录 App Center
appcenter login
```

<!--more-->

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_310739_E5kwkX6VYf21W-pb_1634177525%5B1%5D.png)



## 在 App Center CLI 中创建应用并生成部署密钥

```shell
# 使用 appcenter cli 在 appcenter 中创建两个 app 应用
appcenter apps create -d MyReactNativeDemo-Android -o Android -p React-Native
appcenter apps create -d MyReactNativeDemo-iOS -o iOS -p React-Native

# 查看 appcenter 中创建的应用
appcenter apps list

# 设置当前操作的应用
appcenter apps set-current 3148441341-qq.com/MyReactNativeDemo-Android

# 查看当前操作的应用
appcenter apps get-current

# 创建两个部署（ Staging 和 Production ）
appcenter codepush deployment add Staging  
appcenter codepush deployment add Production

# 查看两个部署的部署密钥
appcenter codepush deployment list --displayKeys
```

当前 React Native App Demo 应用的两个部署的部署密钥

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_897404_szX8CvAnozLcxdRO_1634177618%5B1%5D.png)



## 集成 Code Push 插件

### 安装 `react-native-code-push`

```shell
# 进入项目文件夹
cd MyReactNativeDemo
# 安装 react-native-code-push
npm install --save react-native-code-push
```



### 项目配置

（1）在 `android/settings.gradle` 文件中添加以下内容

```js
include ':app', ':react-native-code-push'
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
```

（2）在 `android/app/build.gradle` 文件中添加以下内容，注意顺序

```js
...
apply from: "../../node_modules/react-native/react.gradle"
apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"
...
```

（3）在 `MainApplication.java` 文件中进行如下修改

文件路径：`android\app\src\main\java\com\myreactnativedemo\MainApplication.java`

```java
...
// 1. Import the plugin class.
import com.microsoft.codepush.react.CodePush;
public class MainApplication extends Application implements ReactApplication {
    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        ...
        // 2. Override the getJSBundleFile method to let
        // the CodePush runtime determine where to get the JS
        // bundle location from on each app start
        @Override
        protected String getJSBundleFile() {
            return CodePush.getJSBundleFile();
        }
    };
}
```

（4）将部署密钥添加到 `strings.xml`

文件路径：`android\app\src\main\res\values\strings.xml`

这里直接使用 `Production: A1A3V3vVS0seesi2d8b1e13NVo2ftPx1Xb23q`

```xml
<!-- android\app\src\main\res\values\strings.xml -->
<resources>
    <string name="app_name">MyReactNativeDemo</string>
    <string moduleConfig="true" name="CodePushDeploymentKey">A1A3V3vVS0seesi2d8b1e13NVo2ftPx1Xb23q</string>
</resources>
```



### 代码签名设置（可选）

[使用 App Center CLI 发布 CodePush 更新 - Visual Studio App Center  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/cli#code-signing)

不影响热更新正常使用。



### 自定义配置（多部署测试）

[将 React Native SDK 与 CodePush 结合使用 - 分发 - Visual Studio 应用中心  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-deployment)

打开 `android/app/build.gradle` 文件，进行以下修改

```js
android {
    ...
    buildTypes {
        debug {
            ...
            // Note: CodePush updates shouldn't be tested in Debug mode as they're overriden by the RN packager. However, because CodePush checks for updates in all modes, we must supply a key.
            resValue "string", "CodePushDeploymentKey", '""'
            ...
        }
        releaseStaging {
            ...
            resValue "string", "CodePushDeploymentKey", '"wAOCubam2DmxXVUdldVFkAjKPuzIU7zz2k8A1"'
            // Note: It's a good idea to provide matchingFallbacks for the new buildType you create to prevent build issues
            // Add the following line if not already there
            matchingFallbacks = ['release']
            ...
        }
        release {
            ...
            resValue "string", "CodePushDeploymentKey", '"A1A3V3vVS0seesi2d8b1e13NVo2ftPx1Xb23q"'
            ...
        }
    }
    ...
}
```

配置如下图所示

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_517799_HaHkHwvSvGHos-Lw_1634178575%5B1%5D.png)

记得要注释掉 3.2 中`strings.xml` 的配置

```xml
<!-- android\app\src\main\res\values\strings.xml -->
<resources>
    <string name="app_name">MyReactNativeDemo</string>
   <!-- <string moduleConfig="true" name="CodePushDeploymentKey">A1A3V3vVS0seesi2d8b1e13NVo2ftPx1Xb23q</string>
   -->
</resources>
```



### 在项目中使用 `react-native-code-push`

[React Native Client SDK 插件使用 - Visual Studio App Center  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-plugin)

[使用 CodePush API 参考 React Native SDK - Visual Studio 应用中心  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-api-ref)

[React Native 集成 CodePush 指南 - 掘金](https://juejin.cn/post/6844904073309716494)

参照上述文章

```js
// App.js
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
    <View>
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

[发布更新 - Visual Studio 应用中心  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-updates)

使用 appcenter cli 发布更新， 在此之前通过 appcenter login 登录 App Center。

```shell
# 发布更新-原命令
appcenter codepush release-react -a <ownerName>/<appName>
# 设置当前操作的应用之后
appcenter codepush release-react
# 可以添加发布描述
appcenter codepush release-react -m --description "第一次热更新"
```

（1）打包应用

```shell
# 打开 android 文件夹
cd android
# 打包 release 应用
./gradlew assembleRelease
```

打包失败

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_947469_mucS8RgZGBMBUhDI_1634179016%5B1%5D.png)

猜测是因为 `android\settings.gradle` 的配置问题，更改配置如下：

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_267893_1MdTXMw_5IVX3Mu3_1634179036%5B1%5D.png)

再次尝试打包，还是失败。

猜测是因为 `android\app\build.gradle` 的配置问题，更改配置如下：

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_268461_rf_qQwYlgXqzHoIT_1634179057%5B1%5D.png)

再次尝试打包

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_699822_mOicEafb2V4TS3Nr_1634179078%5B1%5D.png)

打包成功，确实是 `android\app\build.gradle` 的配置问题，只引入下面的文件即可，不必和文档上一样引入两个文件。

（2）发布第一次更新

```shell
# 进入项目文件夹
cd D:\ShenGuYun\Source\gitee\MyReactNativeDemo
# 发布更新
appcenter codepush release-react -m --description "第一次热更新"
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_912094_t0nsyxRy25neqcL4_1634179116%5B1%5D.png)

发现发布的是 Staging 模式部署，升级发布 Production 模式部署

```shell
# 原命令
appcenter codepush promote -a <ownerName>/MyApp -s Staging -d Production
# 简化之后
appcenter codepush promote -s Staging -d Production
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_28862_HUniZYy7LQOsH7nb_1634179171%5B1%5D.png)

**成功发布!!!**

（3）在手机上查看（检查更新）是否有更新

确实有更新提示

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_682183_9PG7sBBA1obmSgJQ_1634179210%5B1%5D.jpg" style="zoom:25%;" />

点击 CONTINUE 手机会自动应用最新的热更新包，并刷新页面。

（4）修改 App.js 代码，应用打包，再次发布更新

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_908210_Mf0qYXG_mx_geE1Q_1634179232%5B1%5D.png)

执行热更新命令

```shell
appcenter codepush release-react -m --description "第二次热更新"
appcenter codepush promote -s Staging -d Production
```

应用热更新之前

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_446273_LG9jA8SZr9nrYjUe_1634179270%5B1%5D.jpg" style="zoom:25%;" />

应用热更新之后

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_569130_yzGJtTA1KvqBTQz2_1634179281%5B1%5D.jpg" style="zoom:25%;" />

集成 Code Push 成功 !!!



<hr />

参考博文：

[使用 CodePush 实时更新您的应用程序 - Visual Studio App Center  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/)

[React Native Client SDK 入门 - Visual Studio App Center  微软文档](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-get-started)

[Ionic5.x+Capacitor+Vue3.x 集成 Code Push（Android） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/12/Ionic5-x-Capacitor-Vue3-x-集成-Code-Push（Android）/)



