---
title: git 常用操作小结
date: 2021-12-07 23:35:10
tags: Git
categories: 代码版本管理
---

## git 配置密钥 SSH Key

1.1 首先检查一下用户名和邮箱是否配置

```shell
# 查看全局的 git 配置
git config --global --list
```

若未进行配置，则执行下列命令进行配置

```shell
git config --global user.name "admvli2016"
git config --global user.email "3148441341@qq.com"
```
<!--more-->

1.2 然后执行以下命令生成密钥 SSH Key

```shell
ssh-keygen -t rsa -C "3148441341@qq.com"
```

执行命令后需要进行3次或4次确认：

i，确认秘钥的保存路径（如果不需要改路径则直接回车）

ii，如果上一步置顶的保存路径下已经有秘钥文件，则需要确认是否覆盖（如果之前的秘钥不再需要则直接回车覆盖，如需要则手动拷贝到其他目录后再覆盖）

iii，创建密码（如果不需要密码则直接回车）

iv，确认密码

**直接敲3~4次回车即可**

生成的 id_rsa 和 id_rsa.pub 文件默认的保存路径是：

C:\Users\admvli2016\.ssh

1.3 打开 id_rsa.pub 文件，将其内容复制 Gitee/GitLab/GitHub/腾讯工蜂 当中

可通过下列命令查看 id_rsa.pub 文件内容

```shell
# 查看 id_rsa.pub 文件内容
cat ~/.ssh/id_rsa.pub
```



- GitHub 中配置 SSH


![Github](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_672166_bNPnkfg9Vd3lKZRf_1632469789.gif)
          


- Gitee 中配置 SSH


![Gitee](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_614336_x95KDOip3_2N3UQ__1632469804.gif)



- GitLab 中配置 SSH


![GitLab](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_474104_QeOu_A3Uw5JvFfBH_1632469833.gif)



- 腾讯工蜂中配置 SSH


![腾讯工蜂](https://admvli2016.oss-cn-shenzhen.aliyuncs.com/img/MTY4ODg1MDk0ODIwNzAwOA_424484_8fmD2tcHxpek2Mim_1632469847.gif)           



参考博文：

[git ssh key配置 - 讨厌走开啦 - CSDN博客](https://blog.csdn.net/lqlqlq007/article/details/78983879)

[利用Git生成本机SSH Key并添加到GitHub中 - 境由心生 - CSDN博客](https://blog.csdn.net/u014103733/article/details/79190004)

[(1条消息)github上传项目的时候报出git@github.com: Permission denied (publickey). fatal: Could not read from remote repo_weixin_44394753的博客-CSDN博客](https://blog.csdn.net/weixin_44394753/article/details/91410463)



## git 查看 & 设置用户名、用户邮箱

2.1 git 查看当前用户名、用户邮箱

```shell
# 查看当前用户名
git config user.name
# 查看当前邮箱地址
git config user.email
```

2.2 git 设置用户名、用户邮箱

```shell
# 全局级配置，如果没有仓库级别的特殊配置，默认读取这个配置
git config --global user.name "name"
git config --global user.email "email"

# 仓库级配置，一般一个项目配置一次
git config user.name "name"
git config user.email "email"
```

参考博文：

[GIT 查看/修改用户名和邮箱地址 - 程序园](http://www.voidcn.com/article/p-sybtklyd-bqw.html)



## git 创建本地仓库，与远程仓库关联

```shell
# 1，初始化本地仓库
git init

# 2，在 gitee/gitlab/github 上新建仓库
# 新建的远程仓库地址： git@gitee.com:admvli2016/git-test.git

# 查看本地关联的远程仓库地址
git remote -v

# 3，若本地已有关联的远程仓库，则会报错：'fatal:remote origin already exists'
# 移除已有的远程仓库地址，再添加
git remote remove origin

# 4，添加新建的远程仓库地址
git remote add origin git@gitee.com:admvli2016/git-test.git

# 5，初次提交
# 添加文件
git add .

# commit 提交
git commit -m "备注"

# 将本地分支 master 内容推送到远程仓库去
# 第一次推送 master 分支时，加上了 –u 参数
# 此时，git 不但会把本地的 master 分支内容推送到远程新的 master 分支上，还会把本地的 master 分支与远程的 master 分支关联起来
git push -u origin master
```

或者

```shell
# 1，在 gitee/gitlab/github 上新建仓库时选择初始化仓库
# 2，然后使用 git clone "xxx(刚才新建的远程仓库地址)"，如下所示，也可将本地仓库与远程仓库关联起来
git clone git@gitee.com:admvli2016/git-test.git
```

参考博文：

[git基础知识（一）之把本地仓库推送到github - 简书](https://www.jianshu.com/p/9c3a27c831e8)

[使用git将本地项目推送到远程仓库github - 简书](https://www.jianshu.com/p/b1f9f684fac8)

[将本地文件夹添加到Git仓库 - 坚守梦想 - 博客园](https://www.cnblogs.com/mkl34367803/p/11220465.html)

[(1条消息)Git将本地文件夹添加到远程仓库_StarFishing-CSDN博客](https://blog.csdn.net/qq_34803821/article/details/86648313)

[git将本地文件上传到远程github仓库中丶Java教程网-IT开发者们的技术天堂](https://www.liangzl.com/get-article-detail-11850.html)

[Git关联远程仓库 - 冬音 - 博客园](https://www.cnblogs.com/wintertone/p/11650058.html)

[git报错：'fatal:remote origin already exists'怎么处理？附上git常用操作以及说明。 - leaf+ - 博客园](https://www.cnblogs.com/leaf930814/p/6664706.html)

[(5条消息) git仓库更换远程地址_IT水很深，路还很长-CSDN博客](https://blog.csdn.net/S3328047358/article/details/98183662)

[git remote 命令  菜鸟教程](https://www.runoob.com/git/git-remote.html)



## git 创建本地分支，推送至远程仓库

```shell
# 1，本地新建分支
# git branch xxx && git checkout xxx
git checkout -b xxx
# 2，将本地分支推送至远程仓库
git push -u origin xxx
```



## git clone 拉下来的远程仓库代码一般都处于主分支上，如何切换至其他分支

```shell
# 1, 拉取远程仓库代码
git clone git@git.code.tencent.com:sgc-ins-os/ui-app.git

# 2, 在 VsCode 中可通过 Git Graph 等插件找到从当前分支切出 sg8k 分支的那一次提交的 commitId；回退到到那一次提交；这样做的主要目的是为了避免产生新的合并记录。
git reset --hard xxx 

# 3, 创建并切换到 sg8k 分支
# git branch sg8k && git checkout sg8k
git checkout -b sg8k

# 4, 将远程 sg8k 分支设为本地 sg8k 分支的上游分支
git branch --set-upstream-to=origin/sg8k sg8k

# 5, 拉取代码
git pull
```

参考博文:

[(7条消息) 如何拉取 GitLab 上指定分支的内容？（附常用 git 命令）_Bule_daze的博客-CSDN博客_git怎么pull分支上的内容](https://blog.csdn.net/Bule_daze/article/details/103272403)

## git 下载远程仓库指定分支的代码

```shell
# 下载远程仓库 dev 分支上的代码
git clone -b dev git@gitee.com:admvli2016/git-test.git
```

参考博文：

[通过git命令行从github上下载指定branch的项目源码 - zhang168w520的博客 - CSDN博客](https://blog.csdn.net/zhang168w520/article/details/79766824)


## git 常规提交流程 & 注释规范

7.1 git 常规提交流程

```shell
# 1，添加修改
git add . || git add *

# 2，commit 提交
git commit -m "备注" || git commit -am "备注"

# 3，拉取其他开发人员的更新内容
git pull

# 4，推送到远程分支
git push
```

7.2 git 注释规范

| 注释开头 | 含义                                                         |
| :------- | :----------------------------------------------------------- |
| feat     | 新功能的开发                                                 |
| fix      | 测试提的 bug 修复；开发过程中已开发功能发现的问题的修复（对测试提的 bug 需在注释末尾写明 Bug ID） |
| docs     | 文档类修改                                                   |
| style    | 格式化、分号增删等修改，代码没有变动                         |
| refactor | 代码重构：同样的功能点，只是逻辑上重构了该功能的编码实现     |
| chore    | 依赖包升级，代码迁移、初始化、覆盖等，业务代码没变动         |

备注：1、举例，提交时注释     

git commit -m "feat: 轨迹图找点功能"       

git commit -m "fix: 修复轨迹图XX问题"



## git stash 暂存修改

```shell
# 添加文件
git add . || git add *

# 将当前所有修改内容(未提交的)暂存，此时代码回到上一次的提交
git stash

# 列出所有暂存项
git stash list

# 删除队列中某一暂存项
git stash drop stash@{0} # 删除最近的暂存项
git stash drop stash@{1} # 删除第二近的暂存项
git stash drop stash@{2} # 删除第三近的暂存项
# 依此类推

# 清除所有暂存项
git stash clear

# 将暂存的修改重新应用，可以看到以前暂存的修改又回来了
git stash apply

# 应用队列中某一暂存项
git stash apply stash@{0} # 应用最近的暂存项
git stash apply stash@{1} # 应用第二近的暂存项
git stash apply stash@{2} # 应用第三近的暂存项
# 依此类推 

# 恢复暂存的修改，同时删除 stash 记录
# git stash apply stash@{0} && git stash drop stash@{0}
git stash pop  
```

参考博文：

[(5条消息) git stash暂存修改_大掌教的Cocos Creator研究院-CSDN博客](https://blog.csdn.net/dawn_moon/article/details/6977388)

[(2条消息) git 删除stash 的内容_csdnmuyi的博客-CSDN博客](https://blog.csdn.net/csdnmuyi/article/details/80237173)

##  git 给某次提交打上 tag & 删除 tag

9.1 打上 tag

```shell
# 打 tag 流程分析
git add *
git commit -am "xxx"

# git pull 不仅会同步修改的内容还会同步 tags
git pull

# 只提交修改的内容，不会提交 tag
git push 

# 给最近的提交打上 tag
git tag v1.1.x 

# 只提交 tags，不会提交修改的内容
# 会跟远程的 tags 进行比较，将所有新增的 tags 都推送到远程
git push --tags  

# 只提交 tag，不会提交修改的内容
# 将具体的某一个 tag 推送至远程
git push origin v1.1.0
```

- git push 是不包含 tag 的；如果想包含可以在 push 时加上 --tags 参数，但是此时只会提交 tag，不会提交修改的内容
- git push --tags 与 git push origin v1.1.0 的区别在于 git push --tags 会跟远程的 tags 进行比较，将所有新增的 tags 都推送至远程（增量更新）
- git pull 不仅会同步修改的内容还会同步 tags

9.2 删除 tag

```shell
# 移除 tag
git tag -d v1.1.x # 删除本地 tag
git push origin :refs/tags/v1.1.x  # 删除远程 tag
```

9.3 具体的打 tag 流程

```shell
# 提交修改的内容
git add *
git commit -am "xxx"
git pull
git push

# 打 tag，并将其推送至远程仓库
git tag v1.5.8.1
git push origin v1.5.8.1
```

参考博文：

[Git - 打标签](https://git-scm.com/book/zh/v2/Git-基础-打标签)

[git 打tag标签_倒骑驴走着瞧的博客-CSDN博客_git打tag](https://blog.csdn.net/nongminkouhao/article/details/106272776)

[git删除tag - 极_地 - 博客园](https://www.cnblogs.com/jidi/p/10105201.html)

## git cherry-pick 将一个分支上的某些提交应用到另一个分支

```shell
# 切换到想要应用修改的分支 xxx
git checkout xxx

# 应用一个分支的某一个提交到 xxx 分支
git cherry-pick <commit-id>

# 应用一个分支的多个提交到 xxx 分支，提交之间使用空格分隔
git cherry-pick <commit-id1> <commit-id2> <commit-id3> ...

# 应用一个分支中连续的多个提交到 xxx 分支
# 此语法对应的操作区间是左开右闭，不包含 start-commit-id
# 另外要注意这两个commit 之间要求有连续关系的，并且前者要在后者之前，顺序不能颠倒
git cherry-pick <start-commit-id>..<end-commit-id>

# 应用一个分支中连续的多个提交到 xxx 分支
# 此语法对应的操作区间是闭区间，包含 start-commit-id
git cherry-pick <start-commit-id>^..<end-commit-id>

# 应用该分支最近的提交到 xxx 分支
git cherry-pick <branch-name>

# cherry-pick 其它命令
# 当 cherry-pick 多个提交时，假设遇到冲突：
# --continue 继续进行下一个
# --quit 结束 cherry-pick 操作，但是不会影响冲突之前多个提交中已经成功的
# --abort 直接打回原形，回到 cherry-pick 前的状态，包括多个提交中已经成功的
git cherry-pick --continue 
git cherry-pick --quit 
git cherry-pick --abort 
```

参考博文：

[(5条消息) Git-用 cherry-pick 挑好看的小樱桃_段浅浅的博客-CSDN博客_git pick-cherry](https://blog.csdn.net/qq_32452623/article/details/79449534)

[(2条消息) Git：cherry-pick应用一个分支某些现有提交，到另外一个分支_琦彦-CSDN博客](https://blog.csdn.net/fly910905/article/details/89371517)

## git 删除远程分支上的某次提交

只讨论最简单也是最常用的一种情形：

**删除最后一次提交（远程仓库版本回滚要提醒其它开发人员保存好自己本地的修改内容）**

有两种方式 revert 和 reset

```shell
# revert 方式
# 备份最后一次提交的内容
git branch xxx  # 从当前分支切一个新分支

# 放弃指定提交的修改内容，但是会生成一次新的提交，需要填写提交注释，以前的提交记录都在
# 放弃最后一次提交
git revert HEAD

# 将生成的新提交 commit 推送到远程
git push
```

```shell
# reset 方式
# 备份最后一次提交的内容
git branch xxx  # 从当前分支切一个新分支

# 将 HEAD 指针指到指定提交，历史记录中不会出现放弃的提交记录
# 让 HEAD 指针回到上一次的提交commit，即放弃最后一次提交
git reset --hard HEAD^ || git reset --hard HEAD~1

# reset 之后本地库落后于远程库一个版本，因此需要强制提交 -f
git push -f
```

参考博文：

[(2条消息) git 删除远程分支上的某次提交_张小强的专栏-CSDN博客_git删除远程某次提交](https://blog.csdn.net/qqxiaoqiang1573/article/details/68074847)

[Git删除master branch中最近一次的提交 - 新西兰程序员 - 博客园](https://www.cnblogs.com/wphl-27/p/8607661.html)

## git 删除本地分支上的某次提交

不讨论特别复杂的情况，仅考虑删除最后一次提交

```shell
# 删除最后一次的提交记录，但是还保留提交所做的更改
git reset --soft HEAD^ || git reset --soft HEAD~1
```

```shell
# 删除最后一次的提交记录，并且不保留提交所做的更改
git reset --hard HEAD^ || git reset --hard HEAD~1
```

参考博文：

[【转】Git删除commit提交的log记录 - 程序小工 - 博客园](https://www.cnblogs.com/zqunor/p/8620335.html)

[Git撤销本地commit_K.Sun-CSDN博客_git 删除本地commit](https://blog.csdn.net/sinat_36246371/article/details/79995661)

## git branch 删除分支 & 恢复分支

13.1 查看所有分支

```shell
# 查看所有分支
git branch -a
```

13.2 删除与恢复本地分支

```shell
# 查看本地分支列表
git branch
# 删除本地分支
git branch -d xxx  # 删除不是当前分支的其它分支
git branch -D xxx  # 强制删除当前分支
 
# 恢复本地分支
# 查找被删除分支的 commmit-id
git reflog
# 恢复被删除的分支
git branch xxx commit-id
```

13.3 删除本地的remotes分支

```shell
# 查看所有分支
git branch -a
# 删除本地的remotes分支
git branch -r -d origin/xxx
```

13.4 删除远程分支

```shell
# 查看远程分支列表
git branch -r
# 删除远程分支
git push origin --delete xxx 
```

参考博文：

[Git删除分支/恢复分支 - uTank - 博客园](https://www.cnblogs.com/utank/p/7880441.html)

[(18条消息) git命令行删除远程分支_枫竹梦-CSDN博客](https://blog.csdn.net/furzoom/article/details/53002699)

[git删除本地分支和远程分支 - 安静的小龙码 - 博客园](https://www.cnblogs.com/lwcode6/p/11084537.html)

## git 通过 tag 回退版本修复 bug

背景：线上版本出现 bug，需要紧急修复这个 bug，然后马上发版本，可是这个时候代码的新功能已经开发到一半了，不能回退，遇见这种情况怎么办？

参见上述博文，步骤如下：

14.1 当前分支回退到打 tag 的那次提交，比如回到 tag v1.0 对应的那次提交

- 获取 tag v1.0 对应的那次提交的 commit-id 

```shell
git show v1.0
```

对应的 commit-id 为 16c0866879541a489c83532ccbe7926984ad46d0

这一步也可以通过 Git Graph 等工具直接查看

- 回退到打 tag 的那次提交

```shell
git reset --hard 16c0866879541a489c83532ccbe7926984ad46d0
```

14.2 创建新分支 xxx，备份当前内容

```shell
# 创建新分支 xxx
git branch xxx
```

14.3 当前分支立即回到最近的一次提交

通过 git reflog 查看当前分支最近一次提交对应的 commit-id

```shell
# 查看最近一次提交的 commit-id
git reflog
```

同样也可以通过 Git Graph 等工具查看当前分支最近一次提交的 commit-id

对应的 commit-id 为 271aa56c58ad00c8768e83b515f693f22ff2fe28

```shell
# 回到最近一次提交
git reset --hard 271aa56c58ad00c8768e83b515f693f22ff2fe28
```

14.4 切换到 xxx 分支，修改 bug，发版本，打新 tag

```shell
# 切换到 xxx 分支
git checkout xxx
```

注意这里的发版本是指开发人员自己发版，配置 nginx 等

```shell
# 修改完后打 tag 
git add *
git commit -am "fix: xxx"
git tag v1.4 // 打上新 tag
```

14.5 将 xxx 分支合并到主干

```shell
# 切换到 master 分支
git checkout master
# 将 xxx 分支合并到主干
git merge xxx
```

有冲突处理冲突，再次提交

```shell
git add *
git commit -am "合并冲突"
git pull
git push
```

14.6 推送新 tag 到远程仓库

```shell
# 将新 tag 推送至远程仓库
git push --tags || git push origin v1.4
```

如此通过 tag 回退版本修复 bug 的整个流程就完成了，可继续保留本地分支 xxx ，以防需要再次进行修改

参考博文：

[Git高级教程 (一)\] 通过Tag标签回退版本修复bug_梧桐那时雨-CSDN博客_git通过tag回退](https://blog.csdn.net/fuchaosz/article/details/51698896)

## git 恢复 git reset --hard xxx 之前的 commit-id 对应的内容

**与 14 不同，15 在 git reset --hard commit-id 操作之后，在当前分支上进行了修改**

步骤如下：

15.1 暂存当前修改的内容

```shell
# 暂存修改的内容
git stash
```

15.2 使用 git reflog  或者查看 .git/logs/refs/heads/branch_name 文件找到想要回去的 commit-id

- 执行命令 git reflog
- 文件路径： .git/logs/refs/heads/branch_name

15.3 新建分支，恢复 git reset --hard 之前的 commit-id 对应的内容

```shell
# 新建分支 branch_name，恢复 commit-id 对应的内容
git checkout commit-id -b branch_name
```

参考博文：

[git reset --hard 回滚以后 以后怎么再回去？ - SegmentFault 思否](https://segmentfault.com/q/1010000002984945)

[恢复git reset --hard之前的commit号 - HappyMrSpring - CSDN博客](https://blog.csdn.net/liufuchun111/article/details/79892524)

[版本回退 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)