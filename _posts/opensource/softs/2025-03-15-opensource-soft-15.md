---
layout: post
title: 开源软件推荐系列15:RSSHub
date: 2025-03-15 05:20:00 +0800
categories: [开源软件推荐系列]
tags: [开源软件推荐系列]
---
能把各种各样的网站内容转换成RSS格式

RSSHub Radar是配合RSSHub使用的一个浏览器扩展，它能自动识别你正在访问的网站是否有可以在RSSHub上生成RSS Feed，并一键订阅。

在RSSHub中，路由是一个用于获取特定信息源的URL路径。例如，如果你想获取BBC中文网的最新新闻，RSSHub的路由可能是 /bbc/chinese。

测试路由:如果你的RSSHub实例在 http://[NAS的IP地址]:1200，而你找到的GitHub仓库路由是 /github/repos/:user/:repo，则完整的URL可能是 http://[NAS的IP地址]:1200/github/repos/[用户名]/[仓库名]。

在浏览器中打开这个完整的URL，看看是否能获取到信息。

有哪些使用场景
场景一：跟踪多个新闻网站的最新文章

