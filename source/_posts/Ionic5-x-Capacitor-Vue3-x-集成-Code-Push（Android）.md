---
title: Ionic5.x+Capacitor+Vue3.x 集成 Code Push（Android）
date: 2021-12-12 00:39:22
tags: [Ionic5.x,Capacitor,Vue3.x,Android,Code Push]
categories: 移动端开发
---

## App Center 注册用户

[App Center](https://appcenter.ms/signup?utm_source=CodePush&utm_medium=Azure)

访问 App Center，注册一个账户

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_592203_Kb28w5ED_rpJotTj_1627888914%5B1%5D.png)

<!--more-->

使用 GitHub 创建，点击 Authorize App Center

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_148227_-PRQbQbvYO1g6pLB_1627888971%5B1%5D.png)

输入密码，然后选择用户名（Choose username）

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_421944_3vOntH5GPFopIEPI_1627889001%5B1%5D.png)

进入之后的界面

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_213532_2wSj9hGHPf8ZXF0Q_1627889025%5B1%5D.png)

## 本地 Code Push 环境配置

参考博文

https://docs.microsoft.com/en-us/appcenter/distribution/codepush/

https://docs.microsoft.com/en-us/appcenter/distribution/codepush/cli

[React Native\]使用App Center CLI发布CodePush更新--iOS简易版_weixin_33873846的博客-CSDN博客](https://blog.csdn.net/weixin_33873846/article/details/91477960)

按照入门说明进行如下配置

2.1 安装 appcenter-cli

```shell
# 安装 appcenter-cli
npm install -g appcenter-cli
# 登录 appcenter
appcenter login
```

appcenter-cli 的 login 命令会打开浏览器，要求用户在浏览器上进行认证，认证完成后会生成一个 access key ，粘贴这个 key 到命令行，完成认证

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_700630_Rs5WeDvIt-fXdg22_1627889258%5B1%5D.png)

2.2 管理 App

```shell
# 使用 appcenter-cli 在 appcenter 中创建两个 app 应用
appcenter apps create -d IonicDemo-Android -o Android -p Cordova
appcenter apps create -d IonicDemo-IOS -o iOS -p Cordova

# 查看 appcenter 中创建的应用
appcenter apps list

# 重命名 app
appcenter apps update -n <newName> -a <ownerName>/<appName>

# 删除 app
appcenter apps delete -a <ownerName>/<appName>

# 设置当前操作应用
appcenter apps set-current 3148441341-qq.com/IonicDemo-Android

# 这样可以缩短命令，如下：
# 缩短后
appcenter codepush deployment list --displayKeys
# 原命令
appcenter codepush deployment list --displayKeys -a <ownerName>/<appName> 

# 查看当前操作的应用
appcenter apps get-current

# 手动创建两个部署（ Staging 和 Production ）
# 缩短后
appcenter codepush deployment add Staging  
appcenter codepush deployment add Production
# 原命令
appcenter codepush deployment add -a <ownerName>/<appName> Staging
appcenter codepush deployment add -a <ownerName>/<appName> Production

# 访问两个部署的部署密钥
# 缩短后
appcenter codepush deployment list --displayKeys
# 原命令
appcenter codepush deployment list --displayKeys -a <ownerName>/<appName>
```

当前 App 应用的两个部署的部署密钥


![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_276195_Si_8lN9iwAAvJ3y2_1627889423%5B1%5D.png)

## 在项目中集成 Code Push

3.1 安装 capacitor-codepush

```shell
# 安装 capacitor 插件
npm i @capacitor-community/http @capacitor/deivce @capacitor/dialog @capacitor/filesystem
# 安装 capacitor-codepush  
npm i https://github.com/mapiacompany/capacitor-codepush 
# 同步到 android / ios platform
ionic cap sync
```

3.2 在 capacitor.config.json 中配置 deployment key

capacitor.config.json 配置模板

```json
//  capacitor.config.json
"Plugins": {
    ... (other plugins)
    "CodePush": {
      "IOS_DEPLOY_KEY": "IOS_DEPLOYMENT_KEY",
      "IOS_PUBLIC_KEY": "APP_SECRET_KEY",
      "ANDROID_DEPLOY_KEY": "ANDROID_DEPLOYMENT_KEY",
      "ANDROID_PUBLIC_KEY": "APP_SECRET_KEY",
      "SERVER_URL": "https://codepush.appcenter.ms/"
    }
}
```

+ 如果只需要配置 Android 或者 iOS 其中一个平台的话，另一个平台的配置不需要写
+ 如果 Release 时不需要使用公私钥认证的话，PUBLIC_KEY 的配置也不需要写

配置之后的 capacitor.config.json

```json
// capacitor.config.json
{
  "appId": "com.ionicframework.xxxxxx",
  "appName": "因思工业服务",
  "webDir": "dist",
  "bundledWebRuntime": false ,
  "Plugins": {
    "CodePush": {
      "ANDROID_DEPLOY_KEY": "X1vN32baDusvL1nVB15I9erm_9NRrBc_-YvwA",
      "SERVER_URL": "https://codepush.appcenter.ms/"
    }
  }
}
```

3.3 配置 CSP ，确保 App 能够访问到 Code Push 的服务器

在 index.html 中，添加 Content-Security-Policy 的 meta 标签

```html
<!--index.html-->
<meta http-equiv="Content-Security-Policy" content="default-src https://codepush.appcenter.ms 'self' data: gap: https://ssl.gstatic.com 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src *" />
```

3.4 添加示例代码

```html
<!-- App.vue -->
<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script lang="ts">
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import { defineComponent, onMounted } from 'vue';
import Jpush from "@/utils/jpush";
import { SplashScreen } from '@capacitor/splash-screen';
import { codePush } from'capacitor-codepush';
export default defineComponent({
  name: 'App',
  components: {
    IonApp,
    IonRouterOutlet
  },
  setup() {
     onMounted(() => {
      // 由于是cordova插件，需要在deviceready回调后才能使用，用过Cordova的都懂
      document.addEventListener('deviceready', () => {
        new Jpush().setAlias('190395486111268864');
        codePush.sync({ updateDialog: true }); // 热更新检查
      })
      setTimeout(() => SplashScreen.hide(), 2000);
    })
  }
});
</script>
```

## 发布热更新

4.1 打包生成的网页资源存放在 dist 目录下

```json
// capacitor.config.json
{
  "appId": "com.ionicframework.sipapp202440",
  "appName": "因思工业服务",
  "webDir": "dist",   // 打包生成的web资源存放路径
  "bundledWebRuntime": false ,
  "Plugins": {
    "CodePush": {
      "ANDROID_DEPLOY_KEY": "X1vN32baDusvL1nVB15I9erm_9NRrBc_-YvwA",
      "SERVER_URL": "https://codepush.appcenter.ms/"
    }
  }
}
```

4.2 通过 appcenter-cli 发布更新

```shell
# 发布更新到 Production 环境，并指定受众版本不小于 1.0
# 缩短后
appcenter codepush release -c dist/ --target-binary-version "~1.0" -d Production
# 原命令
appcenter codepush release -a  <ownerName>/<appName> -c dist/ --target-binary-version "~1.0" -d Production

# 详解
appcenter codepush release -a <ownerName>/<appName> -c <updateContentsPath> -t <targetBinaryVersion> -d <deploymentName>

# [-с|--update-contents-path <updateContentsPath>]  
# 指定更新内容的路径

# [-t|--target-binary-version <version>]  
# 指定此次 release 的受众版本，不在该版本范围内的用户将不能下载此次更新

# [-d|--deployment-name <deploymentName>] 
# 指定是 Staging 或者 Production
```

4.3 模拟热更新

（1） 手机上要安装用于 `Debug` 或 `Release` 的 Apk、同时要进行一次热更新，将此时的页面内容上传至热更新服务器，作为初始内容

（2） 修改页面内容，然后进行热更新，但在热更新之前，需要运行如下命令，再上传热更新内容

```shell
ionic build
ionic cap sync # 保证写入热更新内容
```

否则发布热更新时会出现如下报错

```shell
Error: The uploaded package was not released because it is identical to the contents of the specified deployment's current release.
# 错误：上载的包未发布，因为它与指定部署的当前版本的内容相同。
```

4.4 第一次模拟热更新

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_585521_upEshZccL01iw9XF_1627890445%5B1%5D.png)

**手机并没有出现热更新提示**

Android Studio Debug 报错信息："Could not get preference: ANDROID_DEPLOY_KEY"

```shell
D/Capacitor: Sending plugin error: {"save":false,"callbackId":"22681177","pluginId":"CodePush","methodName":"getDeploymentKey","success":false,"error":{"message":"Could not get preference: ANDROID_DEPLOY_KEY"}}
D/Capacitor: Handling local request: http://localhost/js/chunk-2d0c9758.62f099a9.js
D/Capacitor: Handling local request: http://localhost/js/chunk-69eb8776.8f434896.js
I/Capacitor/Console: File: http://localhost/ - Line 213 - Msg: undefined
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: [CodePush]  Deployment key not found.. StackTrace: Error: Deployment key not found.
        at Function.<anonymous> (http://localhost/js/chunk-vendors.37e09625.js:1:9481)
        at Generator.throw (<anonymous>)
        at s (http://localhost/js/chunk-vendors.37e09625.js:1:8490)
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: [CodePush] An error occurred during sync. Deployment key not found.. StackTrace: Error: Deployment key not found.
        at Function.<anonymous> (http://localhost/js/chunk-vendors.37e09625.js:1:9481)
        at Generator.throw (<anonymous>)
        at s (http://localhost/js/chunk-vendors.37e09625.js:1:8490)
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: Uncaught (in promise) Error: Deployment key not found.
```

从报错信息上来看，是因为没有读取到 Deployment key

更改示例代码，syncOption 中添加 `deploymentKey` ，重新打包

```js
// App.vue
codePush.sync({ 
  updateDialog: true, 
  deploymentKey:'X1vN32baDusvL1nVB15I9erm_9NRrBc_-YvwA' 
});
```

4.5 第二次模拟热更新

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_617243_Mk5hLehhk6yGGfj3_1627891154%5B1%5D.jpg" alt="" style="zoom:25%;" />

**手机上有热更新提示，但是更新未安装成功**

Android Studio Debug 报错信息 "Could not get the package start page"

```shell
D/Capacitor: Sending plugin error: {"save":false,"callbackId":"2315552","pluginId":"CodePush","methodName":"preInstall","success":false,"error":{"message":"Could not get the package start page"}}
V/Capacitor/Plugin: To native (Capacitor plugin): callbackId: 2315553, pluginId: CodePush, methodName: getServerURL
V/Capacitor: callback: 2315553, pluginId: CodePush, methodName: getServerURL, methodData: {}
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: [CodePush] Preinstall failure. Could not get the package start page. StackTrace: Error: Could not get the package start page
        at Object.cap.fromNative.result [as fromNative] (http://localhost/:426:32)
        at <anonymous>:1:18
D/Capacitor: Sending plugin error: {"save":false,"callbackId":"2315553","pluginId":"CodePush","methodName":"getServerURL","success":false,"error":{"message":"Could not get preference: SERVER_URL"}}
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: [CodePush]  An error has occured while installing the package. Could not get the package start page. StackTrace: Error: An error has occured while installing the package. Could not get the package start page
        at c (http://localhost/js/chunk-vendors.37e09625.js:1:17470)
V/Capacitor/Plugin: To native (Capacitor plugin): callbackId: 2315554, pluginId: CodePush, methodName: getAppVersion
V/Capacitor: callback: 2315554, pluginId: CodePush, methodName: getAppVersion, methodData: {}
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: [CodePush] An error occurred during sync. An error has occured while installing the package. Could not get the package start page. StackTrace: Error: An error has occured while installing the package. Could not get the package start page
        at c (http://localhost/js/chunk-vendors.37e09625.js:1:17470)
E/Capacitor/Console: File: http://localhost/js/chunk-vendors.37e09625.js - Line 1 - Msg: Uncaught (in promise) Error: An error has occured while installing the package. Could not get the package start page
```

[https://github.com/mapiacompany/capacitor-codepush#api-reference](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fmapiacompany%2Fcapacitor-codepush%23api-reference)

从 capacitor-codepush 的 API 文档中查阅到：

提供给 -c 标志的路径可能因平台或框架而异，Ionic 项目的路径如下

+  Android : 	`./android/app/src/main/assets/public/`
+  iOS :	`./ios/App/App/public/`

所以是自己之前的热更新命令错了，更改发布命令

```shell
appcenter codepush release -c android/app/src/main/assets/public/ --target-binary-version "~1.0" -d Production
```

4.6 第三次模拟热更新

**手机上有热更新提示，但是更新内容未安装成功（后面发现其实并不是更新内容未安装成功，而是更新内容未立即安装，关闭之后再重新打开才会安装更新内容）**

Android Studio Debug 报错信息 "File does not exist"
/data/user/0/com.ionicframework.sipapp202440/files/codepush/currentPackage.json (No such file or directory)

https://github.com/mapiacompany/capacitor-codepush/issues/21

此报错不影响功能正常使用，参照上述博文，该 BUG 已被修复，但错误日志未进行删除

```shell
/Capacitor: Sending plugin error: {"save":false,"callbackId":"83886644","pluginId":"Filesystem","methodName":"stat","success":false,"error":{"message":"File does not exist"}}
V/Capacitor/Plugin: To native (Capacitor plugin): callbackId: 83886645, pluginId: Filesystem, methodName: readFile
V/Capacitor: callback: 83886645, pluginId: Filesystem, methodName: readFile, methodData: {"directory":"DATA","path":"codepush\/currentPackage.json","encoding":"utf8"}
E/Capacitor/Plugin: File does not exist
    java.io.FileNotFoundException: /data/user/0/com.ionicframework.sipapp202440/files/codepush/currentPackage.json (No such file or directory)
        at java.io.FileInputStream.open0(Native Method)
        at java.io.FileInputStream.open(FileInputStream.java:231)
        at java.io.FileInputStream.<init>(FileInputStream.java:165)
        at com.capacitorjs.plugins.filesystem.Filesystem.getInputStream(Filesystem.java:152)
        at com.capacitorjs.plugins.filesystem.Filesystem.readFile(Filesystem.java:24)
        at com.capacitorjs.plugins.filesystem.FilesystemPlugin.readFile(FilesystemPlugin.java:62)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.getcapacitor.PluginHandle.invoke(PluginHandle.java:121)
        at com.getcapacitor.Bridge.lambda$callPluginMethod$0$Bridge(Bridge.java:584)
        at com.getcapacitor.-$$Lambda$Bridge$25SFHybyAQk7zS27hTVXh2p8tmw.run(Unknown Source:8)
        at android.os.Handler.handleCallback(Handler.java:873)
        at android.os.Handler.dispatchMessage(Handler.java:99)
        at android.os.Looper.loop(Looper.java:224)
        at android.os.HandlerThread.run(HandlerThread.java:65)
```

https://github.com/mapiacompany/capacitor-codepush#syncoptions

+ **installMode (InstallMode)** - Specifies when you would like to install optional updates (i.e. those that aren't marked as mandatory). Defaults to `InstallMode.ON_NEXT_RESTART`. Refer to the `InstallMode` enum reference for a description of the available options and what they do.

更改 syncOptions，让用户点击 Install 之后立即更新，参照上述博文，installMode 默认值是 `InstallMode.ON_NEXT_RESTART`（下次重启之后再更新），将其改为 `InstallMode.IMMEDIATE` 即可

```js
// 更改后的 App.vue
import { codePush, InstallMode } from'capacitor-codepush';

setup() {
     onMounted(() => {
      // 由于是cordova插件，需要在deviceready回调后才能使用，用过Cordova的都懂
      document.addEventListener('deviceready', () => {
        new Jpush().setAlias('190395486111268864');
        codePush.sync({ 
          updateDialog: true, 
          deploymentKey: 'X1vN32baDusvL1nVB15I9erm_9NRrBc_-YvwA',
          installMode: InstallMode.IMMEDIATE // 注意这里
        });
      })
      setTimeout(() => SplashScreen.hide(), 2000);
    })
}
```

4.7 第四次模拟热更新

**手机上出现热更新提示，并且点击 install 之后立即热更新**

之前的页面

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_520468_BXXw9LsmxZIhaxDE_1627892079%5B1%5D.jpg" alt="" style="zoom:25%;" />

热更新提示

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_47699_Fa_JSeJforI4_QeM_1627892102%5B1%5D.jpg" alt="" style="zoom:25%;" />

热更新之后

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_83358_kmWez6IUL_Ek9fHZ_1627892136%5B1%5D.jpg" alt="" style="zoom:25%;" />

Code Push 集成成功 !!!

