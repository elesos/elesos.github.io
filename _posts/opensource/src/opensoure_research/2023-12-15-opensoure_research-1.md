---
layout: post
title: 开源库调研系列1:payload
date: 2023-12-15 12:10:00 +0800
categories: [开源库调研系列]
tags: [开源库调研系列]
---
开源的全栈框架 + 无头内容管理系统（Headless CMS）
它可以让你 快速在 Next.js 应用中内置后台管理界面、API、数据库访问、权限系统等后端能力，而不是像传统 CMS 那样仅管理内容。

与传统 CMS（例如 WordPress）不同，它 不关心前端显示，内容以 API 形式输出给你想要使用它的应用。


自动创建后台管理界面（React + Tailwind）

自动生成 REST + GraphQL API

one click to deploy Payload with Workers, R2 for uploads, and D1 for a globally replicated database.
点一下按钮，就能把 Payload 后端完整部署到 Cloudflare 的全球网络上。

Fully self-contained（完全自包含）

意思是：

👉 不依赖外部服务

不用自己买服务器（EC2 / VPS）

不用外接 MongoDB / PostgreSQL

不用接 S3 / OSS

不用单独部署 API + CMS + DB

R2 是 Cloudflare 的对象存储,类似 AWS S3

D1 是什么？

Cloudflare 提供的 Serverless SQL 数据库

基于 SQLite

和 Workers 深度集成