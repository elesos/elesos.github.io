---
layout: post
title: Linux常见问题系列2:Ubuntu配置apt源
date: 2024-01-09 06:00:00 +0800
categories: [Linux常见问题]
tags: [Linux常见问题]
---
```bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak #备份
vim /etc/apt/sources.list #修改，里面会有代号，其中trusty是14.04的，16.04是xenial，18.04是bionic，可以通过  cat /etc/lsb-release  查看到
把里面的url都换成 mirrors.aliyun.com

通过全局替换命令：

:%s/源字符串/目的字符串/g

或：

sed -i "s/archive.ubuntu.com/mirrors.aliyun.com/g"  /etc/apt/sources.list
sed -i "s/security.ubuntu.com/mirrors.aliyun.com/g" /etc/apt/sources.list
apt update #更新列表
## apt使用
apt-get update -qq # qq:No output except for errors
```
## 参考
<http://mirrors.163.com/.help/ubuntu.html>

<https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.53322f700fCvX1>