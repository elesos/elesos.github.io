---
title: git简明教程
tags:
  - git
categories:
  - git
date: 2021-07-09 07:13:01
---

![](images/2021/git.png)

git 默认不区分文件名大小写

```
git config core.ignorecase false  //ignore case 忽略大小写
```

Windows版本: https://git-scm.com/downloads

Git的好处是提交时不需要联网，有网络时再推送即可

如果clone时使用https，推送时需要输入密码，最好用ssh协议

## 初始配置

```
git config --global user.name "your_name"
git config --global user.email your_email
```

查看config配置信息

```
查看系统config  git config --system --list　　
查看当前用户（global）配置 git config --global  --list 
查看当前仓库配置信息 git config --local  --list
```

## 配置别名

以后输入git st就表示git status，可以节省时间

```
git config --global alias.st status && git config --global alias.co checkout 
git config --global alias.ci commit && git config --global alias.br branch
```

## 忽略文件权限变化

在项目里面运行：

```
git config core.fileMode false
```

## 常用操作

```
git add -A && git ci-m "update" && git push -u origin master
```



# Git 解决远程仓库文件大小写问题

https://www.jianshu.com/p/420d38913578

```
git config core.ignorecase false
```

设置本地git环境识别大小写

然后把本地文件改成小写的3rdparty/curl/mac（原来是Mac）

删除Mac文件夹下的所有文件

```
git rm --cached 3rdparty/curl/Mac -r （注意一定要是原来的Mac,不能在Mac上层目录执行！！！）
```



```
git add . 在自己的目录下运行！！！
git commit -m'rm files'
git push 
```

把我坑惨了！！！

现在别人拉代码，会把本地的删除！！！

```
git checkout . # 本地所有修改的。没有的提交的，都返回到原来的状态，慎用！会把自己的修改覆盖！！！
git checkout -- 3rdparty
```

**以后尽量先删除，提交，然后再整理好了，重新提交！**

## 改动

```
git diff readme.txt 
git log
git log --pretty=oneline
```

HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^

回退到上一个版本

```
git reset --hard HEAD^
git reset --hard 1094a  //回到某个版本
```

## 工作区（Working Directory）

电脑里能看到的目录

## 版本库（Repository）

工作区有一个隐藏目录.git，是Git的版本库。里面有暂存区，

add和commit就是在里面。

```
git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别
```

## 撤销

git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

```
git reset HEAD readme.txt 把暂存区的修改撤销掉（unstage），就是撤销add过后的。
git checkout . # 恢复暂存区的所有文件到工作区
git clean -nfd  删除 untracked files,d表示目录，-n 参数先看看会删掉哪些文件，防止重要文件被误删，然后运行git clean -fd真正删除。（最好在子目录下运行！一部分一部分的删除）
```

返回到某个版本，不要本地的修改了。

先show log, 选reset 先第3个hard选项。



### 撤消对文件的修改状态

```
git co -- <file>
```



## 删除文件

```
rm test.txt && git rm test.txt &&  git commit -m "remove test.txt"
```

误删的文件恢复到最新版本

```
git checkout -- test.txt 用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。  也可以作用于目录
```

没有被添加到版本库就被删除的文件，是无法恢复的！

## 分支管理

删除dev分支就是把dev指针给删掉

dev分支的工作完成，我们就可以切换回master分支：

```
git checkout master
git merge dev把dev分支的工作成果合并到master分支上，Fast-forward信息告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
Your branch is ahead of 'origin/master' by 1 commit.提示我们当前master分支比远程的master分支要超前1个提交。
```

## 冲突

如果git merge feature1冲突了。解决后，add,ci,再删除分支

不用Fast forward, --no-ff参数，表示禁用Fast forward：

```
git merge --no-ff -m "merge with no-ff" dev 
```

Git就会在merge时生成一个新的commit,因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

## bug分支

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
git stash //假设此时在dev分支上
```

现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

```
git checkout master
git checkout -b bug-101
```

假如之前在dev分支上，

```
git stash list 查看刚才保存的工作现场
git stash pop
```

# Git 本地服务器

```
git init --bare star.git
cd star.git
pwd
/c/Users/leijh/laravel_code/aa/star.git
```

then

```
git clone /c/Users/leijh/laravel_code/aa/star.git
```

# Git clone仓库的一个子目录

```
cd models
git init
git remote add origin  https://github.com/tensorflow/models.git # 增加远端的仓库地址
git config core.sparsecheckout true # 设置Sparse Checkout 为true 
echo "research/deeplab" >> .git/info/sparse-checkout # 将要部分clone的目录相对根目录的路径写入配置文件
git pull origin master
```

如果只想保留最新的文件而不要历史版本的文件，上例最后一行可以用git pull --dpeth 1命令，即“浅克隆”：

```
git pull --depth 1 origin master 
```

git有个"Spare Checkout"的功能，在checkout的时候，只跟踪符合配置文件.git/info/sparse-checkout里面写入的模式的文件



## 防止重复修复

在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

同样的bug，要在dev上修复，我们只需要把4c805e2 fix bug 101这个提交所做的修改“复制”到dev分支。注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。

cherry-pick命令，让我们能复制一个特定的提交到当前分支：

```
git cherry-pick 4c805e2
```

就不需要在dev分支上手动再把修bug的过程重复一遍。

## 不想要的分支

不想合并的分支，使用大写的-D参数。。

```
git branch -D feature-vulcan
```

## 推送

如果要推送其他分支，比如dev，就改成：

```
git push origin dev
```

别的小伙伴，创建远程origin的dev分支到本地

```
git checkout -b dev origin/dev
```

如果别人已经向远程库push了。那我们push前需要先

```
git branch --set-upstream-to=origin/dev dev 指定本地dev分支与远程origin/dev分支的链接
git pull
```

如果自动合并有冲突，就手动解决。

## Rebase变基

为什么Git的提交历史不能是一条干净的直线？

git pull时有可能会自动生成一次提交。

```
git rebase 分叉的提交现在变成一条直线了！
```

然后push

rebase操作可以把本地未push的分叉提交历史整理成直线；

[https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA](https://git-scm.com/book/zh/v2/Git-分支-变基)

merge合并的结果是生成一个新的快照（并提交）

其实，还有一种方法：你可以提取在 experiment 中引入的补丁和修改，然后在 master 的基础上应用一次。 在 Git 中，这种操作就叫做 变基（rebase）。 你可以使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

```
$ git checkout experiment
$ git rebase master
```

现在回到 master 分支，进行一次快进合并。

$ git checkout master 

$ git merge experiment



### Git pull --rebase

If you pull remote changes with the flag --rebase, then your local changes are reapplied on top of the remote changes.



https://sdqweb.ipd.kit.edu/wiki/Git_pull_--rebase_vs._--merge

It is best practice to always rebase your local commits when you pull before pushing them.

```
git config --global pull.rebase true
```

https://stackoverflow.com/questions/2472254/when-should-i-use-git-pull-rebase



## SourceTree

Atlassian开发

如果本地已经有了Git库，直接从资源管理器把文件夹拖拽到SourceTree上，就添加了一个本地Git库：

也可以选择“New”-“Clone from URL”直接从远程克隆到本地。

### 提交

左侧面板的“WORKSPACE”-“File status”，右侧会列出当前已修改的文件（Unstaged files）

选中某个文件，该文件就自动添加到“Staged files”，实际上是执行了git add README.md命令

然后，我们在下方输入Commit描述，点击“Commit”，就完成了一个本地提交：实际上是执行了git commit -m "update README.md"命令。

### 分支

左侧面板的“BRANCHES”下，列出了当前本地库的所有分支。当前分支会加粗并用○标记。

要切换分支，我们只需要选择该分支，例如master，然后点击右键，在弹出菜单中选择“Checkout master”，实际上是执行命令git checkout master：

要合并分支，同样选择待合并分支，例如dev，然后点击右键，在弹出菜单中选择“Merge dev into master”，实际上是执行命令git merge dev：

### 推送

在SourceTree的工具栏上，分别有Pull和Push，分别对应命令git pull和git push，只需注意本地和远程分支的名称要对应起来

## 如何clone远程的指定分支？

在“远程origin”列表里有所有远端的分支，右键“检出”即可。

在本地分支标签下有全部本地的分支，双击就可以切换。


https://www.liaoxuefeng.com/wiki/896043488029600

# Git add . git add -u git add -A命令区别

```
git add -A  提交所有变化，--all的缩写
git add .  同-A
git add -u 已经被add的文件（即tracked file） ,不会提交新文件（untracked file）。（git add --update的缩写）即： -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
```

![](images/2021/git_add.png)

详细请参见git help add

http://www.cnblogs.com/skura23/p/5859243.html

### 提交到本地

```
git add file_name
git ci -m "message"
git add [dir] # 添加指定目录到暂存区，包括子目录
```

### 修改url

```
git remote set-url origin  new_url
```

### fetch与pull

```
git fetch <远程主机名>   #通常用来查看他人的进程，取回的代码对本地开发代码没有影响
```

默认取回所有分支的更新,如果只想取回特定分支的更新，可以指定分支名。

```
git fetch <远程主机名> <分支名>
```

取回远程主机的更新以后，可以在它的基础上，创建一个新的分支。

```
git checkout -b newBrach origin/master
```

**也可以**使用git merge命令或者git rebase命令，在本地分支上合并远程分支。 如**在当前分支上**，合并origin/master：

```
git merge origin/master
```

或者

```
git rebase origin/master
```

- git pull的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。

```
git pull <远程主机名> <远程分支>:<本地分支>
```

比如，取回origin主机的next分支，与本地的master分支合并，需要写成下面这样。

```
git pull origin next:master
```

这等同于先做git fetch，再做git merge

```
git fetch origin
git merge origin/next
```

### push

提交代码前先下载最新的线上代码并进行合并

```
git pull //会进行2个操作，下载与合并
```

如果有冲突，解决掉并git add和git commit，最后再推送到服务器上。

```
git push <远程主机名> <本地分支>:<远程分支>
```

顺序是<源>:<目的>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。

push时如果该远程分支不存在，则会被新建。

删除origin主机的master分支:

```
git push origin --delete master
```

如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样以后就可以不加任何参数使用git push了

```
git push -u origin master
```

不带任何参数的git push，默认只推送当前分支

将本地的所有分支都推送到远程主机，这时需要使用--all选项：

```
git push --all origin
```

### remote

```
git remote //列出所有远程主机
git remote -v//使用-v选项，可以查看远程主机的网址：
```

添加远程主机

```
git remote add <主机名> <网址>
```

删除远程主机

```
git remote rm <主机名>
```

### branch

查看当前所在分支：

```
git br  //前面有*号的表示当前所在分支, -a查看所有分支,-r选项可以用来查看远程分支，
```

- 分支管理

代码下载后，先在本地创建一个分支

```
git co -b bug4  //创建一个 bug4分支 并切换过去，用以解决编号为4的bug
```

或者创建一个开发新功能的分支

```
git co -b new_feature_name 
```

确定合并这个分支

```
git co master   //切换到主分支
git merge bug4  //合并bug4分支
git br -d bug4 //删除bug4分支
```

其它

```
git checkout -B new_branch <start_point>
```

其中start-point可以是tag, https://git-scm.com/docs/git-branch#Documentation/git-branch.txt-ltstart-pointgt

## Github常用操作

- 添加ssh keys

```
ssh-keygen -t rsa -C "elesos" 
```

然后一路回车，如果有多个，也可以在下面指定文件名：

```
Enter file in which to save the key (/c/Users/elesos/.ssh/id_rsa): elesos
```

点右上角的Settings，然后选择"SSH and GPG keys"，复制id_rsa.pub内容

测试

```
ssh -T git@github.com  //详细的还可以用-vT。-T：Disable pseudo-tty allocation.禁止分配伪终端,当用ssh或telnet等登录系统时，系统分配给我们的终端就是伪终端。
```

- 如果**已经在本地创建了**一个仓库，又想在GitHub创建一个仓库，并且让这两个仓库进行同步

首先，在GitHub上创建一个新的仓库,创建时最好一切都保持默认设置，不要选GPL协议，不然不是空库了。

```
git remote add origin url
```

添加后，origin是远程主机的名字

下一步，把本地库的内容推送到远程库上：

```
git push -u origin master  //把master推送到远程
```

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master推送到远程新的master，还会把本地的master和远程的master关联起来，在以后的推送或者拉取时就可以简化命令(如直接用git push)。

现在，只要本地作了提交add和commit之后，就可以：

```
git push origin master
```

把本地master的最新修改推送至GitHub

- 如何参与开源项目

先“Fork”，然后clone这个fork到自己账号下的库,然后，往自己的仓库推送。

最后在自己的fork库处发起pull request。

- fork了别人的库，如何更新？

```
git remote -v
git remote add upstream source_url //添加别人的库地址
git fetch upstream
git co master
git merge upstream/master   #同步
git push origin master     #更新到自己的github库上
```

## 问题

- client_loop: send disconnect: Broken pipe

上述问题出现在vmware里面的centos8上，修改/etc/ssh/ssh_config文件，在Host *条目下添加 IPQoS=throughput

```
Host *    
   IPQoS=throughput
```

重新启动ssh服务

```
systemctl restart sshd
```

# 常见问题

## Git for Windows乱码

卸载重装即可。

## LF will be replaced by CRLF

Windows平台上： 使用回车（CR）和换行（LF）两个字符来结束一行，回车+换行(CR+LF)，即“\r\n”；

Mac 和 Linux平台：只使用换行（LF）一个字符来结束一行，即“\n”；

许多 Windows 上的编辑器会悄悄把行尾的换行（LF）转换成回车（CR）和换行（LF），或在用户按下 Enter 键时，插入回车（CR）和换行（LF）两个字符。

解决方法1(推荐)： Git 可以在你提交时自动地把回车（CR）和换行（LF）转换成换行（LF），而在检出代码时把换行（LF）转换成回车（CR）和换行（LF）。

```
git config --global core.autocrlf true
```

解决方法2：

在提交时把回车和换行转换成换行，而在检出时不转换。

```
git config --global core.autocrlf input
```

解决方法3： 提交检出均不转换

```
git config --global core.autocrlf false
```

https://www.jianshu.com/p/450cd21b36a4

# Git 子模块

```
[submodule "aPlatform"]
path = aPlatform
url = http://url/aPlatform.git
git clone -b develop --recurse-submodules url
```

子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

```
git submodule add https://github.com/chaconinc/DbConnector 如果你想要放到其他地方，那么可以在命令结尾添加一个不同的路径。
git submodule add git@github.com:webrtc/samples.git webrtc/samples
git config -f .gitmodules submodule.webrtc/samples.branch gh-pages  跟踪子模块仓库的 “gh-pages” 分支
```

如果你已经克隆了项目但忘记了 --recurse-submodules

```
git submodule update --init --recursive
```





```
git pull 命令添加 --recurse-submodules  当你不在那个子目录中时，Git 并不会跟踪它的内容， 而是将它看作子模块仓库中的某个具体的提交。
```

或 git submodule update --remote https://git-scm.com/book/en/v2/Git-Tools-Submodules



## 参考

http://www.ruanyifeng.com/blog/2014/06/git_remote.html

https://help.github.com/articles/testing-your-ssh-connection/

https://help.github.com/articles/configuring-a-remote-for-a-fork/

https://help.github.com/articles/syncing-a-fork/

[https://github.com/geeeeeeeeek/git-recipes/wiki/3.3-%E5%88%9B%E5%BB%BAPull-Request](https://github.com/geeeeeeeeek/git-recipes/wiki/3.3-创建Pull-Request)

http://blog.csdn.net/ygrx/article/details/9109783

http://blog.xiayf.cn/2016/01/18/github-fork-pull-request/

# Linux 搭建 git 服务器

后面注释部分为Ubuntu平台

```
yum install git  #apt install git
```

创建一个git用户，用来运行git服务

```
useradd git  #adduser git
passwd git
```

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

```
git init --bare sample.git   #Git会创建一个裸仓库，裸仓库没有工作区，并且服务器上的Git仓库通常都以.git结尾。
chown -R git:git sample.git
```

将你的id_rsa.pub上传到git服务器，然后用cat命令将内容拷贝到授权文件authorized_keys中。

```
su git 
```

如果.ssh目录不存在，可以运行ssh-keygen -t rsa -C "yourname"

```
touch /home/git/.ssh/authorized_keys   #注意authorized_keys文件权限为git
cat id_ras.pub >> authorized_keys   #注意a single '>' will overwrite all the contents of the second file you specify. A double '>' will append it。
```

If you want to add others to your access list, they simply need to give you their id_rsa.pub key and you append it to the authorized keys file.一行一个 打开RSA认证

```
vim /etc/ssh/sshd_config
RSAAuthentication yes     
PubkeyAuthentication yes     
AuthorizedKeysFile  .ssh/authorized_keys
service sshd restart
```

现在，可以通过git clone命令克隆远程仓库了：

```
git clone git@server:/srv/sample.git
git clone git@192.168.8.34:/data/git/learngit.git
```

如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，可以用Gitosis来管理公钥。

## 管理权限

有很多视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。

因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。

这里不介绍Gitolite了，不要把有限的生命浪费到权限斗争中。

## 权限参考

用户home目录755权限 rwx r-x r-x

.ssh目录700权限 rwx --- ---

authorized_keys 600权限 rx- --- ---

## 参考

https://github.com/elesos/progit/blob/master/zh/04-git-server/01-chapter4.markdown 有GitWeb的介绍

[https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E5%9C%A8%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E6%90%AD%E5%BB%BA-Git](https://git-scm.com/book/zh/v2/服务器上的-Git-在服务器上搭建-Git)
