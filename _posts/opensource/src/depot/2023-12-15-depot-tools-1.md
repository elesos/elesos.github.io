---
layout: post
title: depot_tools系列1:windows安装
date: 2023-12-15 10:00:00 +0800
categories: [depot_tools系列]
tags: [depot_tools系列]
---
它里面有很多工具，如

```bash
fetch --help
gclient help #代码checkout工具
```

## 安装

以下为win上安装文档

win上下载 [depot_tools.zip](https://storage.googleapis.com/chrome-infra/depot_tools.zip) 并解压

或 

```bash
**git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git**
```

将 C:\src\depot_tools 添加到PATH环境变量的最前面

设置DEPOT_TOOLS_WIN_TOOLCHAIN环境变量，设置为0，告诉depot_tools使用本地的vs版本

设置`vs2022_install`环境变量为vs2022安装地址：C:\Program Files\Microsoft Visual Studio\2022\Professional

然后在cmd.exe里面进入depot_tools目录下运行gclient（不需要加.bat后缀，它会安装msysgit and python）

当运行 `gclient`时depot_tools会自动更新，首次更新后，可以设置环境变量DEPOT_TOOLS_UPDATE=0禁用更新

运行下where python确保python已经安装并且在最前面，最好将其它版本python删除。