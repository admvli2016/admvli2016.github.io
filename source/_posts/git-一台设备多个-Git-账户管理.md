---
title: git 一台设备多个 Git 账户管理
date: 2021-12-23 18:38:56
tags: Git
categories: 代码版本管理
typora-copy-images-to: upload
---

目前我有3个 Git 账户，前一个是私人账户（Github 和 Gitee 共用），后一个是公司账户

```
github.com / gitee.com
账户：xxxxxxxxxx                邮箱：xxxxxxxx@qq.com
git.code.tencent.com (腾讯工蜂)
账户：xxxxxxxxxx                邮箱：xxxxxxxx@xxx.com
```

在自己的笔记本上办公时，既需要同步私人仓库的代码，又需要同步公司仓库的代码；在私人 git 账户和公司 git 账户之间切换非常麻烦，每次都需要重新生成 SSH key。若不想切换账户，就必须将用公司 git 账户生成的 SSH key 添加至私人仓库的 SSH keys 列表中，但我不想将二者混淆，于是参照下述博文进行了配置。

<!--more-->

参考博文：

[一台电脑管理多个Git账户和SSH KEY  霖溦之境](https://kukumalucn.github.io/blog/2018/08/02/一台电脑管理多个Git账户和SSH-KEY/)

[同一客户端下使用多个 Git 账号 - 掘金](https://juejin.cn/post/6844903902916132878)

## git 多个 SSH key 管理

1.1 创建 SSH key

```shell
# 1. 创建公钥
ssh-keygen -t rsa -C "xxxxxxxx@xxx.com"
# 2. 将公钥重命名为 id_rsa_tencent
/c/Users/admvli2016/.ssh/id_rsa_tencent
# 3. 之后一直回车即可
```

同理另一个 SSH key 创建流程如下所示

```shell
ssh-keygen -t rsa -C "xxxxxxxx@qq.com"
/c/Users/admvli2016/.ssh/id_rsa_github&&gitee
```



1.2 配置 SSH 代理

```shell
# 1. 查询系统 SSH KEY 的代理
ssh-add -l
# 若已设置代理，需要删除
ssh-add -D
# 若提示 Could not open a connection to your authentication agent. 执行以下命令
exec ssh-agent bash

# 2. 添加刚才创建的 SSH KEY 的私钥
ssh-add ~/.ssh/id_rsa_tencent
ssh-add ~/.ssh/id_rsa_github&&gitee
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_804559_shrifRDso1Uw967y_1637999494%5B1%5D.png)

识别不了 && 连接的字符，重新生成公钥

```shell
ssh-keygen -t rsa -C "xxxxxxxx@qq.com"
/c/Users/admvli2016/.ssh/id_rsa_github_gitee
```

重新添加

```shell
ssh-add ~/.ssh/id_rsa_github_gitee
```



1.3 添加公钥

[git 常用操作小结 - Admvli2016's Blog](https://admvli2016.github.io/2021/12/07/git-常用操作小结/)

参照上述博文



1.4 配置文件 `config`

```shell
# 1. 在 /.ssh 目录下创建 config 配置文件
vim ~/.ssh/config

# 2. config 文件内容如下
# github 配置
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_github_gitee

# gitee 配置
Host gitee.com
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/id_rsa_github_gitee
    
# 腾讯工蜂配置
Host git.code.tencent.com
    HostName git.code.tencent.com
    User git
    IdentityFile ~/.ssh/id_rsa_tencent
    
# 3. 编辑保存 config，再次查看 SSH KEY 的代理
ssh-add -l
```

发现到第3步后有问题

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_754000_xv5LGGjxjP3w8Zix_1638001559%5B1%5D.png)

还是报错： The agent has no identities.

[配置git链接到github遇到的问题_少年时。-CSDN博客](https://blog.csdn.net/a15512138486/article/details/106341668)

参照上述博文重新添加

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211223181359588.png)



1.5 验证 SSH key

```shell
# 输入以下命令，验证配置是否成功
ssh -T github.com
ssh -T gitee.com
ssh -T git.code.tencent.com
```

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211223182629871.png)

但是运行 `ssh -T git.code.tencent.com` 时报错，如下所示

![](https://gitee.com/admvli2016/pictures/raw/master/img/MTY4ODg1MDk0ODIwNzAwOA_42418_qmjkqgyINfWMgHGJ_1638002384%5B1%5D.png)

可能是由于腾讯工蜂不支持 SSH 登录这种方式



## git 多个账户管理

2.1 检查全局配置

```shell
# 查看全局配置
git config --global user.name
git config --global user.email

# 为了后续避免麻烦，我们可以取消全局配置
git config --global --unset user.name
git config --global --unset user.email
```



2.2 全局配置与局部配置

```shell
# 1. cd进入到指定git仓库的根目录下，执行局部git配置命令
cd D:\Source\ShenGuYun\Code\ins-app
git config user.name
git config user.email
# 2. 若返回均为空，说明未进行过局部配置，分别配置github/gitee/腾讯工蜂的账户名和邮箱
git config user.name "xxxxxxxx"
git config user.email "xxxxxxxx@qq.com"
```

同理，其他 git 仓库按照上述步骤依次进行操作即可。
已尝试，gitee/github/腾讯工蜂上都可成功拉取/推送

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211223181708837.png)

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211223181959306.png)

**另外即使没有取消默认的全局git配置，在进行了局部配置后，后者的优先级也会更高。**

```shell
git config --list
```

查看当前仓库的具体配置信息，在当前仓库目录下查看的配置是全局配置+当前项目的局部配置，**使用的时候会优先使用当前仓库的局部配置，如果没有，才会去读取全局配置。**



2.3  hexo 部署时 git 提交报错问题

![](https://gitee.com/admvli2016/pictures/raw/master/img/image-20211223183103960.png)

之前取消了 git 全局配置，只要将其恢复， hexo 就可以正常部署了。