---
title: Docker安装MySQL
meta:
  donate: true
categories: [编程学习]
tag: [docker, mysql]
abbrlink: ef32
date: 2020-05-08 16:00:00
---

介绍一下如何通过 docker 安装 mysql

<!-- more -->

MySQL 是最流行的关系型数据库管理系统。这里介绍一下如何通过 docker 来安装。

## 查看可用版本
我们首先通过 MySQL 镜像库来查询有哪些可用版本。
{% note link, [MySQL 镜像库](https://hub.docker.com/_/mysql?tab=tags) %}

## 获取镜像
可以通过下面的命令获取最新版本的镜像
```bash
docker pull mysql:latest
```
往往我们需要知道版本号，可以通过指定版本号的方式获取指定镜像
```bash
docker pull mysql:8.0
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
docker run -it -p 3306:3306 \
    -v /mojipanda/docker/mysql/conf:/etc/mysql/conf.d \
    -v /mojipanda/docker/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    --name mysql \
    --restart=always \
    -d mysql:8.0
```

介绍一下主要执行参数：

| 执行参数 | 说明 |
| :----- | :--- |
| `-p 3306:3306` | 指定端口号 |
| `-v` | 挂载本地目录 |
| `-e MYSQL_ROOT_PASSWORD=123456` | 设置root用户的密码 123456 |
| `--name mysql` | 设置容器名称为 mysql |
| `--restart=always` | 设置容器重启后自动启动 |
| `-d mysql:8.0` | 指定镜像 |

## 启动成功
通过下面的命令查看容器的运行信息
```bash
docker ps
```

> 到这里我们的 mysql 就已经成功安装部署好了。
