---
title: React-Native 0.66 Demo 集成极光推送（Android）
date: 2021-12-25 11:27:14
tags: [React Native,Android,极光推送]
categories: 移动端开发
typora-copy-images-to: upload
---

当前 Demo 的 AppId 是 com.myreactnativedemo，公司已有项目（开通了极光推送服务）的 AppId 为 com.xxxxxxx.xxxxxx

参照 [React Native 0.66 Demo 搭建全过程（Android） - Admvli2016's Blog](https://admvli2016.github.io/2021/12/24/React-Native-0-66-Demo-搭建全过程（Android）/)

重新创建项目设置 AppId 为 com.xxxxxxx.xxxxxx

```shell
npx react-native init com.xxxxxxx.xxxxxx
```

创建失败。

<!--more-->

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16404000134778.png)

改变策略，先尝试集成极光推送插件，若无法集成，再尝试参照下述博文更改 AppId (包名) 

[在React Native中更改Android的软件包名称](https://qastack.cn/programming/37389905/change-package-name-for-android-in-react-native)



## 安装 jcore-react-native 、jpush-react-native

[jpush/jpush-react-native: JPush's officially supported React Native plugin (Android & iOS). 极光推送官方支持的 React Native 插件（Android & iOS）。](https://github.com/jpush/jpush-react-native)

参照官方文档

```shell
# 安装 jcore-react-native
npm install --save jcore-react-native
# 安装 jpush-react-native
npm install --save jpush-react-native
```

安装完成后连接原生库，进入项目目录执行

```shell
npx react-native link jpush-react-native
npx react-native link jcore-react-native
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_452879_rH1NM0QDI1FqXWGn_1634197484%5B1%5D.png)



## 项目配置

（1）`android\app\build.gradle` 添加如下内容：

```js
android {
      defaultConfig {
          // 在此替换你的应用包名
          applicationId "com.xxxxxxx.xxxxxx" 
          ...
          manifestPlaceholders = [
                   // 在此替换你的 APPKey
                  JPUSH_APPKEY: "xxxxxxxx",  
                  // 在此替换你的 channel   
                 JPUSH_CHANNEL: "developer-default"  
          ]
      }
}
```

```js
dependencies {
      ...
      // 添加 jpush 依赖
      implementation project(':jpush-react-native') 
      // 添加 jcore 依赖 
      implementation project(':jcore-react-native')  
}
```

这里的 applicationId 设置为已开通极光推送服务的 AppId -- com.xxxxxxx.xxxxxx，尝试是否能接收到推送。

（2）`android\settings.gradle` 添加如下内容：

```js
include ':jpush-react-native'
project(':jpush-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jpush-react-native/android')
include ':jcore-react-native'
project(':jcore-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/jcore-react-native/android')
```

如下图所示

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_316510_R6AQ6rbQmHQGM0qz_1634197677%5B1%5D.png)

（3）在 `AndroidManifest.xml` 文件中添加以下内容：

文件路径：`android\app\src\main\AndroidManifest.xml`

```xml
<meta-data android:name="JPUSH_CHANNEL"
	android:value="${JPUSH_CHANNEL}"/>
<meta-data android:name="JPUSH_APPKEY"
	android:value="${JPUSH_APPKEY}" />
```

如下图所示

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_133500_EG2m6q7miqJ21hom_1634197828%5B1%5D.png)

（4） 修改 `MainApplication.java` 文件

文件路径：`android\app\src\main\java\com\myreactnativedemo\MainApplication.java`

进行如下修改：

```java
// 1.
+ import cn.jiguang.plugins.push.JPushModule;

// 2.
 @Override
  public void onCreate() {
    super.onCreate();
    SoLoader.init(this, /* native exopackage */ false);
    // 调用此方法：点击通知让应用从后台切到前台
   JPushModule.registerActivityLifecycle(this);
  }
```



## 在项目中使用 jpush-react-native

直接使用官方的 App.js 例子

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



## 打包项目并安装

（1）打包项目

```shell
# 打开 android 文件夹
cd android
# 打包 release 应用
./gradlew assembleRelease
```

打包成功！

（2）将 APK 安装到真机上

> 注意：**一定要打开手机的通知权限！**

安装到手机上时报运行异常错误

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_123527_SeFi7D-gFJ9MVjqy_1634198025%5B1%5D.jpg" style="zoom:25%;" />

调试一下查看错误原因。

报错提示：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_192188_o42Q0YMJf8drTYx6_1634198048%5B1%5D.png)

这个之前遇见过，是因为将 applicationId 设置为了 "com.xxxxxxx.xxxxxx"，先改回来看一看有没有其他的报错。

报错提示：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_607937_FuGfkgdloLN258WF_1634198073%5B1%5D.png)

`TypeError: _jpushReactNative.default.addCustomMessagegListener is not a function.`

找到原因：官方提供的 App.js 文件中 addCustomMessageListener 拼写错了，Message 后面多一个 g

将 applicationId 改回 "com.xxxxxxx.xxxxxx"，再次打包，并安装到手机上。结果可以正常使用。

（3）设置别名

> 注意：**一定要打开手机的通知权限！**

点击 setAlias 按钮，给该 App 设置别名

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_844182_DwSpK58Pa9IdhL7__1634198112%5B1%5D.jpg" style="zoom:25%;" />



## 验证极光推送是否集成成功

在极光推送平台上模拟推送通知，查看手机是否能够接收到通知。

尝试模拟推送通知，但手机上未接收到推送通知。

原因：突然发现刚才打包的应用的 applicationId 为 com.myreactnativedemo，没有改过来，改回 com.xxxxxxx.xxxxxx 后 ，再重新打包，并安装到手机上。结果可以接收到通知！

> 注意：**一定要打开手机的通知权限！**

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_207992_qafwlxJcsK6ijFAp_1634198193%5B1%5D.jpg" style="zoom:25%;" />

极光推送集成成功!!!