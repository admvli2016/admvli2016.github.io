---
title: nvm 安装与使用
date: 2021-12-08 22:59:30
tags: nvm
categories: 前端工程化
typora-copy-images-to: upload
---

## nvm 安装（Windows）

1.1 下载安装包

1.2 选择自定义安装目录，例如 D:\Software\nvm

1.3 设置 nodejs 的安装存储目录，例如 D:\Software\nvm\nodejs 

1.4 查看 nvm 是否安装成功

<!--more-->

```shell
# 查看 nvm 版本
nvm version
```

1.5 配置 node 和 npm 的镜像地址

```shell
# 配置 node 镜像地址
nvm node_mirror https://npm.taobao.org/mirrors/node/
# 配置 npm 镜像地址
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

1.6 打开安装目录 D:\Software\nvm，新建一个文件夹 nodejs

1.7 设置环境变量

用户变量：将 NVM_SYMLINK 变量值改为 D:\Software\nvm\nodejs

系统变量：将 NVM_SYMLINK 变量值改为 D:\Software\nvm\nodejs

1.8 关闭 cmd 或 powershell，然后重新打开，这一步的目的是为了使环境变量生效

1.9 安装 node 版本

```shell
# 查看可安装的 node 版本列表
nvm list available
# 安装 node v14.17.3
nvm install 14.17.3
# 切换 node 版本
nvm use 14.17.3
# 查看 node 版本
node -v
# 查看 npm 版本
npm -v
```

**Tips**: 若 nvm use xxx，出现下面的报错：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16456703304112.png)

exit status 1: You do not have sufficient privilege to perform this operation.

此时需要用管理员身份打开 cmd 命令行工具，再执行 nvm use xxx。

1.10 设置 npm 全局依赖在各 node 版本之间共享（安装全局依赖使用 `npm i xxx -g`）

（1）用 npm root -g 命令，可查看 npm 全局依赖默认的存储位置

默认的 npm 全局依赖安装目录：C:\Users\admvli2016\AppData\Roaming\npm

默认的 npm 全局依赖缓存目录：C:\Users\admvli2016\AppData\Roaming\node_cache

（2）修改 npm 全局依赖安装目录、npm 全局依赖缓存目录

在 D:\Software\nvm 文件夹下创建 node_global 和 node_cache 文件夹

```shell
# 修改 npm 全局依赖安装目录：
npm config set prefix "D:\Software\nvm\node_global"
# 修改 npm 全局依赖缓存目录：
npm config set cache "D:\Software\nvm\node_cache"
```

这样做的目的主要为了避免在不同 node 版本下都安装一遍需要的 npm 全局依赖

**若需要安装的 npm 全局依赖之间存在版本冲突等不兼容问题（如一个项目依赖 `ionic/cli@3.9.2` ，另一个项目依赖 `ionic/cli@6.16.3`），则不建议采取上述策略。可按步骤（3）操作**

（3）修改 npm 全局依赖安装目录、npm 全局依赖缓存目录（**可选**）

在 D:\Software\nvm\nodejs 文件夹下创建 node_global 和 node_cache 文件夹

```shell
# 修改 npm 全局依赖安装目录：
npm config set prefix "D:\Software\nvm\nodejs\node_global"
# 修改 npm 全局依赖缓存目录：
npm config set cache "D:\Software\nvm\nodejs\node_cache"
```

这样做的缺点是对于常用的 npm 全局依赖需要在所有需要的 node 版本下都安装一遍。

1.11 设置系统变量

（1）新增环境变量 NODE_PATH 

D:\Software\nvm\node_global\node_modules

（2）在 PATH 变量上添加

D:\Software\nvm\node_global

> 若上一步进行了操作（3），这一步也需要相应地更改文件夹路径。
>
> + 新增环境变量 NODE_PATH 
>
> D:\Software\nvm\nodejs\node_global\node_modules
>
> + 在 PATH 变量上添加
>
> D:\Software\nvm\nodejs\node_global

1.12 设置淘宝镜像

```shell
# 设置淘宝镜像
npm config set registry=http://registry.npm.taobao.org
```

1.13 nvm 常用命令

```shell
# 查看当前系统是 32bit 还是 64bit
nvm arch  
# 安装 xxx 版本的 node（32bit / 64bit）
nvm install xxx  [arch]   
# 查看可安装的 node 版本列表
nvm list available  
# 查看已安装的 node 版本列表
nvm list / nvm list installed  	
# 配置 node 镜像地址
nvm node_mirror [url]   
# 配置  npm 镜像地址
nvm npm_mirror [url]    
# 卸载 xxx 版本的 node
nvm uninstall xxx  
# 切换 node 版本
nvm use [version] [arch]   
# 查看 nvm 版本
nvm version  
# 帮助
nvm help  
```

1.14 补充

（1）配置完成之后，若使用 npm 下载依赖不成功，可通过下列命令检查 npm 是否有代理配置。

```shell
npm config list
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16480043084936.png)

确保 `https-proxy`、`proxy` 为空。否则可通过下列命令进行设置：

```
npm config set proxy null
npm config set https-proxy null
```

参考博文：

[(30条消息) nodejs环境变量配置及使用n及nvm进行版本切换_ruanhongbiao的专栏-CSDN博客_nvm安装的node环境变量](https://blog.csdn.net/qappleh/article/details/98210168)



## nvm 安装（macOS）

2.1 安装 Homebrew

[The Missing Package Manager for macOS (or Linux) — Homebrew](https://brew.sh/)

```shell
# 安装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装失败

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_110930_2H4Zn8oeNqBPEm0M_1634724494%5B1%5D.png)

[MacOs M1安装Homebrew 在国内最简单方法_YD-10-NG的博客-CSDN博客](https://blog.csdn.net/sinat_38184748/article/details/114115441)

参照上述博文

```shell
# 主机是 intel 芯片
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

安装成功

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_684570_d49PN2YGVTay--PF_1634725347%5B1%5D.png)

2.2 安装 nvm

```shell
# 安装 nvm
brew install nvm
```
安装成功

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_986409_Rce_MckZCM_3uOeN_1634797297%5B1%5D.png)

2.3 配置可在 shell 中使用 nvm 命令

```shell
# 查看是否存在 ~/.nvm 文件夹
cd ~/.nvm 
# 创建 ~/.nvm 文件夹
mkdir ~/.nvm
# 查看是否存在 ~/.profile 文件
cd ~/.profile
# 返回根目录
cd ~
# 创建 .profile 文件
touch .profile
```

打开并在 .profile 文件中添加如下信息

```shell
export NVM_DIR="$HOME/.nvm"
  [ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && . "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

[MAC：通过NVM安装指定版本的node - 简书](https://www.jianshu.com/p/a6044bd2ad35)

参照上述博文，重新 source

```shell
source .profile
```

2.4 使用 nvm 安装不同版本 node

```shell
# 查看远程所有可用的 node 版本
nvm ls-remote
# 下载想要安装的版本
nvm install xxx  				# nvm install v8.11.3
# 使用指定版本的 node
nvm use xxx  					# nvm use v8.11.3
# 每次启动终端都使用该版本的 node
nvm alias default xxx  			# nvm alias default v8.11.3 
```

2.5 查看版本信息

```shell
# 查看 node 版本
node -v
# 查看 npm 版本
npm -v
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_174170_lE6IMXhcnr4CM9HT_1634798562%5B1%5D.png)

还可以配置在不同 node 版本下全局安装的依赖始终可用，参照下述博文：

[(12条消息) mac nodejs安装_weixin_30791095的博客-CSDN博客](https://blog.csdn.net/weixin_30791095/article/details/95144605)