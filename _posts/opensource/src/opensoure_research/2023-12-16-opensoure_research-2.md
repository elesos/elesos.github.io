---
layout: post
title: 开源库调研系列2:new-api
date: 2023-12-16 12:10:00 +0800
categories: [开源库调研系列]
tags: [开源库调研系列]
---
docker-compose up -d 部署后，访问http://服务器IP:3000将自动引导到初始化页面。按照页面指引手动设置管理员账号和密码，后续删除代码目录也没有问题。

PostgreSQL 的所有数据（包括数据库文件、表空间等）都保存在：
/var/lib/postgresql/data （这是 postgres 官方镜像的默认数据目录）

因为在 compose 文件里定义了这个卷：

volumes:
  - pg_data:/var/lib/postgresql/data

并且在文件底部声明了：

volumes:
  pg_data:

意味着 Docker 会自动创建一个名为 pg_data 的 named volume（命名卷）。这个卷的物理文件通常位于宿主机的以下目录：
/var/lib/docker/volumes/pg_data/_data/
（里面会有 base、global、pg_wal 等 PostgreSQL 数据目录和文件）


docker volume ls
# 应该能看到类似这样的：
# local     your_project_pg_data

# 查看详细路径（最有用！）
docker volume inspect your_project_pg_data 
看到 Mountpoint 那行，就是数据库文件真正在宿主机上的位置。

/var/lib/docker/volumes/new-api_pg_data/_data

# 停止服务
docker compose down