# Git学习笔记

主要学习来源：黑马https://www.bilibili.com/video/BV1MU4y1Y7h5/?p=2&vd_source=b244a15efff210683ea753a21a0b3d89

学习时间：2022年9月23日~2022年9月30日

## 1 前言

一般来说，团队合作开发的话，每个人都需要在自己的功能分支 feat/XXX 上开发，最后一起合并到总的开发分支 dev 上，然后将开发分支 dev 合并到测试分支上，最后将测试分支合并到正式发布分支上。

其中总的开发分支一般叫做 dev 分支，正式发布分支一般是叫 main/master/release 分支。

![image-20220118141526543](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118141526543.png)

比如说有 A、B、C 三个人协助进行功能开发：

1. 首先 A、B、C 三位小伙伴从总开发分支 Dev 上开辟自己的功能分支，分别是 feat/AXXX、feat/BXXX、feat/CXXX，也就是图中 feat/AXXX、feat/BXXX、feat/CXXX 的三条线；

2. 然后在自己的开发机上进行开发，这里的开发机可以是本地环境也可以是一些云端的开发机。开发完毕后，再分别合到总开发分支 dev 上，也就是图中蓝色的三条线，在这个过程中可能会产生一些代码冲突，挨个 solve 即可；

3. 接着在 dev 分支上确认所有功能开发完毕，进行简单自测，fix 一些 bug 后再向测试分支上进行合并；

4. 这个时候就可以艾特测试组的同学来进行测试，测试通过后再合到 master 分支进行发布。

   

   一般来说，基本的流程就是这样的，不同公司或许其中流程有些出入，不过问题不大，大致方向是如此的。

## 2 概述

### 2.1 版本控制的方式

- 集中式版本控制工具：集中式版本控制工具，版本库是集中存放在中央服务器的，team里每个人work时从中央服务器下载代码，是必须联网才能工作，局域网或互联网。个人修改后然后提交到中央版本库。

  举例：SVN和CVS

![image-20220118142532662](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118142532662.png)

分布式版本控制工具：分布式版本控制系统没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样工作的时候，无需要联网了，因为版本库就在你自己的电脑上。多人协作只需要各自的修改推送给对方，就能互相看到对方的修改了。
举例：Git

### 2.2 Git

Git是分布式的，Git不需要有中心服务器，我们每台电脑拥有的东西都是一样的。我们使用Git并且有个中心服务器，仅仅是为了方便交换大家的修改，但是这个服务器的地位和我们每个人的PC是一样的。我们可以把它当做一个开发者的pc就可以就是为了大家代码容易交流不关机用的。没有它大家一样可以工作，只不过“交换”修改不方便而已。

git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。Git是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。 同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。Linux 内核开源项目有着为数众多的参与者。绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991－2002 年间）。到 2002 年，整个项目组开始启用一个专有的分布式版本控制系统 BitKeeper 来管理和维护代 码。

到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。 这就迫使 Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds）基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。 他们对新的系统制订了若干目标：

- 速度
- 简单的设计
- 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 完全分布式
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

![image-20220118142736028](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118142736028.png)



### 2.3 Git工作流程

![image-20220118142757623](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118142757623.png)

命令如下：

1. clone（克隆）: 从远程仓库中克隆代码到本地仓库
2. checkout （检出）:从本地仓库中检出一个仓库分支然后进行修订
3. add（添加）: 在提交前先将代码提交到暂存区
4. commit（提交）: 提交到本地仓库。本地仓库中保存修改的各个历史版本
5. fetch (抓取) ： 从远程库抓取到本地仓库，不进行任何的合并动作，一般操作比较少。
6. pull (拉取) ： 从远程库拉到本地库，自动进行合并(merge)，然后放到到工作区，相当于
7. fetch+merge
8. push（推送） : 修改完成后，需要和团队成员共享代码时，将代码推送到远程仓库



## 3 Git安装与常用命令

Git命令例子都是在Git Bash中演示的，会用到一些基本的linux命令。

### 3.1 Git环境配置

#### 3.1.1 基本配置

下载安装

安装完成后，在电脑桌面（也可以是其他目录）点击右键，如果能够看到如下两个菜单则说明Git安装成功。

![image-20220118143339211](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118143339211.png)

Git GUI：Git提供的图形界面工具

Git Bash：Git提供的命令行工具

当安装Git后首先要做的事情是设置用户名称和email地址。这是非常重要的，因为每次Git提交都会使用该用户信息。

基本配置

设置用户信息
git config --global user.name "XXX" # 设置用户名
git config --global user.email "XXXX" # 设置邮箱
查看配置信息
git config --global user.name
git config --global user.email

#### 3.1.2 为常用指令配置别名

有些常用的指令参数非常多，每次都要输入好多参数，我们可以使用别名。

打开用户目录，创建 .bashrc 文件
touch ~/.bashrc

在 .bashrc文件中输入如下内容：

```
#用于输出git提交日志 

alias git-log='git log --pretty=oneline --all --graph --abbrev-commit' 
#用于输出当前目录所有文件及基本信息 
alias ll='ls -al'
```


注：vi退出编辑时，按esc，输入冒号（英文），然后切换到最后一行模式，最后一行模式决定是否保存文件。例如输入wq保存并退出。

打开gitBash，执行：
source ~/.bashrc

#### 3.1.3 解决GitBash乱码问题

1.打开GitBash执行下面命令

```
git config --global core.quotepath false
```

2.${git_home}/etc/bash.bashrc文件最后加入下面两行

```
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
```



### 3.2 创建本地仓库

要使用Git对我们的代码进行版本控制，首先需要获得本地仓库

在电脑的任意位置创建一个空目录（例如test）作为我们的本地Git仓库
进入这个目录中，点击右键打开Git bash窗口
执行命令git init
如果创建成功后可在文件夹下看到隐藏的.git目录

```
git init
```

### 3.3 基础操作指令

Git工作目录下对于文件的修改(增加、删除、更新)会存在几个状态，这些修改的状态会随着我们执行Git的命令而发生变化。

![image-20220118144908667](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118144908667.png)

本章节主要讲解如何使用命令来控制这些状态之间的转换：

1. git add (工作区 —> 暂存区)

2. git commit (暂存区 —> 本地仓库)

   

   工作区 —> 暂存区

   ![image-20221003105038723](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003105038723.png)

   

   ![image-20221003105118530](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003105118530.png)

   暂存区 —> 本地仓库

   ![image-20221003105238797](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003105238797.png)

   git log 查看日志

   ![image-20221003105359503](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003105359503.png)

#### 3.3.1 status查看修改的状态

作用：查看修改的状态（暂存区、工作区）

命令形式：
git status

#### 3.3.2 add添加工作区到暂存区

作用：添加工作区一个或多个文件的修改到暂存区

命令形式：git add 单个文件名|通配符
git add file.txt # 添加单个文件
git add . # 将所有修改加入暂存区

#### 3.3.3 commit提交暂存区到本地仓库

作用：提交暂存区内容到本地仓库的当前分支

命令形式：git commit -m ‘注释内容’
git commit -m "XXX update"

#### 3.3.4 log查看提交日志

配置的别名git-log就包含了这些参数，所以后续可以直接使用指令 git-log

作用:查看提交记录

命令形式：git log [option] 或者 git-log

—all 显示所有分支
—pretty=oneline 将提交信息显示为一行
—abbrev-commit 使得输出的commitId更简短
—graph 以图的形式显示

![image-20221003110900365](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003110900365.png)

设置别名 git-log

![image-20221003111142957](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003111142957.png)

#### 3.3.5 版本回退

作用：版本切换

命令形式：

```
git reset --hard commitID # commitID 可以使用 git-log 或 git log 指令查看
```

commitID可以选中该id，按鼠标中键复制,回退或者找回只需修改id

#### ![image-20221003111537957](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003111537957.png)

如何查看已经删除的记录？

```
git reflog
```

![image-20221003111728805](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003111728805.png)

#### 3.3.6 添加文件至忽略列表

一般我们总会有些文件无需纳入Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以在工作目录中创建一个名为 .gitignore 的文件（文件名称固定），列出要忽略的文件模式。

```
touch .gitignore
vi .gitignore
*.a      #要忽略的.a
```



![image-20221003111344496](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003111344496.png)

#### 3.3.7 命令使用示例

利用上面初始化好的本地仓库test，新建一个文件file01.txt，并用status查看状态：

```
touch file01.txt
git status
```

添加到暂存区

```
git add file01.txt

或者

git add .
```

提交至本地仓库并查看状态

```
git commit -m "commit file01.txt"
git status
```

查看日志

```
git log
```


修改file01.txt文件内容并查看状态


添加至暂存区并查看状态


提交至本地仓库并查看日志

利用git-log查看提交日志，并回退至第一个版本

```
git-log
git reset --hard XXXXX
```


点击打开file01.txt，发现其中内容没有了。

同样，可以再次回到第二个版本：

新建文件file02.txt，加入.gitignore中

```
touch file02.txt
touch .gitignore
vi .gitignore # 添加内容： file02.txt
```

.gitignore内容

```
# Created by .ignore support plugin(hsz.mobi)
### Java template
*.class

# Mobile Tools for java(J2ME)
.mtj.tmp/

# Package Files
*.jar
*.war
*.ear
*.zip

#virtual machine crash logs,see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

.idea

*.iml*.bak
*.class
*.rar
*.log
*.project
*.settings
*.classpath
target
classes
lib
*.DS_Store
.gradle
build
out
log
```

### 3.4 分支

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

![image-20221003135224638](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003135224638.png)

#### 3.4.1 查看本地分支

```
git branch
```

#### 3.4.2 创建本地分支


```
git branch 分支名
```

#### 3.4.3 切换分支


```
git checkout 分支名
```

![image-20221003135415334](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003135415334.png)

head指向当前分支，分支文件目录分开展示

我们还可以直接切换到一个不存在的分支（创建并切换）：


```
git checkout -b 分支名
```

#### 3.4.4 合并分支

开发两个功能，最终放到一块

一个分支上的提交可以合并到另一个分支


```
git merge 分支名
```

#### 3.4.5 删除分支

不能删除当前分支，只能删除其他分支

```
git branch -d 分支名 # 删除分支时，需要做各种检查
git branch -D 分支名 # 不做任何检查，强制删除
```

#### 3.4.6 解决冲突

当两个分支上对文件的修改可能会存在冲突，例如同时修改了同一个文件的同一行，这时就需要手动解决冲突，解决冲突步骤如下：

1. 处理文件中冲突的地方
2. 将解决完冲突的文件加入暂存区(add)
3. 提交到仓库(commit)

第一步，假设分支dev，修改file01.txt文件update count=2

![image-20221003154859488](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003154859488.png)

添加到暂存区

![image-20221003155124449](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003155124449.png)

第二步，在master中修改file01,update count=3

![image-20221003155319181](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003155319181.png)

![image-20221003155637280](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003155637280.png)

合并，file01自动合并失败

![image-20221003155708604](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003155708604.png)

![image-20221003155809961](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221003155809961.png)

此时需要手动将file01改为自己想要的结果，然后add和commit即可

#### 3.4.7 开发中分支使用原则与流程

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

在开发中，一般有如下分支使用原则与流程：

- master （生产） 分支：线上分支，主分支，中小规模项目作为线上运行的应用对应的分支；


- develop（开发）分支：是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支，准备上线。


- feature/xxxx分支：从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到develop分支，之后该分支可以删除。


- hotfix/xxxx分支：从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、develop分支。


还有一些其他分支，在此不再详述，例如test分支（用于代码测试）、pre分支（预上线分支）等
等。

![image-20220118170619389](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220118170619389.png)

#### 3.4.8 命令使用示例

1） 示例1
查看分支，并新建分支dev01

```
git branch
git branch dev01
```


其中HEAD指向的是当前工作区所处的分支，例如此时处在master的分支上。

将上一节中的.gitignore提交，并查看分支：


将当前分支切换到dev01
SH
1
git checkout dev01


同时观察到，test目录下的.gitignore消失了

重新切换到master分支，.gitignore又出现了。新建分支dev02，并同时切换到上面
SH
1
git checkout -b dev02


切换到dev01分支，并新建文件file03.txt，并提交


切换到master分支，并将dev01的提交合并到master上
SH
1
2
git checkout master
git merge dev01


此时，可以看到在master分支下的仓库中，有了file03.txt

删除dev02分支
SH
1
git branch -d dev02


2） 示例2——解决冲突
删除分支dev01，创建并切换到分支dev上。此时master和dev分支上的file01.txt文件中内容都为update_count = 1
SH
1
2
git branch -d dev01
git checkout -b dev




在dev分支上修改file01.txt文件内容为update_count = 2，并提交


在master分支上修改file01.txt文件内容为update_count = 3，并提交


将dev分支合并到master分支上，发现报错。查看file01.txt内容
SH
1
git merge dev




解决冲突：将file01.txt提示冲突的地方修改成我们想要的内容，例如再次修改成update_count = 3，并再次提交即可。

## 4 Git远程仓库

### 4.1 常用的托管服务

前面我们已经知道了Git中存在两种类型的仓库，即本地仓库和远程仓库。那么我们如何搭建Git远程仓库呢？我们可以借助互联网上提供的一些代码托管服务来实现，其中比较常用的有GitHub、码云、GitLab等。

GitHub（ 地址：https://github.com/ ）是一个面向开源及私有软件项目的托管平台，因为只支持Git 作为唯一的版本库格式进行托管，故名GitHub

码云（地址： https://gitee.com/ ）是国内的一个代码托管平台，由于服务器在国内，所以相比于 GitHub，码云速度会更快

GitLab （地址： https://about.gitlab.com/ ）是一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的web服务，一般用于在企业、学校等内部网络搭建git私服。

### 4.2 操作远程仓库

#### 4.2.1 添加远程仓库

此操作是先初始化本地库，然后与已创建的远程库进行对接。

命令：
远端名称：默认是origin，取决于远端服务器设置
仓库地址：从远端服务器获取此url

```
git remote add <远端名称> <仓库地址>
```

例如：

```
git remote add origin git@gitee.com:czbk_zhang_meng/git_test.git
```

#### 4.2.2 查看远程仓库

命令：

```
git remote
```

#### 4.2.3 推送到远程仓库


```
git push [-f] [--set-upstream] [远端名称] [本地分支名][:远端分支名]
```

命令：

如果远程分支名和本地分支名相同，则可以只写本地分支名

git push origin master


```
+ `-f`  表示强制推送，**一般在公司内没有这个的使用权限**，否则容易冲掉远程仓库的所有代码
+ `--set-upstream` 推送到远端的同时，建立起和远端分支的关联关系。用于第一次推送时。
+ 如果当前分支已经和远端分支关联，则可以省略分支名和远端名
    + git push 将master分支推送到已关联的远端分支
#### 4.2.4 本地分支与远程分支的关联关系

+ 命令：

sh
git branch -vv
```

#### 4.2.5 从远程仓库克隆

如果已经有一个远端仓库，我们可以直接clone到本地。

命令：

```
git clone <仓库地址> [本地目录]
```

本地目录可以省略，会自动生成一个目录

#### 4.2.6 从远程仓库中抓取和拉取

远程分支和本地的分支一样，我们可以进行merge操作，只是需要先把远端仓库里的更新都下载到本地，再进行操作。

抓取命令：

- 抓取指令就是将仓库里的更新都抓取到本地，不会进行合并
- 如果不指定远端名称和分支名，则抓取所有分支。

```
git fetch [remote name] [branch name]
```


拉取命令：

- 拉取指令就是将远端仓库的修改拉到本地并自动进行合并，等同于fetch+merge

- 如果不指定远端名称和分支名，则抓取所有并更新当前分支。

  ```
  git pull [remote name] [branch name]
  ```

  

#### 4.2.7 解决合并冲突

在一段时间，A、B用户修改了同一个文件，且修改了同一行位置的代码，此时会发生合并冲突。

A用户在本地修改代码后优先推送到远程仓库，此时B用户在本地修订代码，提交到本地仓库后，也需要推送到远程仓库，此时B用户晚于A用户，故需要先拉取远程仓库的提交，经过合并后才能推送到远端分支，如下图所示。

![image-20220119174403295](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img7/image-20220119174403295.png)

在B用户拉取代码时，因为A、B用户同一段时间修改了同一个文件的相同位置代码，故会发生合并冲突。

远程分支也是分支，所以合并时冲突的解决方式也和解决本地分支冲突相同相同，在此不再赘述。

### 4.3 命令使用示例

#### 4.3.1 远程仓库添加查看推送

初始化本地库（例如使用上一节创建的本地仓库test），然后与已创建的远程库（例如创建将好一个远程仓库git_test）进行对接

```
git remote add origin git@gitee.com:zeng-yiming/git_test.git
```

查看对接好的远程仓库

```
git remote
```

推送到远程仓库

```
git push origin master
```


因为本地分支名master和远程分支名master同名，所以省略了远程分支名，完整的指令如下：

```
git push origin master:master
```

查看本地分支与远程分支的绑定关系

```
git branch -vv
```


此时尚未有绑定关系

将本地分支master和远程分支master绑定关系

```
git push --set-upstream origin [本地分支名]:[远端分支名]
```

- 强制覆盖
  - git push -f --set-upstream origin [本地分支名]:[远端分支名]
- 如果远程分支名和本地分支名称相同，则可以只写本地分支
  - git push origin master
- --set-upstream 推送到远端的同时并且建立起和远端分支的关联关系
  - git push --set-upstream origin master
- 如果当前分支已经和远端分支关联，则可以省略分支名和远端名
  - git push 将master分支推送到已关联的远端分支

发现本地分支master已经和远程分支master绑定成功

此时本地分支master若要推送到远程分支master上，则可直接使用命令：

```
git push
```



#### 4.3.2 克隆_抓取和拉取

克隆刚才的远程仓库git_test，放在目录git_test中

```
git clone git@gitee.com:zeng-yiming/git_test.git git_test
```


查看克隆仓库的日志


在本地仓库test中新建一个文件file04.txt并提交和推送


发现由于快进模式，远端分支master也进入到了和本地仓库test的master分支相同的位置。

本地仓库git_test从远端仓库抓取

```
git fetch
```

发现远端分支master比本地分支master的版本更新，因此进行合并

```
git merge origin/master
```


步骤4和5的指令可用一条指令完成：

```
git pull
```

#### 4.3.3 解决冲突

在本地仓库test和git_test中同时修改文件file01.txt，并提交，但是test首先推送至远端

在test推送后，git_test首先进行抓取



发现远端分支已经有test的推送结果

git_test进行合并，提示冲突



解决冲突：将file01.txt的内容改成我们想要的，例如就改成5，之后提交并推送即可

### 4.4 配置SSH公钥

- 生成SSH公钥
  - ssh-keygen -t rsa
  - 不断回车
    - 如果公钥已经存在，则自动覆盖
- Gitee设置账户公钥
  - 获取公钥
    - cat ~/.ssh/id_rsa.pub
    - 复制ssh到git或者github的设置中添加
- 验证是否配置成功
  - ssh -T git@gitee.com



### 

## 5 IDEA中的Git使用

### 5.1 配置Git

略

### 5.2 在IDEA中操作Git

场景：本地已经有一个项目，但是并不是git项目，我们需要将这个放到码云的仓库里，和其他开发人员继续一起协作开发。

#### 5.2.1 创建项目的远程仓库

![image-20220119181425292](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119181425292.png)







#### 5.2.2 初始化本地仓库

初始化本地仓库

![image-20220119181801881](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119181801881.png)

选择本项目

![image-20220119181854045](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119181854045.png)

#### 5.2.3 设置远程仓库

查找选项

![image-20220119182056750](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119182056750.png)

输入远程仓库地址

![image-20220119182153941](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119182153941.png)

#### 5.2.4 提交到本地仓库

![image-20220119182359136](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119182359136.png)

#### 5.2.5 推送到远程仓库

![image-20220119182651297](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119182651297.png)

#### 5.2.6 创建分支

方法一：最常规的方式

![image-20220119190427959](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119190427959.png)

方法2：最强大的方式

![image-20220119190603175](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img8/image-20220119190603175.png)

#### 5.2 总结

在IDEA的终端中可使用git命令来完成以上所有功能
切换分支前先提交本地的修改
代码及时提交，提交过了就不会丢

## 6 Git知识补充

### 6.1 rebase和merge

#### 6.1.1 HEAD的理解

HEAD 指向当前所在的分支，类似一个活动的指针，表示一个「引用」。

HEAD 既可以指向「当前分支」的最新 commit，也可以指向历史中的某一次 commit (「分离头指针」的情况)。归根结底，HEAD 指向的就是某个提交点。

当我们做分支切换时，HEAD 会跟着切换到对应分支。

#### 6.1.2 fast-forward 与 —no-ff 的区别

假如有一个场景：有两个分支，master 分支和 feature 分支。现在，feautre 分支需要合并回 master 分支。

![image-20220228134116327](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228134116327.png)

fast-forward 合并方式是条件允许的情况，通过将 master 分支的 HEAD 位置移动到 feature 分支的最新提交点上，这样就实现了快速合并。这种情况，是不会新生成 commit 的。（快进模式）

```
git checkout master # 先切换到master分支
git merge feature # 将feature分支合并到当前分支上（master）
```



![image-20220228134238702](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228134238702.png)

--no-ff 的方式进行合并，master 分支就会新生成一次提交记录。

![image-20220228134456687](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228134456687.png)

BASH

```
git checkout master # 先切换到master分支
git merge --no-ff feature # 将feature分支合并到当前分支上（master）
```


如果条件满足时，merge 默认采用的 fast-forward 方式进行合并，除非你显示的加上 --no-ff 选项；而条件不满足时，merge 也是无法使用 fast-forward 合并成功的！

#### 6.1.3 merge操作

git merge 操作是区分上下文的。当前分支始终是目标分支，其他一个或多个分支始终合并到当前分支。这个注意点记住了，方便记忆！所以，当需要将某个分支合并到目标分支时，需要先切到目标分支上。

条件满足的含义

快进模式能够进行的条件是：源分支和目标分支之间没有分叉。下图则是无法通过 HEAD 的快速移动实现分支的合并。

![image-20220228135612743](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228135612743.png)

如果执行合并操作，默认会尝试 fast-forward 的方式进行合并，但是因为分叉了，所以此时会采用 no-ff 的方式进行合并，有新的 commit 生成了。最终的结果图如下：

```
git checkout master # 先切换到目标分支
git merge feature
```



![image-20220228135749975](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228135749975.png)

#### 6.1.4 rebase操作

rebase 合并往往又被称为 「变基」。这里的「基」就是一个「基点」、「起点」的意思。git rebase 命令通常称为向前移植（forward porting）。

「变基」就是改变当前分支的起点。注意，是当前分支！ rebase 命令后面紧接着的就是「基分支」。与merge操作相反。

变基前：

![image-20220228140250490](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228140250490.png)

执行命令：

```
git checkout feature # 切换到当前分支，或待变基分支
git rebase master # 变基
```

可合并为下面的语句

```
git rebase master feature
```

变基后：

![image-20220228140717295](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228140717295.png)


解释：rebase，变基，可以直接理解为改变基底。feature分支是基于master分支的B拉出来的分支，feature的基底是B。而master在B之后有新的提交，就相当于此时要用master上新的提交来作为feature分支的新基底。实际操作为把B之后feature的提交存下来，然后删掉原来这些提交，再找到master的最新提交位置，把存下来的提交再接上去（新节点新commit id），如此feature分支的基底就相当于变成了E而不是原来的B了。（注意，如果master上在B以后没有新提交，那么就还是用原来的B作为基，rebase操作相当于无效，此时和git merge就基本没区别了，差异只在于git merge会多一条记录Merge操作的提交记录）
推荐使用场景

往公共分支上合代码的时候，推荐使用merge。

拉公共分支最新代码的时候，推荐使用rebase，也就是git pull -r或git pull --rebase，但有个缺点就是rebase以后我就不知道我的当前分支最早是从哪个分支拉出来的了，因为基底变了嘛。

图示

从Develop分支分出两个分支，分属两个人员进行开发。F1分支开发完毕后，push到总分支。F2分支开发到F2_5时需要拉取最新代码。

如果F2分支采用git pull拉取最新代码：

F1分支的视角（F1分支的commit记录）：

![image-20220228144950474](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228144950474.png)

F2分支的视角：这将会把F1分支的修改直接拉下来于本地代码merge，且产生一个commit F2_5，也就是merge commit。

![image-20220228145053996](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228145053996.png)

如果F2分支采用git pull --rebase 拉取最新代码：

F1分支视角不变
F2分支视角：

![image-20220228150132439](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228150132439.png)

### 6.2 强制拉取到本地仓库

有些时候本地仓库提交管理混乱，需要从远程仓库强制拉取，以刷新本地仓库，覆盖所有add和commit操作。可执行以下代码：

```
git fetch --all  
git reset --hard origin/master 
git pull
```

### 6.3 远程分支合并

该项职责由git管理员来完成。例如当开发分支Develop上的所有功能已经完成时，需要合并到master上时：

代码clone到本地仓库

```
git clone
```

在本地创建dev分支并与远程dev分支对应

```
git checkout -b dev origin/dev
```

切换到master分支

```
git checkout master
```


本地的dev合并到master上（遇到冲突解决完后再次提交）

```
git merge dev
```


推送到远程的master上（执行这项操作时，需要有操作远程master分支的权限）

```
git push origin master
```


释放权限：

![image-20220228161136506](https://kisugitakumi.oss-cn-chengdu.aliyuncs.com/img11/image-20220228161136506.png)

