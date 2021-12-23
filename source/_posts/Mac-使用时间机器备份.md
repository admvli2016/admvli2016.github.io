---
title: Mac 使用时间机器备份
date: 2021-12-23 16:13:02
tags: [Mac,时间机器]
categories: 操作系统
typora-copy-images-to: upload
---

参照下述博文，首先使用 ”时间机器“ 备份 Mac 必须要有一个外置的储存设备。

[如何备份 Mac - 官方 Apple 支持](https://support.apple.com/zh-cn/mac-backup)

[使用“时间机器”备份您的 Mac - Apple 支持 (中国)](https://support.apple.com/zh-cn/HT201250)

[可与“时间机器”搭配使用的备份磁盘 - Apple 支持 (中国)](https://support.apple.com/zh-cn/HT202784)

我尝试了两种备份方案：（1）备份到 smb 网络硬盘 （2）备份到移动硬盘

下面分别介绍一下操作步骤

<!--more-->

## 备份到 smb 网络硬盘

smb 网络硬盘也就是 windows 共享文件夹

参考博文：[添加Mac的Time Machine备份到smb网络硬盘（windows 共享文件夹）](https://www.douban.com/note/614980869/?ivk_sa=1024320u)

1.1 首先打开 “磁盘工具”

如何在 Mac 中打开 “磁盘工具” ？参照下面的博文

[如何使用Mac的磁盘实用程序进行分区，擦除，修复，还原和复制驱动器-howtoip.com在线科技杂志](https://www.howtoip.com/how-to-use-your-macs-disk-utility-to-partition-wipe-repair-restore-and-copy-drives/)

**Command + 空格  >  输入 Disk Utility**

![Lanuchboard 中的磁盘工具](https://gitee.com/admvli2016/pictures/raw/master/img/p41844520.webp)



1.2 在 “磁盘工具” 中，移到最上面的菜单栏，选择文件 > 新建映像 > 空白映像

![新建映像 > 空白映像](https://gitee.com/admvli2016/pictures/raw/master/img/p41844551.webp)



1.3 在弹出来的配置窗口中，填入如下信息。注意选择映像格式为 “稀疏磁盘映像”，设置大小为1TB。名称可以自己设定，最好不要用中文，支持空格，但后面配置命令行的时候要注意路径。配置完之后，点击 “存储”。

![配置镜像信息](https://gitee.com/admvli2016/pictures/raw/master/img/p41844569.webp)



1.4 点击存储后，大概几十秒钟的样子，就创建成功了。这个时候，你可以在 “文稿” 中看到这个镜像 “backup.sparseimage”，并且在 finder 中，应该能看到已经挂载了该镜像。如果挂载了，先退出这个镜像。没有的话，就忽略该步骤。

![查看是否挂载了该镜像](https://gitee.com/admvli2016/pictures/raw/master/img/p41844863.webp)



1.5 将 backup.sparseimage 通过网络拷贝到 smb 网络硬盘（Windows 共享文件夹）下，然后双击打开它。接下来，你可以在设备列表中看见 “backup” 挂载。如果没有看到，可以点击 ”xxx的Macbook Pro“ ，查看该镜像是否在列表中，在的话，可以拖出来到左边的设备列表中。

如何在 Mac 中访问 Windows 共享文件夹？参照下面的博文

[Mac OS X 访问 Windows共享文件夹 - 简书](https://www.jianshu.com/p/4b650b48c643)

**点击访达菜单中的 前往 > 前往服务器**

![在"XXX的MacBook Pro"中查看backup是否已挂载](https://gitee.com/admvli2016/pictures/raw/master/img/p41846688.webp)





1.6 接下来，打开终端，输入以下命令

```shell
sudo tmutil setdestination -p /Volumes/backup/
```

如何在 Mac 中打开终端？参照下面的博文

[(12条消息) mac启动terminal终端快捷键_彭世瑜的博客-CSDN博客_mac打开命令行快捷键](https://blog.csdn.net/mouday/article/details/80744586)

**Command + 空格 > 输入 terminal**

![终端里面输入命令](https://gitee.com/admvli2016/pictures/raw/master/img/p41846775.webp)

如果之前的名称中有空格，在输入的时候切记要加上`\`, 比如取得名字是 `mac backup`，那么上面的backup 的路径是 `mac\ backup`。

过程中出现如下报错：

```shell
tmutil: setdestination requires Full Disk Access privileges.
To allow this operation, select Full Disk Access in the Privacy tab of the Security & Privacy preference pane, and add Terminal to the list of applications which are allowed Full Disk Access.

# tmutil:setdestination 需要完全磁盘访问权限。要允许此操作，请在“安全性与隐私”菜单中选择“完全磁盘访问”选项卡，并添加 Terminal 到允许完全访问磁盘的应用程序列表中。
```

如何在 Mac 中打开 ”安全性与隐私“？参照下面的博文

[更改 Mac 上“安全性与隐私”中的“通用”偏好设置 - Apple 支持](https://support.apple.com/zh-cn/guide/mac-help/mh11784/mac)

**苹果菜单 > 系统偏好设置 > 点击安全性与隐私**

如何在 Mac 中启用完整磁盘访问权限？参照下面的博文

[在 macOS Mojave 中启用完整磁盘访问 – Intego 支持](https://support.intego.com/hc/de/articles/360016683471-Enable-Full-Disk-Access-in-macOS-Mojave)

![启用完整磁盘访问权限](https://gitee.com/admvli2016/pictures/raw/master/img/FDA.gif)



（1）打开 ”安全性与隐私“ 窗口，并选择 “隐私” 选项卡

（2）在左侧列表中选择 ”完整磁盘访问“

（3）点击左下角的锁图标，解锁界面

（4）输入您的 Mac 管理员密码

（5）将 Terminal 应用程序的图标拖放到右侧的列表中，如上图所示



1.7 成功后，打开 Time Machine 设置，就能看到 backup 磁盘了。勾上 “自动备份” 选项以及 “在菜单栏中显示 Time Machine”。

![Time Machine 设置界面中看到backup磁盘](https://gitee.com/admvli2016/pictures/raw/master/img/p41847030.webp)



1.8 接下来在桌面菜单栏中，可以看到 Time Machine 图标。点击选择立即备份，就可以了。

![Time Machine图标](https://gitee.com/admvli2016/pictures/raw/master/img/p41847070.webp)

基本的设置就是这样。这样的备份只是一种非常规的Time Machine使用，基于现有的条件使用。这种备份没办法解决系统崩溃无法开机的情况。还是建议定期使用移动硬盘作备份，并且妥善保管之。另外，在我之前的使用中，出现了网络临时断开后备份报错，到后面怎么也无法再次备份的情况，只能删掉挂载的分区重新建立，所以，这种情况还会带来这种无法预知的问题。大家用的时候还是要慎重。



## 备份到移动硬盘

参考博文：

[使用“时间机器”备份您的 Mac - Apple 支持 (中国)](https://support.apple.com/zh-cn/HT201250)

要使用“时间机器”创建备份，您只需一个外置储存设备。连接储存设备并选择它作为您的备份磁盘后，“时间机器”会自动制作过去 24 小时的每小时备份、过去一个月的每天备份以及之前所有月份的每周备份。如果备份磁盘已满，则最早的备份会被删除。

2.1 连接外置储存设备

连接以下其中一种外置储存设备

+ 连接到 Mac 的外置驱动器，例如 USB 或雷雳驱动器
+ 支持通过 SMB 进行“时间机器”备份的联网储存 (NAS) 设备
+ 共享为“时间机器”备份目标位置的 Mac
+ AirPort 时间返回舱，或连接到 AirPort 时间返回舱或 AirPort Extreme 基站 (802.11ac) 的外置驱动器



2.2 选择您的储存设备作为备份磁盘

2.2.1 打开时间机器

**苹果菜单 > 系统偏好设置 > 点按 ”时间机器“**

2.2.2 点按 ”选择备份磁盘“

![选择备份磁盘](https://gitee.com/admvli2016/pictures/raw/master/img/macos-big-sur-system-prefs-tm.jpg)

2.2.3 从可用磁盘列表中选择您的备份磁盘。为了使您的备份仅供拥有备份密码的用户访问，您可以选择 “加密备份”。然后点按 “使用磁盘”：

![加密备份&使用磁盘](https://gitee.com/admvli2016/pictures/raw/master/img/macos-big-sur-system-prefs-tm-select-backup-disk.jpg)

如果您选择的磁盘没有按照 “时间机器” 的要求进行格式化，系统会提示您先抹掉磁盘。点按 “抹掉” 以继续操作。这样操作会抹掉备份磁盘上的所有信息。 

如何在 Mac 上格式化磁盘？参照下面的博文

[在 Mac 上的“磁盘工具”中抹掉并重新格式化储存设备 - Apple 支持](https://support.apple.com/zh-cn/guide/disk-utility/dskutl14079/mac)

**文件系统格式必须为 "Mac OS 扩展"**



2.3 享受自动备份带来的便利

选择备份磁盘后，“时间机器” 会立即开始自动定期备份，无需您进行进一步操作。首次备份可能需要很长时间，但您可以在进行备份时继续使用 Mac。“时间机器” 只会备份自上次备份以来有变动的文件，因此将来的备份速度会加快。

要手动启动备份，请在菜单栏的 “时间机器” 菜单中选择 “立即备份”。使用这个菜单，还可以查看某一备份的状态或跳过正在进行的某一备份。



## 使用时间机器恢复 Mac 备份

参考博文：

[从备份恢复 Mac - Apple 支持 (中国)](https://support.apple.com/zh-cn/HT203981)

[在 Mac 上恢复使用时间机器备份的项目 - Apple 支持](https://support.apple.com/zh-cn/guide/mac-help/mh11422/mac)

3.1 在 Mac 上，打开想要恢复项目的窗口。

例如，若要恢复意外从 “文稿” 文件夹中删除的文件，请打开 “文稿” 文件夹。

如果丢失了桌面上的项目，则无需打开窗口。



3.2 打开时间机器。当您的 Mac 连接到备份磁盘时，可能会出现一条信息。

点按菜单栏中的时间机器图标 ，然后选取“进入时间机器”，也可以打开时间机器。如果菜单栏中没有时间机器图标，请选取苹果菜单  >“系统偏好设置”，点按“时间机器” ，然后选择“在菜单栏中显示时间机器”。



3.3 使用箭头和时间线浏览本地快照和备份。

![时间线浏览](https://gitee.com/admvli2016/pictures/raw/master/img/516113d998540ea662facfea43ffcf46.png)

如果看到脉冲光线到半暗灰色刻度标记，则表示仍在将备份载入备份磁盘或者仍在验证。



3.4 选择要恢复的一个或多个项目（其中可以包括文件夹或整个磁盘），然后点按“恢复”。

恢复的项目将返回到其原始位置。例如，如果某个项目在“文稿”文件夹中，它将返回“文稿”文件夹。