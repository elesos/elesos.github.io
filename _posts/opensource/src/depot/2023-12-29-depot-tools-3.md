---
layout: post
title: depot_tools系列3:首次运行gclient时报错
date: 2023-12-29 07:15:00 +0800
categories: [depot_tools系列]
tags: [depot_tools系列]
---
首次运行gclient时报错:

```bash
depot_tools\\.cipd_bin\goma_ctl.py", line 403, in communicate
```

这个goma是一个distributed compiler service，可以不需要。

在goma_ctl.py里面

_CheckOutput传入的第一个参数是C:\code\depot_tools\.cipd_bin\gomacc.exe

可以先创建一个分支

```bash
git new-branch xxx 
git map-branches -v
```

然后在update_depot_tools.bat里面屏蔽它。

然后再运行gclient

最后DEPOT_TOOLS_UPDATE=0禁止更新

## 参考
<https://magpcss.org/ceforum/viewtopic.php?f=6&t=19395>