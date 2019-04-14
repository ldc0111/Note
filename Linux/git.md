* [一.git配置](#一git配置)
  - [1.配置git用户名和邮箱](#1配置git用户名和邮箱)
* [二.仓库](#二仓库)
  - [1.创建git仓库](#1创建git仓库)
  - [2.查看仓库状态](#2查看仓库状态)
  - [3.远程仓库](#3远程仓库)
  - [4.协同工作](#4协同工作)
  - [5.使用GitHub](#5使用github)
* [三.版本控制](#三版本控制)
    - [1.添加或删除修改](#1添加或删除修改)
    - [2.提交版本](#2提交版本)
    - [3.改动查询](#3改动查询)
    - [4.版本回退](#4版本回退)
    - [5.查看历史提交](#5查看历史提交)


- [四.文件控制](#四文件控制)
  - [1.我撤销修改](#1撤销修改)
  - [2.删除文件](#2删除文件)
  - [3.分支](#3分支)
  - [4.解决冲突](#4解决冲突)
  - [5.分支管理策略](#5分支管理策略)
  - [6.bug分支](#6bug分支)
  - [7.feature分支](#7feature分支)
  - [8.多人协作](#8多人协作)
  - [9.抓去分支](#9抓取分支)
  - [10.报错](#10报错)

<br>
<br>

# 一.git配置

优先级：`--local > --global > --system`

> 用了`--global`这个参数，表示你这台机器上所有的Git仓库都会使用这个配置

### 1.配置git用户名和邮箱

> ```bash
> git config --global user.name        # 查看
> git config --global user.name 用户名  # 修改
> git config --global user.email       # 查看
> git config --global user.email 邮箱   # 修改
> git config -l #查看
> ```
>

<br>

# 二.仓库

### 1.创建git仓库

> ```bash
> git init 仓库名  #创建一个git仓库
> git init        #将一个项目转化为使用git管理（创建.git目录）
> git clone git185@192.168.1.411:./test185.git
> ```
>

示例：

![](../Linux/pic/git-1.png)

目录结构：

> ```bash
> project
>    |------.git
>             |--------branches
>             |--------config        #仓库的配置文件
>             |--------description
>             |--------HEAD
>             |--------hooks
>             |--------info
>             |--------objects
>             |--------refs
> ```
>

> 隐藏目录`.git`不算工作区，而是Git的版本库

### 2.查看仓库状态

> ```bash
> git status
> ```
>

### 3.远程仓库

> 最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分
> 
> 实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交
> 
> GitHub就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库，即Github为我们的git仓库提供了一个远程仓库，有了这个远程仓库，妈妈再也不用担心我的硬盘了

#### 1）为本地与GitHub的通信配置ssh

**本地git仓库和GitHub上的远程仓库之间的传输是通过SSH加密的，所以，需要一点设置**：

1. **创建ssh key**：
    ```bash
    ssh-keygen -t rsa -C "youremail@example.com"
    ```
2. **登录你的GitHub帐号，`Settings -> SSH and GPG keys -> new SSH key` ，将id_rsa.pub的内容复制进去**

> 为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送

#### 2）让本地git仓库和远程仓库同步

> 在有了本地git仓库后，还需创建对应的远程仓库

1. **在GitHub上创建远程仓库**（如果已有则省略）
2. **为本地仓库设置远程仓库信息**（如果同时需要为本地仓库添加多个远程仓库（如果github+码云），则可以将`origin`分别换成`github`和`gitee`，推送操作时也要修改`origin`。添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库）
    ```bash
    git remote add origin https://github.com/用户名/仓库名
    ```
    * **删除本地仓库的远程仓库信息**：`git remote remove origin`
    * **修改远端地址**：`git remote set-url 新地址`
    * **查看远程仓库信息**：`git remote -v`
3. **将本地git仓库push到远程仓库**
    ```bash
    # 由于远程库是空的，我们第一次推送master分支时，加上了-u参数,Git不但会把本地的
    # master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master
    # 分支关联起来，在以后的推送或者拉取时就可以简化命令
    git push [-u] origin 分支名
    ```

并不是一定要把本地分支往远程推送。哪些分支需要推送、哪些不需要呢？

* `master`：主分支，要时刻与远程同步
* `dev`：开发分支，团队所有成员都需要在上面工作，所有也需要与远程同步
* `bug`：只用于在本地修复bug，就没必要推送到远程了，除非老板要看看你每周修复了几个bug

4. 直接修改配置文件：

1. ```c
   进入 SingTel/.git 目录
   
   vim config
   
   修改 [remote “origin”]下面的url即可
   ```

   

### 4.协同工作

拉取分支：

> ```bash
> git pull
> ```
>

`git clone`时，默认情况下只能看到本地的`master`分支。如果要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，可以使用如下命令创建本地`dev`分支：

> ```bash
> git checkout -b dev
> ```
>

将本地`dev`分支与远程`origin/dev`分支关联起来：

> ```bash
> git branch --set-upstream dev origin/dev
> ```
>

### 5.使用GitHub

Bootstrap的官方仓库twbs/bootstrap、你在GitHub上克隆的仓库my/bootstrap，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

![](../Linux/pic/git-2.png)

如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送

**如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了**

<br>

# 三.版本控制

隐藏目录`.git`不算工作区，而是Git的版本库。版本库里存了很多东西，其中最重要的就是**称为stage（或者叫index）的暂存区**。还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

![](../Linux/pic/git-3.png)

### 1.添加或删除修改

- 将修改添加到暂存区：


> ```bash
> git add 文件/目录
> ```
>

- 从暂存区删除修改：


> ```bash
> git rm --cached 文件/目录
> ```
>

- 以下命令可以将暂存区的修改重置，暂存区的改变会被移除到工作区：


> ```bash
> git rest HEAD [文件名]
> ```
>

- 以下命令可以丢弃工作区的修改：


> ```bash
> git checkout -- [文件名]
> ```
>

- 如果刚对一个文件进行了编辑，可以撤销文件的改变，回到编辑开始。命令其实起到“一键恢复”的作用，还可用于“误删恢复”。可以在 `git reset HEAD [文件名]` 后使用


### 2.提交版本

如果修改了readme.txt，添加了文件LICENSE，并将2者添加到暂存区后，暂存区的状态就变成这样：

![](../Linux/pic/git-4.png)

- 使用commit提交修改，实际上就是把暂存区的所有内容提交到当前分支：


> ```bash
> git commit -m '信息'
> ```
>

![](../Linux/pic/git-5.png)

> commit相当于游戏里面一次存档。对应一个版本

### 3.改动查询

> ```bash
> git diff [选项]           # 查看工作区中的修改
> git diff [选项] --staged   # 查看已添加到暂存区的修改
> git diff [选项] HEAD       # 查看当前所有未提交的修改
> 
> 选项：
>     --color-words： 颜色
>     --stat：        不显示具体修改，只显示修改了的文件
> ```
>

### 4.版本回退

> ```bash
> git reset --hard 版本ID/HEAD形式的版本
> 
> git reset --hard HEAD      # 当前版本
> git reset --hard HEAD^     # 上一个版本
> git reset --hard HEAD^^    # 上上个版本
> git reset --hard HEAD~n    # 前n个版本
> git log --prtty=oneline	   # 查看最近到最远提交日志
> git reset --hard 3628df	   # 回退版本号码：回退版本commit id为3628df
> git reflog                 # 想要恢复:使用命令记录来找回commit id回退
> 
> 
> ```
>

如果回到过去的版本，想要回到原来新的版本：

* 如果终端未关，可以找到新版本的id，通过上述命令回去新版本
* 如果终端已关，`git reflog`查看版本，再通过上述命令回去新版本

### 5.查看历史提交

> ```bash
> git log [选项]
> 
> 选项：
>     --online：只显示提交提示信息
>     --stat：添加每次提交包含的文件信息
>     --path：查看每次提交改变的内容
>     --graph
> ```
>

**加文件名可以显示具体文件相关的所有提交信息** 

# 四.文件控制

### 1.撤销修改

> ```bash
> ———>              git add files              git commit  
> working directory--------------stage-------------history
> <———         git checkout --files         git reset --files
> ```
>

###  2.删除文件

> ```bash
> rm read.txt
> （从版本库中删除）
> git rm read.txt
> git commit -m "delete"
> （删错了）
> git checkout -- read.txt 
> ```
>

 

### 3.分支

> ```bash
> (分支名字dev)
> 创建分支：git branch dev
> 切换分支：git checkout dev
> 创建+切换分支：git checkout -b dev
> 查看当前分支：git branch
> 切换回master分支：git checkout master
> 合并指定分支到当前分支：git merge dev
> (fast-forward 快进模式)
> 删除分支：git branch -d dev
> ```
>

 



### 4.解决冲突

> ```bash
> (分支名字fea)
> 冲突原因：master和Dev同时增长。
> git checkout -b fea
> （修改Creating a new branch is quick AND simple.）
> git add read.txt
> git commit -m "fea"
> git checkout master
> （修改Creating a new branch is quick & simple.）
> git addread.txt
> git commit -m "master"
> （此时形成了master和fea各自指着一个分支）
> git merge fea
> （合并错误，git status , cat read.txt可以查看）
> （修改read.txt的文本内容 Creating a new branch is quick and simple.）
> git add read.txt
> git commit -m “conf”
> （现在master和fea指向同一个人点了，git log可查看合并情况）
> git branch -d fea
> ```
>

 

### 5.分支管理策略

> ```bash
> （--no-ff）（分支名字dev 文件名字read.txt）
> git checkout -b dev
> git add read.txt
> git commit -m "dev"
> git checkout master
> （注意下个参数--no-ff,表示禁用fast forward）
> （fast forward合并看不出曾经做过合并，而--no-ff参数合并后的历史有分支，negative看出曾经做过合并）
> git merge --no-ff -m "merge with --no-ff" dev
> （查看分支历史 git log --graph --pretty=oneline --abbrev-commit）
> ```
>

 

### 6.bug分支

> ```bash
> (bugg分支为要解决bug的分支)
> 思想：当手头还有工作时，先将工作现场git stash(避免bug修复好后将为完成的工作一起提交),然后修复bug、提交之后，在用git stash pop将原来的工作显示在工作区 。
> （git status查看状态）
> git stash
> git checkout -b bugg
> （修改bug后）
> git add bugg.txt
> git commit -m "fixed bug"
> git checkout master
> git merge --no-ff -m "merge bug" bugg
> git branch -d bugg
> （接下来回到dev上继续工作）
> git checkout dev
> （查看工作区git status）
> （用git stash list查看）
> git stash list
> （恢复的第一种方法：恢复的同时把stash内容同时删除）
> git stash pop
> （恢复的第二种方法：恢复的同时不删除stash内容）
> git stash apply
> （若使用第二种方法想删除stash则用git stash drop）
> （若是多个文件stash 可用git stash apply stash@{0}恢复指定的stash）
> ```
>

 



### 7.feature分支

> ```bash
> (分支为dev)
> (在没完全完成合并时强行删除)
> git branch -D dev
> ```
>

 



### 8.多人协作

> ```bash
> （查看远程库信息）
> git remote
> （查看远程库更详细信息）
> git remote -v
> ```
>

> ```bash
> 推送dev分支
> git push origin dev
> ```
>

### 9.抓取分支

> ```bash
> (克隆)
> git clone git@github.com:TaylorApril/test.git
> （查看能见分支 git branch）
> （在dev分支上开发，创建远程origin的dev分支到本地）
> git checkout -b dev origin/dev
> （修改后，进行提交）
> git commit -m "add"
> git push origin dev
> （在他提交之后你再push的情况时）
> （指定本地dev分支与远程origin分支链接）
> git branch
> git pull
> git commit -m "fixed"
> git push
> ```
>

 

 

### 10.报错

> ```bash
> 报错1
> 
> $ git push -u origin master
> To git@github.com:TaylorApril/test.git
> ! [rejected] master -> master (fetch first)
> error: failed to push some refs to 'git@github.com:TaylorApril/test.git'
> hint: Updates were rejected because the remote contains work that you do
> hint: not have locally. This is usually caused by another repository pushing
> hint: to the same ref. You may want to first integrate the remote changes
> hint: (e.g., 'git pull ...') before pushing again.
> hint: See the 'Note about fast-forwards' in 'git push --help' for details.
> 
> 
> 解决：
> git pull --rebase origin master
> git push -u origin master
> ```
>

 

> ```bash
> 报错2
> 
> $ git checkout master
> Switched to branch 'master'
> Your branch is ahead of 'origin/master' by 1 commit.
> (use "git push" to publish your local commits)
> 
> 解决：
> git push
> ```
>

