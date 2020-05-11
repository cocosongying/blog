---
title: Docker安装Redis
meta:
  donate: true
categories: [编程学习]
tag: [docker, redis]
abbrlink: a342
date: 2020-05-08 15:00:00
---

介绍一下如何通过 docker 安装 redis

<!-- more -->

Redis 是一个高性能的 `key-value` 数据库。这里介绍一下如何通过 docker 来安装。

## 查看可用版本
我们首先通过 Redis 镜像库来查询有哪些可用版本。
{% note link, [Redis 镜像库](https://hub.docker.com/_/redis?tab=tags) %}

## 获取镜像
可以通过下面的命令获取最新版本的镜像
```bash
docker pull redis:latest
```
往往我们需要知道版本号，可以通过指定版本号的方式获取指定镜像
```bash
docker pull redis:6.0
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
docker run -it -p 6379:6379 \
    -v /mojipanda/docker/redis/redis.conf:/etc/redis/redis.conf \
    -v /mojipanda/docker/redis/data:/data \
    --name redis \
    --restart=always \
    -d redis:6.0 \
    redis-server --appendonly yes --requirepass "123456"
```

介绍一下主要执行参数：

| 执行参数 | 说明 |
| :----- | :--- |
| `-p 6379:6379` | 指定端口号 |
| `-v` | 挂载本地目录 |
| `--name redis` | 设置容器名称为 redis |
| `--restart=always` | 设置容器重启后自动启动 |
| `-d redis:6.0` | 指定镜像 |
| `--appendonly yes` | 数据持久化 |
| `--requirepass "123456"` | 设置访问需要的密码 123456 |

## 启动成功
通过下面的命令查看容器的运行信息
```bash
docker ps
```

> 到这里我们的 redis 就已经成功安装部署好了。
