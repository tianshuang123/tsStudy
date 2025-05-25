# GIT

## 1.git概述

Git是一个免费的开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。  也是[Linus Torvalds（林纳斯·本纳第克特·托瓦兹linux版本之父）](https://baike.baidu.com/item/Linus Torvalds/9336769?fromModule=lemma_inlink)为了帮助管理Linux内核开发而开发的一个开放源码的版本控制软件。

git易于学习，占地面积小，性能极快。他具有廉价的本地库，方便的暂存区和多个工作流分支等特性。其性能优于Subversion、CVS、Perforce和ClearCase等版本控制工具。

### 1.1何为版本控制

版本控制是一种记录文件内容变化，以便将来来查阅特定版本修订情况的系统。

版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，方便版本切换。

### 1.2为什么需要版本控制

个人开发过渡到团队协作

### 1.3版本管理工具

分布式（git）和集中式版本管理工具（svn）

### 1.4git和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般称之为远程库

局域网：gitlab

互联网：github    gitee



### 1.5git工作机制

![image-20221201212606232](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221201212606232.png)

## 2.git安装

下载地址

https://git-scm.com/download/win

傻瓜式安装一直下一步

![image-20221201213701246](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221201213701246.png)

## 3.Git常用命令

| 命令名称                                          | 作用                                 |
| :------------------------------------------------ | ------------------------------------ |
| git config --global user.name  ts                 | 设置用户签名                         |
| git config --global user.email tian970623@163.com | 设置用户签名                         |
| git init                                          | 初始化git库                          |
| git status                                        | 查看本地库状态                       |
| git add file                                      | 添加到缓存区git追踪到                |
| git rm --cached file                              | 删除暂存区文件                       |
| git commit -m "first commit"  file                | 提交到本地库                         |
| git reflog                                        | 查看引用日志信息（版本号前七位精简） |
| git log                                           | 查看引用日志信息（完整版本号）       |
| git reset --hard 版本号                           | 穿越到对应版本                       |



````shell
F:\tsCode\gitTest\.git\refs\heads\master  #查看当前指针所指的版本号
F:\tsCode\gitTest\.git\HEAD               #查看当前指针所指向的版本文件  head指向分支，分支指向版本
````



## 4.Git分支操作

### 4.1什么是分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是个单独的副本。(分支底层其实也是指针的引用)。

![image-20221201232807740](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221201232807740.png)

### 4.2分支的好处

同时并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

### 4.3分支的操作



| 命令名称            | 作用                         |
| ------------------- | ---------------------------- |
| git branch 分支名   | 创建分支                     |
| git branch -v       | 查看分支                     |
| git checkout 分支名 | 切换分支                     |
| git merge 分支名    | 把指定的分支合并到当前分支上 |

master、hot-fix 其实都是指向具体版本记录的指针。当前所在的分支，其实是由HEAD决定的。所以创建分支的本质就是多创建一个指针。
HEAD 如果指向 master，那么我们现在就在 master 分支上。

HEAD 如果执行 hot-fix，那么我们现在就在 hotfix 分支上。

## 5.git团队协作机制

### 5.1团队内协作

![image-20221202004333965](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202004333965.png)



### 5.2跨团队协作

![image-20221202004420720](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202004420720.png)





## 6.Github操作

gitbug网址：https://github.com/

全世界最大的同性交友网站，技术宅男的天堂，新世界的大门

### 6.1创建远程仓库别名

````shell
 git remote -v                          #查看当前所有远程地址别名
 git remote 别名 远程地址                 #创建别名
 git push 别名 分支  
 git push gitTest master                #推送本地至远程库
````

### 6.2拉取远程仓库代码

````shell
 git pull 别名 分支
 git push gitTest master                 #拉取远程仓库至本地
````

### 6.3克隆远程仓库到本地

````shell
git clone https://github.com/tianshuang123/gitTest.git    
#clone会做如下操作：1.拉取代码2.初始化本地仓库3.创建别名
````

### 6.4ssh免密登录

````shell
$ ssh-keygen -t rsa -C tian970623@163.com
#以rsa加密算法
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/25157/.ssh/id_rsa):
Created directory '/c/Users/25157/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/25157/.ssh/id_rsa
Your public key has been saved in /c/Users/25157/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:iqWZHKDLpsLls0hPQxq+vJJ2DaF4z+rnlSNdHpd0Csg tian970623@163.com
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|     . .         |
|  .   E . . .    |
| . o     o +     |
|o...o . S +      |
|+o=+ O = o       |
|oOo=X * .        |
|Oo=+=+ .         |
|=*=*+            |
+----[SHA256]-----+

````

![image-20221202203627765](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202203627765.png)

![image-20221202203734544](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202203734544.png)



## 7.idea集成git

### 7.1配置git忽略文件

````git.ignore
# Compiled class file
*.class
# Log file
*.log
# Blue] files
*.ctxt
# Mobile Tools for Java (J2ME)
*.mtj.tmp/
# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, 
hs_err_pid*

*.classpath
.project
.settings
target
.idea
*.iml
````

在C:\Users\25157\.gitconfig加入(需要用正斜线)

````shell
[core]
	excludesfile = C:/Users/25157/git.ignore
````

### 7.2定位git程序

![image-20221202212908946](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202212908946.png)

![image-20221202213045809](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202213045809.png)



![image-20221202213416247](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202213416247.png)



点击vcs(version controller setttings)---import into version controller---create git repository



### 7.3idea提交代码

git-----add-----不强制提交忽略

![image-20221202221505224](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202221505224.png)

 

git----commit  file  

![image-20221202221714204](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202221714204.png)



### 7.4查看版本信息

![image-20221202222256839](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202222256839.png)

下方的的version control --- log

### 7.5切换版本

选择版本右键 checkout revision ‘版本号'

![image-20221202222440437](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202222440437.png)

### 7.6创建分支

![image-20221202223143829](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202223143829.png)



### 7.7切换分支

![image-20221202225056052](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202225056052.png)



### 7.8合并分支

将新建分支合并到master分支

![image-20221202225628783](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202225628783.png)



无冲突自动合并，有冲突

![image-20221202230631503](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202230631503.png)



左边位master分支代码，右边为2022122分支代码，中间为无冲突的代码

![image-20221202230813233](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202230813233.png)



## 8.idea集成github

### 8.1设置github账号

![image-20221202231250012](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202231250012.png)



![image-20221202231333097](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202231333097.png)



**账号密码可能登不上去**

**需要用口令use token**

![image-20221202232307784](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202232307784.png)





![image-20221202232608406](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202232608406.png)

口令只显示一次，及时复制。否则需要重新生成。



### 8.2分享项目到github

![image-20221202232944319](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202232944319.png)



![image-20221202233241098](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202233241098.png)





### 8.3push推送本地库到远程库

**https方式push**

![image-20221202233732750](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202233732750.png)

---

**ssh方式push**

![image-20221202234043607](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202234043607.png)



注意: push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，push 的操作是会被拒绝的。也就是说，要想 push 成功，一定要保证本地库的版本要比远程库的版本高! 因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地代码的区别! 如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送!





### 8.4拉取远程库到本地库

![image-20221202235224983](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221202235224983.png)



### 8.5clone 克隆远程库到本地

![image-20221203000216778](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221203000216778.png)

![image-20221203000257009](C:\Users\25157\AppData\Roaming\Typora\typora-user-images\image-20221203000257009.png)



## 9.自建代码托管中心GitLab

### 9.1安装包准备

**链接：https://pan.baidu.com/s/1OlHaHmD_pr1abV7BP4yc2g?pwd=gaqp 
提取码：gaqp 
--来自百度网盘超级会员V5的分享**

















