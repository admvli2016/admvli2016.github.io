---
title: Ionic5.x+Capacitor+Vue3.x 集成极光推送（Android）
date: 2021-12-11 22:14:20
tags: [Ionic5.x,Capacitor,Vue3.x,Android,极光推送]
categories: 移动端开发
---

参考博文：

[ionic快速集成极光推送 · dicallc/ionic3_angular4_JD Wiki](https://github.com/dicallc/ionic3_angular4_JD/wiki/ionic快速集成极光推送)

## 安装  cordova-plugin-jpush-capacitor

```shell
# 安装 cordova-plugin-jpush-capacitor
npm i cordova-plugin-jpush-capacitor 

# 同步至 ios / android 平台
ionic cap sync
```

<!--more-->

报缺少依赖错误：

```shell
cordova-plugin-jpush-capacitor is missing dependencies:
[capacitor]        - cordova-plugin-device
[capacitor]        - cordova-plugin-jcore
```

安装 cordova-plugin-device，运行如下命令：

```shell
npm install cordova-plugin-device
ionic cap sync
```

安装 cordova-plugin-jcore，运行如下命令：

```shell
npm install cordova-plugin-jcore 
ionic cap sync
```

查看插件列表：

```shell
npx cap ls
```

## 更改 appId 和 appName

修改下列文件中的 appId 和 appName

```json
// ionic.config.json
{
  "name": "因思工业服务",
  "integrations": {
    "capacitor": {}
  },
  "type": "vue"
}
```

```json
// capacitor.config.json
{
  "appId": "com.ionicframework.xxxxxx",
  "appName": "因思工业服务",
  "webDir": "dist",
  "bundledWebRuntime": false
}
```

删除 android 和 ios platform 后重新添加再打包（避免缓存）

```shell
# 安装 rimraf
npm install -g rimraf
# 删除 android 文件夹
rimraf android
# 删除 ios 文件夹
rimraf ios
# 添加 android 平台
npx cap add android
# 添加 ios 平台
npx cap add ios
# 打包
ionic build
# 同步至 ios / android 平台
ionic cap sync
# 在 Android Studio 中打开项目
ionic cap open android
```

## 在 AndroidManifest.xml 文件中添加极光推送的 AppKey 

极光后台：https://www.jiguang.cn/portal/#/dev/

可在极光开发者服务后台 > 应用管理 > 应用设置 > 应用信息中查看 AppKey


![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211211204733833.png)

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211211204839617.png)

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211211205701362.png)

不要忘记在 gradle.properties 文件中添加：

```properties
android.injected.testOnly=false
```

## 添加示例程序

```typescript
// src/utils/jpush.ts
import { isPlatform } from '@ionic/vue';
 class Jpush {
    jpush: any;
    constructor() {
        if (window.JPush) {
            this.jpush = window.JPush;
            this.jpush.setDebugMode(true);
            if (isPlatform('ios')) {
                this.jpush.startJPushSDK();
            }
            this.jpush.init();
        }
    }
    getRegistrationID() {
        return new Promise(resolve => {
            this.jpush.getRegistrationID(function (rId: string) {
                resolve(rId);
                // console.log("JPushPlugin:registrationID is " + rId);
            })
        })
    }
    // 设置别名
    setAlias(alias: string) {
        return new Promise(((resolve, reject) => {
            this.jpush.setAlias({
                alias,
                sequence: new Date().valueOf()
            }, (res: { alias: string; sequence: number }) => {
                // console.log('别名设置成功: ', res);
                resolve(res);
            }, (err: { code: number; sequence: number }) => {
                // console.log('别名设置失败: ', err);
                setTimeout(() => this.setAlias(alias), 3000);
                reject(err);
            });
        }))
    }
    // 设置角标 只限IOS
    setBadge(badge: number) {
        if (isPlatform('ios')) {
            this.jpush.setBadge(badge);
        }
    }
}
export default Jpush
```

```html
<!-- App.vue -->
<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script lang="ts">
import { Plugins } from "@capacitor/core";
import { defineComponent, onMounted } from 'vue';
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import jpush from "@/utils/jpush";
const { SplashScreen } = Plugins;

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
        new jpush().setAlias('app');
      })
      setTimeout(() => SplashScreen.hide(), 2000);
    })
  }
});
</script>
```

## 极光后台模拟推送通知

极光后台：https://www.jiguang.cn/portal/#/dev/

5.1 创建推送信息

设置平台、标题、内容、发送时间

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_474570_vbjkHqJ0bCwsnccP_1627541625%5B1%5D.png)

5.2 选择发送给哪些人

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_928184_JskED6hOUhlZAiP7_1627541642%5B1%5D.png)

5.3 发送推送消息

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_179806_MEQpgv0lOPWK5XxY_1627541672%5B1%5D.png)

5.4 确认发送

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_991448_zZ7z_ezmjqdGoir6_1627541686%5B1%5D.png)

5.5 在 App 上查看是否收到推送通知

## 测试极光推送是否集成成功

重新打包，在 Android Studio 中运行项目并将其安装到手机上

**每次重新打包之后都需要去配置极光插件的 AppKey，之前的配置将会被清除**

在安装过程中，Android Studio 控制台报错：

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_302272_cB5Lei1GvnYKLkkN_1627541317%5B1%5D.png)

原因是没有给 App 授予通知权限（**记住一定要给 App 授予通知权限**）

授予通知权限后，在极光后台上模拟推送一条通知，发现手机上还是接收不到

极光后台报错提示如下：

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_937395_uAOdjUiwaQt1fl2N_1627541369%5B1%5D.png)

可能是因为此时设置的设备别名（alias）有问题，将其改为自己的用户ID 190395486111268864 

```js
// App.vue
document.addEventListener('deviceready', () => {
  // 设置为 190395486111268864
  new jpush().setAlias('190395486111268864');
})
```

重新打包并安装到手机上（需要将之前安装的 APP 删掉），记得授予通知权限

如下图所示，极光推送集成成功！！！ 

<img src="https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_645541_Z-Zt8_OJbbneR7Ym_1627541459%5B1%5D.jpg" alt="" style="zoom: 25%;" />

## 测试 App(Android) 处于不同状态时的推送机制

| Android（Demo）        | 前台模式     | 后台模式     | 关闭     |
| :--------------------- | ------------ | ------------ | -------- |
| 是否推送               | 推送         | 不推送       | 不推送   |
| **Android（线上App）** | **前台模式** | **后台模式** | **关闭** |
| 是否推送               | 推送         | 推送         | 不推送   |

## 怎么让 Demo 进入后台模式也可进行推送

发现将 AndroidManifest.xml 中的配置放在 capacitor-cordova-android-plugins 文件夹中，App 进入后台模式就接收不到推送；而将其放到 app 文件夹中，进入后台模式也可接收到推送

```xml
<!-- app/manifests/AndroidManifest.xml -->
<!-- 将极光推送配置粘贴到 </application> -->
<application>
...
<meta-data android:name="JPUSH_CHANNEL" android:value="developer-default"/>
<meta-data android:name="JPUSH_APPKEY" android:value="AppKey"/>
</application>  
```

```xml
<!-- capacitor-cordova-android-plugins/manifests/AndroidManifest.xml -->
<!-- 删除掉下面两句
这里每次运行 ionic cap sync 时，都会重新生成，所以在打包之前需要检查下，否则 ionic build 会报错 -->
<meta-data android:name="JPUSH_CHANNEL" android:value="developer-default"/>
<meta-data android:name="JPUSH_APPKEY" android:value="AppKey"/>
```

