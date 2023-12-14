---
layout: post
title: Linux命令系列1:软链接ln
date: 2023-12-14 08:00:00 +0800
categories: [Linux命令系列]
tags: [Linux命令系列]
---
软链接类似Windows的快捷方式

```
ln -s 源文件 目标文件(要建立的快捷方式)
```

## 删除软链接

```
ls -alh 
myphpadmin -> /opt/nginx/html/phpmyadmin
```

前面的是目标，所以rm -rf myphpadmin即可，注意后面千万不要加/