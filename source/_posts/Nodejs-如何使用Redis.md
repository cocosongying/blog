---
title: Nodejs-如何使用Redis
meta:
  donate: true
categories: [编程学习]
tag: [Nodejs]
abbrlink: 1ab7
date: 2020-05-15 22:14:47
---

本文主要介绍在 Nodejs 中如何连接并使用 Redis，通过 Nodejs 操作使用 Redis 还是相对比较简单的。

<!-- more -->

## 安装准备
我们需要安装 `redis` 依赖包
```bash
npm install redis --save
```

可以在项目中新建一个 `cache` 文件夹用来管理 `redis` 的相关操作。

## 具体实现
在 `cache` 文件夹下新建 `connection.js`，内容如下：
```js
const redis = require('redis');
const { promisify } = require('util');
const client = redis.createClient({
    host: "localhost",
    password: "123456",
    db: 1,
    retry_strategy: function (options) {
        if (options.error && options.error.code === 'ECONNREFUSED') {
            return new Error('The server refused the connection');
        }
        if (options.total_retry_time > 1000 * 60 * 60) {
            return new Error('Retry time exhausted');
        }
        if (options.attempt > 100) {
            return undefined;
        }
        return Math.min(options.attempt * 100, 3000);
    }
});

const expire = promisify(client.expire).bind(client);
const get = promisify(client.get).bind(client);
const set = promisify(client.set).bind(client);
const del = promisify(client.del).bind(client);

class Cache {
    Timeout = {
        Default: 24 * 60 * 60
    };
    async get(key) {
        return await get(key);
    }
    async set(key, value, timeout) {
        let res = await set(key, value);
        if (timeout) {
            await expire(key, timeout);
        }
        return res;
    }
    async del(key) {
        await del(key);
    }
}

module.exports = new Cache();
```

这里主要是 `redis` 的连接以及 `redis` 的几个简单操作，如获取，设置，删除，如果需要其他功能可以继续封装。

关于 `redis` 的连接配置可以单独写到配置文件里，这里为了方便介绍，把配置写死在示例代码里了。

## 如何使用
在需要操作 `redis` 的文件中先引入
```
const Cache = require('./connection');
```

获取某个 key 对应的值：
```
await Cache.get(key);
```

设置一个 key 对应的 value：
```
await Cache.set(key, value);
```

如果需要指定缓存有效期限，比如 1 天：
```
await Cache.set(key, value, 24 * 60 * 60);
```

## 使用心得
推荐在 `cache` 文件夹下为每个 `key` 新建对应的文件程序来操作 `redis`，把这个 `cache` 当成一个缓存模块，其他地方使用只要引入这个模块操作就好了。这样管理起来方便，代码看起来更清晰。
