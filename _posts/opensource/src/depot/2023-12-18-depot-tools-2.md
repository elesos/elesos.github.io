---
layout: post
title: depot_tools系列2:gclient介绍
date: 2023-12-18 10:00:00 +0800
categories: [depot_tools系列]
tags: [depot_tools系列]
---
代码checkout工具,是一个Python脚本

```bash
gclient help [sync] 帮助和子命令帮助
```

它可以在checkout代码后运行Hooks

.gclient]文件是通过`gclient config <url>`生成的，或手动创建的，跟src目录同级，其中 -unmanaged参数表示 unmanaged mode （is the default），在.gclient文件的managed字段有这个标志，

默认会用到 DEPS文件，

DEPS文件指定依赖关系：

deps变量：子依赖，key是要checkout的目录，

hooks：sync之后要运行的

gclient sync：

同步代码，主要根据`DEPS`文件来进行，

`--nohooks` 表示拉取代码之后不执行hooks

`-no-history`不拉取git提交的历史信息

gclient runhooks：

执行hooks。当你拉取代码时使用了`--nohooks`参数时，就可以使用该命令来手动执行hooks

一般步骤就是config、sync、runhooks

## 参考
<https://www.chromium.org/developers/how-tos/depottools/>

<https://chromium.googlesource.com/chromium/tools/depot_tools/+/HEAD/README.gclient.md>

<https://www.chromium.org/developers/how-tos/get-the-code/gclient-managed-mode/>