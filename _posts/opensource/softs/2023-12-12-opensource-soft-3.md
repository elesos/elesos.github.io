---
layout: post
title: 开源软件推荐系列3:ohmyzsh
date: 2023-12-12 23:30:00 +0800
categories: [开源软件推荐系列]
tags: [开源软件推荐系列]
---
<https://github.com/ohmyzsh/ohmyzsh>

是一个可以让你的终端更强大的工具，可以简写很多命令，如

```bash
gpr=git pull --rebase // 更多缩写参见 https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git
gcb=git checkout -b
gco= git checkout
```
另外不用cd目录，直接输入目录回车就能进入到对应目录。
## windows上安装oh-my-zsh
1，安装 git bash

2，安装zsh

在下面的网站可以下载到最新的 zsh 安装包：

<https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64>

如果是tar.zst，先在其它平台解压：如macos上
```bash
brew install zstd
unzstd yourfile.tar.zst
```
解压会生成etc与usr目录

将这些文件直接解压到 Git 的安装目录下，与之前的文件夹进行合并

启动 git bash，敲下 zsh，出现%表示进入到了zsh里面。
```bash
zsh --version

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
配置 zsh 为 bash 默认终端

编辑 ~/.bashrc，或者是 C:\Program Files\Git\etc\bash.bashrc，加入下面的几行。
```bash
# Launch Zsh
if [ -t 1 ]; then
exec zsh
fi
```
## 参考：
<https://miaotony.xyz/2020/12/13/Server_Terminal_gitbash_zsh/>
