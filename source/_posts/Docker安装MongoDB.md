---
title: Docker安装MongoDB
meta:
  donate: true
categories:
  - 编程学习
tag:
  - docker
  - mongodb
abbrlink: ac76
date: 2020-05-08 17:00:00
---

介绍一下如何通过 docker 安装 mongodb

<!-- more -->

MongoDB 是一个基于分布式文件存储的数据库。这里介绍一下如何通过 docker 来安装。

## 查看可用版本
我们首先通过 MongoDB 镜像库来查询有哪些可用版本。
{% note link, [MongoDB 镜像库](https://hub.docker.com/_/mongo?tab=tags) %}

## 获取镜像
可以通过下面的命令获取最新版本的镜像
```bash
docker pull mongo:latest
```
往往我们需要知道版本号，可以通过指定版本号的方式获取指定镜像
```bash
docker pull mongo:4.2
```

## 查看本地镜像
下面两种执行命令都可以查看本地获取的镜像列表，检查一下是否获取成功
```bash
docker images
```
```bash
docker image ls
```

## 启动容器
镜像获取完毕就可以启动容器了，下面是我安装的时候使用的执行命令
```bash
docker run -it -p 27017:27017 \
    -v /mojipanda/docker/mongo/conf:/data/configdb \
    -v /mojipanda/docker/mongo/data:/data/db \
    --name mongo \
    --restart=always \
    -d mongo:4.2 \
    --auth
```

介绍一下主要执行参数：

| 执行参数 | 说明 |
| :----- | :--- |
| `-p 27017:27017` | 指定端口号 |
| `-v` | 挂载本地目录 |
| `--name mongo` | 设置容器名称为 mongo |
| `--restart=always` | 设置容器重启后自动启动 |
| `-d mongo:4.2` | 指定镜像 |
| `--auth` | 设置访问需要授权 |

## 启动成功
通过下面的命令查看容器的运行信息
```bash
docker ps
```

## 创建用户和设置密码
首先执行下面的命令行进入到容器里并使用 MongoDB 的 shell 连接到 admin 数据库
```bash
docker exec -it mongo mongo admin
```
执行语句创建一个用户名为 `root`，密码为 `123456` 的用户，并授权可以操作所有数据库
```
db.createUser({ user:'root',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'}]});
```

> 到这里我们的 mongodb 就已经成功安装部署好了。
