---
layout: post
title: Linux命令系列3:scp传输文件
date: 2024-01-08 06:00:00 +0800
categories: [Linux命令系列]
tags: [Linux命令系列]
---
从本地复制到远程

```
scp test.tar.gz 192.168.1.11:/opt
```

指定端口：

```
scp -P 60022 nginx.tar.gz 192.168.160.44:/opt/ray/
```

从远程复制到本地

```
scp root@112.126.111.250:/root/test.tar.bz2 /root/
```


