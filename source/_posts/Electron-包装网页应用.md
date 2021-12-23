---
title: Electron 包装网页应用
date: 2021-12-23 11:42:44
tags: Electron
categories: 桌面端开发
---

## 安装 Electron
1.1 环境

```shell
npm -v   
# v12.22.0

node -v  
# 6.14.11

# 操作系统：win10
```

<!--more-->

1.2 安装命令

```shell
# 安装 electron
npm install electron -g --platform=win32

# 验证安装结果
electron -v 
# v15.2.0
```

## 创建 Electron 项目

2.1 创建文件夹 `ui-app-electron`，并进入该文件夹

```
cd D:\ShenGuYun\Source\gitee\ui-app-electron
```

2.2 创建 `package.json` 文件，内容如下

```json
{
  "name": "ins-app",
  "version": "0.1.0",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  },
  "devDependencies": {
    "electron": "^15.2.0"
  }
}
```

2.3 创建 `main.js`，内容如下：

```js
const {app, BrowserWindow} = require('electron')

let mainWindow

// 创建主窗口，设置了宽高等信息
function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 600,
    webPreferences: {
      // node集成，即是否注入node能力
      nodeIntegration: true
    }
  })

  // 加载主页面内容 index.html
  mainWindow.loadFile('index.html')

  mainWindow.on('closed', function () {
    mainWindow = null
  })
}

app.on('ready', createWindow)
```

2.4 创建 `index.html` 文件，内容如下：

```html
<!DOCTYPE html>
<html>
<head>
    <!-- 此title会覆盖package.json中设置的name，作为应用顶部名称 -->
    <title>因思云</title>
</head>
<body>
    Hello World!
</body>
</html>
```

## 运行项目

```shell
# 启动 Electron 应用
npm start
```

## 封装 `InS OS` 网页

4.1 修改 `main.js` 文件

```js
const {app, BrowserWindow} = require('electron')

let mainWindow

// 创建主窗口，设置了宽高等信息
function createWindow () {
  mainWindow = new BrowserWindow({
    // width: 1000,
    // height: 770,
    // 支持最大化
    maximizable: true,
    // 为了让初始化窗口显示无闪烁，先关闭显示，等待加载完成后再显示。 
    show: false,   
    webPreferences: {
      // node集成，即是否注入node能力
      // nodeIntegration: true
    }
  })
  // 加载主页面内容 index.html
  // mainWindow.loadFile('index.html')
  // 改为使用loadURL加载 url地址
  mainWindow.loadURL('http://ins.shenguyun.com/')

  mainWindow.on('closed', function () {
    mainWindow = null
  })
  // 自动隐藏菜单
  // mainWindow.setAutoHideMenuBar(true);
  // 打开时最大化打开，不是全屏，保留状态栏
  mainWindow.maximize();    
}

app.on('ready', createWindow)
```

4.2 重新运行项目

```shell
# 重新启动 Electron 应用
npm start
```

可以看到嵌进去的网页   [ins.shenguyun.com/...](http://ins.shenguyun.com/)

![MTY4ODg1MDk0ODIwNzAwOA_491831_a3bwkn-J7qFGeBi-_1639757803[1]](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_491831_a3bwkn-J7qFGeBi-_1639757803%5B1%5D.png)

## 将该项目打包成桌面端应用

5.1 安装 `electron-builder`

```shell
# 在项目文件夹下运行以下命令
npm install --save-dev electron-builder
# 验证安装结果
node_modules/.bin/electron-builder -h
```

 运行 `node_modules/.bin/electron-builder -h` 报错：Error: Cannot find module 'fs/promises'

[node.js - Cannot find module 'fs/promises' Electron JS - Stack Overflow](https://stackoverflow.com/questions/68085375/cannot-find-module-fs-promises-electron-js)

参照上述博文，将 `electron-builder` 版本降低到 `v22.10.5`

```shell
npm install --save-dev electron-builder@22.10.5
```

运行 `node_modules/.bin/electron-builder --version`

![MTY4ODg1MDk0ODIwNzAwOA_968198_ifuS-05K9EcdjNb0_1639758245[1]](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_968198_ifuS-05K9EcdjNb0_1639758245%5B1%5D.png)

说明安装 `electron-builder` 成功

5.2 打包当前项目（Windows）

```shell
# Windows 打包成 exe 安装文件
# 在 Windows 环境下执行
node_modules/.bin/electron-builder -w nsis
```

打包过程中由于 `package.json` 中没有设置 repository 字段可能会报错，但不影响文件生成，忽略即可。

5.3 打包最终生成的 `exe` 文件在当前项目的 dist 文件夹下

![MTY4ODg1MDk0ODIwNzAwOA_608165_0-CXuVeJSxWYjB62_1639758302[1]](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_608165_0-CXuVeJSxWYjB62_1639758302%5B1%5D.png)

## 设置窗口图标以及桌面快捷方式图标

[(12条消息) Electron设置窗口图标、设置桌面快捷方式图标_BigFamer的博客-CSDN博客_electron 窗口图标](https://blog.csdn.net/BigFamer/article/details/109293202)

问题：桌面快捷方式的图标格式必须大于 `256x256`

解决方法：使用稿定调整图片大小

[【在线PS软件】在线PS图片（照片）处理工具_在线制作编辑图片ps精简版](https://www.uupoop.com/)

更改后的 `main.js` 文件

```js
const {app, BrowserWindow} = require('electron')
const path = require('path')

let mainWindow

// 创建主窗口，设置了宽高等信息
function createWindow () {
  mainWindow = new BrowserWindow({
    // width: 1000,
    // height: 770,
    // 下面这行代码就是配置窗口图标的核心代码了
    // 注意，这里的path是一个node模块哦，需要npm安装并且引入使用。最直接的作用就是拼接字符串。
    icon: path.join(__dirname, './logo@20.ico'),    
    // 支持最大化
    maximizable: true,
    // 为了让初始化窗口显示无闪烁，先关闭显示，等待加载完成后再显示。 
    show: false,   
    webPreferences: {
      // node集成，即是否注入node能力
      // nodeIntegration: true
    }
  })
  // 加载主页面内容 index.html
  // mainWindow.loadFile('index.html')
  // 改为使用loadURL加载 url 地址
  mainWindow.loadURL('http://ins.shenguyun.com/')

  mainWindow.on('closed', function () {
    mainWindow = null
  })
  // 自动隐藏菜单
  // mainWindow.setAutoHideMenuBar(true);
  // 打开时最大化打开，不是全屏，保留状态栏
  mainWindow.maximize();    
}

app.on('ready', createWindow)
```

更改后的 `package.json` 文件

```json
{
  "name": "InS_OS",
  "version": "0.1.0",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  },
  "devDependencies": {
    "electron": "^15.2.0",
    "electron-builder": "^22.10.5"
  },
  "author": "admvli2016",
  "repository": "github:xxx/xxx",
  "build": {
    "win": {
      "icon": "logo.png",
      "target": [
        "nsis"
      ]
    },
    "nsis": {
      "allowToChangeInstallationDirectory": true,
      "oneClick": false,
      "menuCategory": true,
      "allowElevation": false
    }
  },
  "dependencies": {
    "path": "^0.12.7"
  }
}
```

参考博文：

[electron安装包里封装浏览器直接访问URL配置  码农家园](https://www.codenong.com/jsb60fe36cbe84/)

[Electron入门指南  一篇文章看懂Electron封装网页并打包应用](https://qii404.me/2019/07/10/electron.html)

