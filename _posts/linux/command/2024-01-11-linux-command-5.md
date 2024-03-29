---
layout: post
title: Linux命令系列5:cp复制
date: 2024-01-11 07:00:00 +0800
categories: [Linux命令系列]
tags: [Linux命令系列]
---
cp可复制文件或目录

## 语法

```
cp [选项] 源 目标
```

## 常见选项

```
-i 或 --interactive 覆盖之前询问
-f 或 --force       强行复制
-p 或 --preserve    保留源属性
-R                  递归
-v 或 --verbose     显示复制过程
--help              帮助
```

## 示例

将文件file1复制成文件file2

```
cp file1 file2
```

将目录dir1复制成目录dir2
```
cp -R dir1 dir2
```
## 其它
一般Linux的启动文件~/.bashrc中会把cp命名成

```
alias cp='cp -i'
```
这样在Linux下输入cp命令实际上运行的是cp -i，加上一个“\”符号(即运行\cp)可以让此次的cp命令不使用别名(cp -i)运行。

