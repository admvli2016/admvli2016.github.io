---
title: React Native 0.66 Demo 搭建全过程（Android）
date: 2021-12-24 18:43:42
tags: [React Native,Android]
categories: 移动端开发
typora-copy-images-to: upload
---

## 开发环境搭建（Android）

本文 Demo 是在 Windows 10 - 64位系统上搭建的。

[搭建开发环境 · React Native 中文网](https://reactnative.cn/docs/environment-setup)

参照上述博文，需要安装的软件

```
Node   			v12.22.0
JDK     		jdk1.8.0_221
Android Studio		v4.2.2
```

### 安装 Node

可参照下面的博文安装 `nvm`，通过 `nvm` 来安装 node 、管理 node 版本

[nvm 安装与使用 - Admvli2016's Blog](https://admvli2016.github.io/2021/12/08/nvm-安装与使用/#more)

<!--more-->

1.1.1 安装 Yarn

```shell
# 安装 yarn
npm install -g yarn 
```



### 安装 `JDK`

`JDK` 需要安装 `1.8.x` 版本的

（1）下载安装包（我的是 `jdk-8u221-windows-x64.exe`）

（2）安装过程不再赘述，更改文件安装目录  `D:\ShenGuYun\Java\jdk1.8.0_221\`，然后一步一步安装即可

（3）打开 `cmd` 查看 `JDK` 版本号，验证是否安装成功

 ```shell
# 查看 JDK 版本号
java -version
 ```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_345529_Rh7jWrVBFHpl3C9C_1630234703%5B1%5D.png)

（4）`JDK` 环境变量配置

+ 添加系统变量 `JAVA_HOME`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_282878_6TASNn0oOwuGSfzv_1630234777%5B1%5D.png)

+ PATH变量添加下面一条记录: `%JAVA_HOME%\bin`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_702431_W7TFWoJY5BlE_e7J_1630234796%5B1%5D.png)

+ 添加系统变量 `CLASSPATH`: `.;%JAVA_HOME%\lib;` （注：如存在这个环境变量，需要检查）

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_379115_CvsTHfHlWBA6PdA3_1630234816%5B1%5D.png)



### 安装 Android Studio

（1）Android Studio 安装过程

安装界面勾选 Android Virtual Device

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_277098_DJEFqUBOB3A5F-I7_1633922191%5B1%5D.png)

然后一直 Next 即可

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_96772_KkSC55Fow4GGsrQL_1633922212%5B1%5D.png)

点击 Finish，会弹出下面的弹框，选择 Do not import settings

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_916043_lGTGr8tXtRfmYZiJ_1633922262%5B1%5D.png)

接下来可以看到欢迎界面

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_883769_NVw7zP5eMy9X6Uu3_1633922280%5B1%5D.png)

点击 Next，选择 Standard

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_86724_El_pnSTPm9rqXXwa_1633922298%5B1%5D.png)

接下来一直 Next 即可，Android Studio 会自动下载开发所需的组件（需要打开 `VPN`）

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_992911_UUVZgPV0a_TV7yZO_1633922326%5B1%5D.png)

下载完成，点击 Finish

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_35586_P1tbQjKrzx_AeWXs_1633922347%5B1%5D.png)

至此 Android Studio 安装完成

（2）下载 `Android SDK`

Android Studio 安装完成后可以看见如下界面：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_303555_1L8zvXlP9v2U1fYm_1633922432%5B1%5D.png)

打开 `SDK Manager`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_66463_-LCNucY5KKUa_y-Z_1633922452%5B1%5D.png)



`SDK Manager` 还可以在 Android Studio 的 "Preferences" 菜单中找到。

具体路径是 `Appearance & Behavior → System Settings → Android SDK`。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_472594__hORnmIMDFX6YZAS_1633922594%5B1%5D.png)

确保勾选了下面这些组件

- `Android SDK Platform 29`
- `Intel x86 Atom_64 System Image`

然后点击 `SDK Tools` 选项卡

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_733902_O895YbwdkbImTtzv_1633922643%5B1%5D.png)

还是在 `SDK Tools` 选项卡

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_362628_m-EPGMTP33wQj6WU_1633922661%5B1%5D.png)

最后点击 “Apply” 来下载和安装这些组件
选择 Accept

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_356419_93jK7q4CT5gWDoKz_1633922689%5B1%5D.png)

点击 Next

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_819747_NK6l5Mu-Lo7ZwQZm_1633922713%5B1%5D.png)

（3）配置 ANDROID_HOME 环境变量

因为我之前搭建 `ionic5.x` Demo 时，已经配置过 ANDROID_HOME 环境变量，这里就不需要再进行处理了。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_68143_MI2fTrLafrSam6Mr_1633922798%5B1%5D.png)

在 Android Studio 的 "Preferences" 菜单中查看 `SDK` 的真实路径，具体是 `Appearance & Behavior → System Settings → Android SDK`。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_228389_3iFrHBctCQ3CD76p_1633922818%5B1%5D.png)

查看到的 `SDK` 路径也是之前配置过的 `SDK` 路径

`D:\ShenGuYun\Software\Android\android-sdk`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_654757_N6woAU3-AZc43EIl_1633922843%5B1%5D.png)

（4）将一些工具目录添加到环境变量 Path 中

如下所示：

```
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\emulator
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_789500_IuLagJ_jcCznqjif_1633922952%5B1%5D.png)

**重启电脑**使环境变量生效。



## 创建 React Native 项目

> 如果之前全局安装过旧的 `react-native-cli` 命令行工具，请使用 `npm uninstall -g react-native-cli` 卸载掉它以避免一些冲突。

使用 React Native 内建的命令行工具来创建一个名为 "react-native-demo" 的新项目。这个命令行工具不需要安装，可以直接用 node 自带的 `npx` 命令来使用：

> **必须要看的注意事项一**：请`不要`在目录、文件名中使用`中文、空格`等特殊符号。请`不要`单独使用常见的关键字作为项目名（如 class, native, new, package 等等）。请`不要`使用与核心模块同名的项目名（如 react, react-native 等）。
>
> **必须要看的注意事项二**：请`不要`在某些权限敏感的目录例如 `System32` 目录中 `init` 项目！会有各种权限限制导致不能运行！
>
> **必须要看的注意事项三**：请`不要`使用一些移植的终端环境，例如 `git bash` 或 `mingw` 等等，这些在 windows 下可能导致找不到环境变量。请使用系统自带的命令行（`CMD` 或 `powershell`）运行。

```shell
# 创建新项目
npx react-native init react-native-demo
npx react-native init my-react-native-demo
# 前面这两个创建报错
# 项目名称不合法 'xxx is not a valid name for a project'
npx react-native init MyReactNativeDemo
# 使用 MyReactNativeDemo 就创建成功了（Pascal命名：大驼峰命名法）
# 未使用带 typescript 配置的，正式开发时需要使用 typescript
```

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_440222_ezdFEgfPAPagvnVF_1633923177%5B1%5D.png)

**[可选参数] 指定版本或项目模板**

可以使用 `--version` 参数（注意是`两`个杠）创建指定版本的项目。注意版本号必须精确到两个小数点。(指定 React-Native 的版本)

```shell
npx react-native init MyReactNativeDemo --version X.XX.X
```

还可以使用 `--template` 来使用一些社区提供的模板，例如带有 `TypeScript` 配置的：

```shell
npx react-native init MyReactNativeDemo --template react-native-template-typescript
```



## 运行 React Native 项目

### 准备 Android 设备 (真机)

以我的手机 `vivo x23` 为例

（1）打开 `USB` 调试

`设置 > 系统管理 > 开发者选项 > 打开开发者选项 && USB 调试`

（2）通过 `USB` 数据线连接设备

检查设备是否能够正确连接到 `ADB`（Android Debug Bridge），使用 `adb devices` 命令：

过程中手机会弹出如下提示：

`允许 USB 调试吗？ ` 点击确定即可。

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_639304_q6IMEkH12ckfFAXZ_1633923757%5B1%5D.jpg" style="zoom:25%;" />

`adb devices` 运行结果如下所示：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_992736_4M0vb_AE9s_I5HXg_1633923780%5B1%5D.png)

在右边那列看到 **device** 说明设备已经被正确连接了。注意，每次只应当连接一个设备。

> 译注：如果你连接了多个设备（包含模拟器在内），后续的一些操作可能会失败。拔掉不需要的设备，或者关掉模拟器，确保 `adb devices` 的输出只有一个是连接状态。

（3）运行项目

```shell
# 运行项目
npx react-native run-android
```

> 提示：还可以运行 `npx react-native run-android --variant=release` 来安装 release 版的应用。当然需要先配置好签名，且此时无法再开启开发者菜单。注意在 debug 和 release 版本间来回切换安装时可能会报错签名不匹配，此时需要先卸载前一个版本再尝试安装。



### 准备 Android 设备（模拟器）

（1）使用 Android Studio 打开 `MyReactNativeDemo` 项目下的 android 目录

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_771117_XG_v4kDCCoC9QHrU_1633930037%5B1%5D.gif)

注意使用 Android Studio 打开项目时，需要关闭 `VPN`

出现报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_792252_HXKw6Ja_nDutYjyp_1633930092%5B1%5D.png)

[Cannot start react-native run-ios or run-android after initializing · Issue #31284 · facebook/react-native](https://github.com/facebook/react-native/issues/31284)

参照上述博文，是因为 node 版本的问题

```shell
# 切换 node 版本到 v12.22.0
nvm use 12.22.0
```

关闭项目 Close Project，重新打开

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_25441_F3xsslM-_b_2EYN9_1633930182%5B1%5D.png)

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_721387_rghYsPLoJbr0z5rR_1633930196%5B1%5D.png)

解决问题！

（2）使用 `AVD Manager` 查看可用的虚拟设备，它的图标看起来像下面这样：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_27737_OSYOi-wnzUAn1iwv_1633930248%5B1%5D.png)

点击 `AVD Manager`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_703180_WZJ_rDAz2w9p-Ioc_1633930271%5B1%5D.png)

（3）创建一个模拟设备

点击 "Create Virtual Device..."

然后选择所需的设备类型，点击 "Next"

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_371613_BirOivdgmzvUWvmG_1633930404%5B1%5D.png)

然后选择 Q (`API Level 29 image`)

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_602580_KdkxvfEp12_LcJzW_1633930448%5B1%5D.png)

点击 Download（此时需要打开 `VPN`）

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_322593_91B0oJVe_3Tr9PA5_1633930477%5B1%5D.png)

下载完成后点击 Finish

然后点击 “Next”

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_805035_kecamPqHqgwehTvO_1633930532%5B1%5D.png)

点击 “Finish”

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_928436_pd-H_o_XQqS31CRZ_1633930557%5B1%5D.png)

点击 ▸ 运行

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_670502_JqWbuxzGdqzpaVQU_1633930606%5B1%5D.png)

运行结果: 

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_241804_SXO9VYEqReVk4K9R_1633930646%5B1%5D.png" style="zoom: 67%;" />



> 译注：请不要轻易点击 Android Studio 中可能弹出的建议更新项目中某依赖项的建议，否则可能导致无法运行。



### 运行 React Native 项目 (使用真机)

此时需要打开 `VPN`

执行以下命令

```shell
# 打开项目文件夹
cd MyReactNativeDemo
# 运行项目
yarn android 
# 或者 yarn react-native run-android 
# 或者 npx react-native run-android
```

此命令会对项目的原生部分进行编译，同时在另外一个命令行中启动 `Metro` 服务对 ` js` 代码进行实时打包处理（类似 `Webpack`）。`Metro` 服务也可以使用 `yarn start` 命令单独启动。

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_75152_TFBkEpy6Dw6bAmYP_1633930855%5B1%5D.jpg" style="zoom: 25%;" />

使用模拟器运行项目执行命令同上，不过需要先打开模拟器

模拟器运行结果如图所示：

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_766254_Lv2vT6kZggCpHei4_1633931002%5B1%5D.png" style="zoom:67%;" />



### 修改项目中代码

（1）使用 VS Code 打开 `App.js` 文件并进行修改

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_634054_tzurNxZz-1a0PtVN_1633931293%5B1%5D.png)

（2）在 VS Code 中按 `Ctrl + S` 保存，就可以看到最新的修改

修改结果：

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_174670_CONMH5Jc-ywwSjOZ_1633931326%5B1%5D.jpg" style="zoom:25%;" />



## 调试 React Native 项目

注意以下操作都是使用真机调试，若使用模拟器，步骤基本一致。

### 开发菜单

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_427945_dscszb9WbxmgTYyg_1634265863%5B1%5D.jpg" style="zoom:25%;" />

开发菜单包括：`Reload`、`Change Bundle Location`、`Show / Hide Element Inspector`、`Enable / Disable Fast Refresh`、`Show / Hide Perf Monitor`、`Settings`、`Enable / Disable Sampling Profiler`、`Debug ` 等

（1）Reload -  重新加载 `App`

（2）Change Bundle Location

更改 Bundle 的位置，可用于更改远程调试地址（`debugger-ui`）

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_697542_F-gHDwv2aAub92F5_1634266287%5B1%5D.jpg" style="zoom:25%;" />

（3）Show / Hide Element Inspector

+ `Inspect`（审查元素）

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_562762_f4ES8JQyxtfpGYno_1634266428%5B1%5D.jpg" style="zoom:25%;" />

+ `Touchables`（查看可点击区域）

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_855203_uCnmY2dpQ263ZiRo_1634266452%5B1%5D.jpg" style="zoom:25%;" />

+ `Perf`（性能分析工具，需要配合 Chrome 开发者工具的 Performance）
+ `Network`（查看网络请求相关信息，需要配合 Chrome 开发者工具的 Network 使用）

（4）Enable / Disable Fast Refresh - 启用 / 禁用热重载

（5）Show / Hide Perf Monitor

打开允许显示在其他应用的上层

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_962893_85eHVKBd6zCKSgqa_1634266500%5B1%5D.jpg" style="zoom:25%;" />

开启一个悬浮层，其中会显示应用的当前帧数

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_247691_-blEoGK5QM7-JyNH_1634266525%5B1%5D.jpg" style="zoom:25%;" />

（6）Settings

开发调试的一些设置

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_784323_u7UEYJoVUBJEwwdq_1634266548%5B1%5D.jpg" style="zoom:25%;" />

（7）Enable / Disable Sampling Profiler - 启用 / 禁用 取样分析器（貌似不能用）

`JSIExecutor+JSCRuntimedoes not support Sampling Profiler`

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_612777_UeZYFNHTw1lBQJ3r_1634266896%5B1%5D.jpg" style="zoom:25%;" />

（8）Debug - 打开 Debug 模式



### Chrome 开发者工具（`debugger-ui`）

（1）`debugger-ui` 使用步骤

[调试 · React Native 中文网](https://reactnative.cn/docs/debugging)

[React Native开发和调试工具 - 掘金](https://juejin.cn/post/6844904015663218695)

[React Native调试技巧与心得 - 黑乌鸦 - 博客园](https://www.cnblogs.com/gaosheng-221/p/6954434.html)

参照上述博文

完整步骤：

+ 将设备通过 `USB` 数据线连接到电脑上，并开启 `USB` 调试（关于如何开启 `USB` 调试，参见上面的章节）。

+ 在 VS Code 中运行项目 `yarn android` 
+ 另外打开一个 `cmd` 窗口，运行 `adb reverse tcp:8081 tcp:8081`
+ 在弹出 Packager（metro）命令窗口中敲击 d，唤出开发菜单，再点击 Debug

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_904911_NGh3e3Q-W_mjrA-m_1634268975%5B1%5D.png)

如上图所示 `r` 重载 `app`，`d` 打开 `app` 开发菜单

此时可以在 Packager（metro）命令窗口中看到如下提示

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_170198_9Sfy3saq12PWdwwj_1634269013%5B1%5D.png)

且  **`app` 将会刷新一下页面**

点击打开 Debug 的同时将会自动打开调试页面 http://localhost:8081/debugger-ui

如下图所示，说明 Debug 模式已开启！

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_925910_1h7OqL6DA6jQn7_r_1634269163%5B1%5D.png)

在 Chrome 中按 `F12` 打开开发者工具，就可以在 Chrome 中调试 JavaScript 代码了。

在 Console 中可以看到 RN 项目中打印的信息

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_266337_g8J5b8NJAZu2ol8W_1634269217%5B1%5D.png)

> 注意：Chrome 中并不能直接看到 `App` 的用户界面，而只能提供 console 的输出，以及在 sources 项中断点调试 `js` 脚本。一些老的教程和文章会提到 React 的 Chrome 插件，这一插件目前并不支持 React Native，而且调试本身并不需要这个插件。不过可以安装独立（非插件）版本的 React Developer Tools 来辅助查看界面布局，下文会讲述具体安装方法

> 注意：使用 Chrome 调试目前无法观测到 React Native 中的网络请求，你可以使用功能更强大的第三方的 react-native-debugger 或是官方的 flipper（注意仅能在 0.62 以上版本可用）来观测。



+ 在 Source 中调试 `js` - 全局断点

**Pause On Caught Exceptions**

[JavaScript: Is there a way to get Chrome to break on all errors? - Stack Overflow](https://stackoverflow.com/questions/2233339/javascript-is-there-a-way-to-get-chrome-to-break-on-all-errors/17324511#17324511)



（2）Trouble Shooting

问题1：点击打开 Debug，`App` 报错：Failed to connect to debugger! Timeout while connecting to remote debugger

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_693616_7d7KCrS7ZPTYC4bw_1634275330%5B1%5D.jpg" style="zoom:25%;" />

原因是 8081 端口被占用了，导致连接不上。

解决方法：

[解决React Native端口号修改的方法 / 张生荣](https://www.zhangshengrong.com/p/9Oab7l5ONd/)

参照上述博文，尝试修改 RN 本地运行的端口号

第一种方法不可行，运行如下命令将会自动打开一个 Packager 窗口

```shell
# 将 apk 部署到手机上
npx react-native run-android
```

使用 `npx react-native start --port 9999` 打开的 Packager 窗口和部署在手机上的 `App` 没有关联，所以不可行 

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_285296_y4aOvaNxMT0P-KQg_1634275887%5B1%5D.png)

**摸索出来的正确修改 RN 本地运行端口号的办法**

+ 使用 `yarn android --port 9999` 运行项目，将 `apk` 部署到手机上
+ 在自动打开的 Packager 命令窗口中，按 d 唤出开发菜单
+ 选择 Change Bundle Location ，打开弹窗，输入 `localhost:9999`，点击确定
+ 重新唤出开发菜单，选择 Debug，开启 Debug 模式，此时自动打开的调试页面地址为 http://localhost:9999/debugger-ui

开了两个前端服务（占用 8080、8081）验证一下上述方法是否管用

在 `9998` 端口上验证了一下，上述方法确实**是可行的。**



观察到一个现象：

发现启用一个前端服务（占用 8081）之后，使用 yarn android 运行项目，打不开 Packager 页面，并且安装到手机上的 `app`，打开后直接报错：

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_205196_3DiAI_-7-8CVHMhH_1634276274%5B1%5D.jpg" style="zoom:25%;" />

说明运行 `yarn android` 默认占用的是 8081 端口

<hr/>

问题2：`debugger-ui` 页面显示 Status: Another debugger is already connected

[reactjs - React Native localhost Another debugger is already connected - Stack Overflow](https://stackoverflow.com/questions/61449875/react-native-localhost-another-debugger-is-already-connected)

参照上述博文，原因是 Chrome 有两个标签页同时访问了 http://localhost:8081/debugger-ui/  

或者 IE、Edge 和 Chrome 浏览器同时访问了 http://localhost:8081/debugger-ui/

<hr/>

问题3：`Packager` 命令窗口报错：`Error: Unable to resolve module ./debugger-ui/ui.e31bb0bc.js`

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_796422_b8k81z41OxZCQBX2_1634277059%5B1%5D.png" style="zoom:50%;" />

`App` 上同时警告： Remote debugger is in a background... 

完整内容：

```
Console Warning
Remote debugger is in a background tab which may cause apps to perform slowly. Fix this by foregrounding this tab (or opening it in a separate window).
```

控制台警告：远程调试器位于后台选项卡中，这可能会导致应用程序执行缓慢。通过激活该选项卡（或在单独的窗口中打开）修复此问题。

该报错不会影响 Debug 调试功能正常使用，可不作理会。

<hr/>

问题4：点击打开 Debug，`App` 报错：`Attempt to invoke interface method 'java.lang.String.com.facebook.react.bridge.CatalystInstance.getSourceURL()' on a null object reference`

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_645950_Cv59qVYquBW35Tmb_1634277478%5B1%5D.jpg" style="zoom:25%;" />

在开发菜单 Settings 页面设置 Debugging > Debug server host & port for device；然后再点击打开 Debug 会报这个错误，注意一下即可。



### `react-devtools`

```shell
# 全局安装 
npm install -g react-devtools
```

[react/packages/react-devtools at main · facebook/react](https://github.com/facebook/react/tree/main/packages/react-devtools)

参照上述博文

完整步骤：

+ 将设备通过 `USB` 数据线连接到电脑上，并开启 `USB` 调试
+ 在 VS Code 中运行项目 `yarn android` 
+ 启动 `react-devtools`：打开一个 `cmd` 窗口，运行命令 `react-devtools` 
+ 运行在真机上时，需要继续执行以下命令

```shell
adb reverse tcp:8097 tcp:8097
```

+ 在弹出 Packager（metro）命令窗口中敲击 d，唤出开发菜单，再点击 Show Element Inspector 即可。

  在 `react-devtools` 当中选中一个元素（类似于 Chrome 开发者工具中 Elements 的作用）

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_685314_lBzQ1Q3GmOrgI8o__1634267469%5B1%5D.png)

手机端同时选中了相同的元素。

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_379580_h0khrK7zbr-zVeBp_1634267505%5B1%5D.jpg" style="zoom:25%;" />



### React Native Debugger

（1）安装 React Native Debugger

`GitHub`： [jhen0409/react-native-debugger: The standalone app based on official debugger of React Native, and includes React Inspector / Redux DevTools](https://github.com/jhen0409/react-native-debugger)

下载页：[Releases · jhen0409/react-native-debugger](https://github.com/jhen0409/react-native-debugger/releases)

下载安装包，选择合适位置解压即可。



（2）React Native Debugger 调试注意事项

[react-native-debugger/getting-started.md at master · jhen0409/react-native-debugger](https://github.com/jhen0409/react-native-debugger/blob/master/docs/getting-started.md)

参照上述博文，调试时有以下几点需要注意：

+ 确保所有 React Native 的调试客户端都已经关闭，如 `debugger-ui`

+ 确保 React Native Debugger 已打开且处于等待状态

+ React Native Debugger 将尝试连接调试器代理，默认使用 `8081` 端口，如果需要，可以创建一个指定端口的新的调试器窗口（`macOS: comand+ T`，`Linux/Windows: Ctrl+T`）

+ 打开开发菜单，点击 Debug，将会自动启用 React Native Debugger



（3）React Native Debugger 使用步骤总结

[RN调试利器——React Native Debugger【图文】_mob604756fdc4e1_51CTO博客](https://blog.51cto.com/u_15127643/2770943)

[(11条消息) 如何使用React Native Debugger来调试 而不使用RN默认的debugger-ui_hzxOnlineOk的博客-CSDN博客_debugger-ui](https://blog.csdn.net/hzxOnlineOk/article/details/102842365)

[react-native-debugger 调试工具简单使用 · Fish Mobile X](https://fish.iwhalecloud.com/fish-mobile-x/blog/2020/06/10/react-native-debugger.html)

参照上述博文

完整步骤：

+ 将设备通过 `USB` 数据线连接到电脑上，并开启 `USB` 调试

+ 在 VS Code 中运行项目 `yarn android` 

+ 打开下载好的 React Native Debugger，确保所有 React Native 的调试客户端都已经关闭，如 `debugger-ui`

+ 在弹出 Packager（metro）命令窗口中敲击 d，唤出开发菜单，再点击 Debug 即可。

此时可以在 Packager（metro）命令窗口中看到如下提示

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_470545_mPOh6Pc5eaBKESHG_1634268402%5B1%5D.png)

且 **`app` 刷新了一下页面**，说明 Debug 模式已开启！

在 React Native Debugger 界面中可以看到上来的内容：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_866733_Moa7gWr6iVkheGQV_1634268438%5B1%5D.png)



（4）Trouble Shooting

问题：`React Native Debugger v0.12.1` 看不到 React Native Style Editor 部分

[react-native-debugger/react-devtools-integration.md at master · jhen0409/react-native-debugger](https://github.com/jhen0409/react-native-debugger/blob/master/docs/react-devtools-integration.md)

[Selective dependency resolutions  Yarn](https://classic.yarnpkg.com/en/docs/selective-version-resolutions/)

[React DevTools: Unsupported backend version](https://gist.github.com/bvaughn/4bc90775530873fdf8e7ade4a039e579)

参见上述博文，以为是 `react-devtools-core` 版本的问题

在自己的项目 `package.json` 里面加了下面一句

```json
"resolutions": {
    "react-devtools-core": "~4.13.5"
},
```

然后运行了一下 `yarn install`  ，没有任何作用，`react-devtools-core` 也删不掉了。

其实上面的博文和自己遇见的问题根本不一样，自己没有仔细看。

找到另外一篇博文

[cant see Native Style Editor · Issue #569 · jhen0409/react-native-debugger](https://github.com/jhen0409/react-native-debugger/issues/569)

参见上述博文，真正的原因是 `react-devtools` 版本的问题，最新版本的 `react-devtools` 不提供 React Native Style Editor 功能

这部分平常不怎么用到，对开发没有太大影响，也可以使用内置工具 Show Element Inspector 来替代



## React Native 项目打包

[打包发布 · React Native 中文网](https://reactnative.cn/docs/signed-apk-android)

[为应用签名   Android 开发者   Android Developers](https://developer.android.com/studio/publish/app-signing)

参照上述博文，首先需要配置好应用签名。

### 生成一个签名密钥

可以用 `keytool` 命令生成一个私有密钥。在 Windows 上 `keytool` 命令放在 `JDK` 的 bin 目录中（我的是 `D:\ShenGuYun\Software\Java\jdk1.8.0_221\bin`），需要在命令行中先进入此目录才能执行下列命令。

```shell
keytool -genkeypair -v -storetype PKCS12 -keystore my-release-key.keystore -alias com.xxxxxxx.xxxxxx -keyalg RSA -keysize 2048 -validity 1000
```

这条命令会要求你输入密钥库（`keystore`）和对应密钥的密码

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_995630_bES5y_eS747vJYe6_1633932021%5B1%5D.png)

```
# 密钥库（keystore）密码
xx123456
# 对应密钥的密码
xx123456
```

然后设置一些发行相关的信息

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_354722_KOFaSQtcq3S7x8Tk_1633932066%5B1%5D.png)

最后它会生成一个叫做 `my-release-key.keystore` 的密钥库文件。

生成的文件路径：

`D:\ShenGuYun\Software\Java\jdk1.8.0_221\bin\my-release-key.keystore`

在运行上面这条语句之后，密钥库里应该已经生成了一个单独的密钥，有效期为 10000 天。

`--alias` 参数后面的别名是将来为应用签名时所需要用到的，所以记得记录一下这个别名。

```
// 别名
com.xxxxxxx.xxxxxx
```

> 注意：请记得妥善地保管好密钥库文件，一般不要上传到版本库或者其它的地方。



### 设置  `gradle`  变量

（1）把 `my-release-key.keystore` 文件放到工程中的 `android/app`  文件夹下。

（2）编辑 `~/.gradle/gradle.properties`（全局配置，对所有项目有效）或是 `项目目录/android/gradle.properties`（项目配置，只对所在项目有效）。如果没有 `gradle.properties` 文件就自己创建一个，添加如下的代码（注意把其中的 `****` 替换为相应密码）

> 注意：~ 符号表示用户目录，比如 windows 上可能是 `C:\Users\用户名`，而 mac 上可能是 `/Users/用户名`。

```
# Signature application
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=com.xxxxxxx.xxxxxx
MYAPP_RELEASE_STORE_PASSWORD=xx123456
MYAPP_RELEASE_KEY_PASSWORD=xx123456
```

上面的这些会作为 `gradle` 的变量，在后面的步骤中可以用来给应用签名。



### 将签名配置添加到项目的 `gradle` 配置中

编辑项目目录下的 `android/app/build.gradle` 文件

```js
// android/app/build.gradle

android {
    ...
    defaultConfig {
        ...
        // applicationId "com.myreactnativedemo"
        applicationId "com.xxxxxxx.xxxxxx"  
    }
    
    signingConfigs {
         ...
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    
    buildTypes {
        release {
            ...
            // signingConfig signingConfigs.debug
            signingConfig signingConfigs.release
        }
    }
}
```



### 修改应用名称以及应用图标

[我算是从安装到打包完成了一个 React Native 项目 - SegmentFault 思否](https://segmentfault.com/a/1190000038920697)

参照上述博文

（1）修改应用名称

文件路径：`/android/app/src/main/res/values/strings.xml `

```xml
<resources>
    <string name="app_name">因思云</string>
</resources>
```



（2）修改应用图标

将下列文件夹下的图标替换成需要的 Icon 即可。

文件夹路径：`/android/app/src/main/res/`

![image-20211224181307772](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/image-20211224181307772.png)



### 生成发行 `APK` 包

只需在终端中执行以下命令：

```shell
cd android
./gradlew assembleRelease
```

> 译注：`cd android` 表示进入 android 目录（如果已经在 android 目录中了那就不用输入了）。`./gradlew assembleRelease` 在 `macOS`、`Linux` 或是 `windows` 的 `PowerShel`l 环境中表示执行当前目录下的名为 `gradlew` 的脚本文件，且其运行参数为 `assembleRelease`，注意这个 `./` 不可省略；而在 `windows` 的传统 `CMD` 命令行下则需要去掉 `./`。

`Gradle` 的 `assembleRelease` 参数会把所有用到的 JavaScript 代码都打包到一起，然后内置到 `APK` 包中。如果想调整下这个行为（比如 `js` 代码以及静态资源打包的默认文件名或是目录结构等），可以看看 `android/app/build.gradle` 文件，然后琢磨下应该怎么修改以满足需求。

> 注意：请确保 `gradle.properties` 中 `没有` 包含 `_org.gradle.configureondemand=true_` ，否则会跳过 `js` 打包的步骤，导致最终生成的是一个无法运行的空壳。

生成的 `APK` 文件位于 `android/app/build/outputs/apk/release/app-release.apk` ，它已经可以用来发布了。

运行 `gradlew assembleRelease` 报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_148146_uvpXNleOFDACgNKf_1633932439%5B1%5D.png)

猜测是因为 node 版本问题，切换至 `node v12.22.0`

再次尝试运行 `gradlew assembleRelease`，注意此时需要打开 `VPN`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_226936_UOa2BZMbYDyRzLRC_1633932466%5B1%5D.png)

**打包成功！**确实是 node 版本问题

生成的 `apk` 文件如下图所示：

文件路径：`xxx\MyReactNativeDemo\android\app\build\outputs\apk\release\app-release.apk`

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_434709_9zsXGWL2TrZFqTWA_1633932526%5B1%5D.png)



### 将 `APK` 安装到手机上

（1）直接安装 `APK`，可以正常使用

<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_117047_8G-IE2VzFhvJILsH_1633932677%5B1%5D.jpg" style="zoom:25%;" />

（2）命令形式安装

输入以下命令可以在设备上安装发行版本 `APK`：

```shell
# 打开项目文件夹
cd MyReactNativeDemo
# 安装发行版本 APK
npx react-native run-android --variant=release
```

>  注意 `--variant=release` 参数只能在完成了上面的签名配置之后才可以使用。

> 注意：在 debug 和 release 版本间来回切换安装时可能会报错签名不匹配，此时需要先卸载前一个版本再尝试安装。

安装到手机上成功。

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_191471_wqrXEDFBaFJkO53z_1633932778%5B1%5D.png)



<img src="https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_682730_lSo0SsFDpnbwU_SR_1633932792%5B1%5D.jpg" style="zoom:25%;" />

可以看到运行有报错：

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_483791_XjS-dkrztHCIVLgg_1633932815%5B1%5D.png)

应该是之前更改了 `applicationId` 的问题，改回来

```shell
// android/app/build.gradle
android {
    ...
    defaultConfig {
        ...
        applicationId "com.myreactnativedemo"
        // applicationId "com.xxxxxxx.xxxxxx"  
    }
 }
```

再次执行

```shell
# 安装发行版本 APK
npx react-native run-android --variant=release
```

没有报错了！

![](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_499164_EOonuITRSvOTbGZN_1633932907%5B1%5D.png)



### 针对不同的 CPU 架构生成 `APK` 以减小 `APK` 文件的大小

默认情况下，生成的 `APK` 会同时包含针对于多种 CPU 架构的原生代码。这样可以让我们更方便的向其他人分享这个 `APK`，因为它几乎可以运行在所有的 Android 设备上。但是，这会导致所有设备上都有一些根本不会运行的代码，白白占据了空间。目前安卓设备绝大多数是 ARM 架构，因此对于大部分应用来说可以考虑去掉 `x86` 架构的支持（但是请注意模拟器大部分是 `x86` 架构，因此去掉 `x86` 架构后将无法在模拟器上运行）。

可以在 `android/app/build.gradle` 中修改如下代码（false 改为 true）来生成多个针对不同 CPU 架构的 `APK`。

```js
// android/app/build.gradle
splits {
        abi {
            reset()
            enable enableSeparateBuildPerCPUArchitecture
            universalApk false  // If true, also generate a universal APK
            include "armeabi-v7a", "x86", "arm64-v8a", "x86_64"  // 注意这里
        }
    }
```

```js
// android/app/build.gradle
// def enableSeparateBuildPerCPUArchitecture = false
def enableSeparateBuildPerCPUArchitecture = true
```

可以把这上面打包生成的多个 `APK` 都上传到支持对用户设备 CPU 架构定位的应用程序商店，例如 `Google Play` 和 `Amazon AppStore`，用户将自动获得相应的 `APK`。如果您想上传到其他市场，例如 `APKFiles`（不支持一个应用有多个 `APK` 文件），可以修改下面的代码，来额外生成一个适用不同 CPU 架构的通用 `APK`。

```js
// android/app/build.gradle
splits {
        abi {
            reset()
            enable enableSeparateBuildPerCPUArchitecture
            // universalApk false  
            universalApk true // 额外生成一个适用不同CPU架构的通用APK
            include "armeabi-v7a", "x86", "arm64-v8a", "x86_64"  
        }
    }
```



### 启用 `Proguard` 来减少 `APK` 的大小（可选）

`Proguard` 是一个 Java 字节码混淆压缩工具，它可以移除掉 React Native Java（和它的依赖库中）中没有被使用到的部分，最终有效的减少 `APK` 的大小。

重要：启用 `Proguard` 之后，必须再次全面地测试你的应用。`Proguard` 有时候需要为你引入的每个原生库做一些额外的配置。参见 `app/proguard-rules.pro` 文件。

要启用 `Proguard`，修改 `android/app/build.gradle` 文件如下内容：

```js
// android/app/build.gradle
/**
 * Run Proguard to shrink the Java bytecode in release builds.
 */
// def enableProguardInReleaseBuilds = false
def enableProguardInReleaseBuilds = true
```