---
layout: post
title: Linux命令系列6:rsync复制
date: 2024-01-11 07:10:00 +0800
categories: [Linux命令系列]
tags: [Linux命令系列]
---

```bash
也可以同步文件和目录，可以mac下运行

DIR_SYNC_COMMAND=("rsync" "-rcl")//或-rul, -u 代表按修改日期，-c代表按 checksum. 

echo "DIR_SYNC_COMMAND is set to: ${DIR_SYNC_COMMAND[@]}"

其中DIR_SYNC_COMMAND 设置为包含两个元素的数组 ("rsync" "-rcl")；

 ${DIR_SYNC_COMMAND[@]} 是一种特殊的语法，用于展开数组变量 DIR_SYNC_COMMAND 的所有元素，
并以空格分隔

```
欢迎评论交流