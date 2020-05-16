---
title: Nodejs-如何使用MySQL
meta:
  donate: true
categories: [编程学习]
tag: [Nodejs,mysql]
abbrlink: 56c7
date: 2020-05-16 15:28:21
---

本文主要介绍在 Nodejs 中如何连接并使用 MySQL

<!-- more -->

## 安装准备
我们需要安装 `mysql2` 依赖包
```bash
npm install mysql2 --save
```

可以在项目中新建一个 `mysql` 文件夹用来管理 `mysql` 的相关操作。

## 具体实现
在 `mysql` 文件夹下新建 `connection.js`，内容如下：
```js
const mysql = require('mysql2');
const config = require('../config');
const pool = mysql.createPool(config.mysql);
const client = pool.promise();

module.exports = {
    pool,
    client
};
```

这里 `mysql` 的连接配置写在 `config.js` 文件中，示例配置项如下：
```
const initVal = {
    mysql: {
        host: 'localhost',
        port: 3306,
        user: 'username',
        password: '123456',
        database: 'demo',
        charset: 'utf8mb4',
        connectionLimit: 5,
        dateStrings: true,
        decimalNumbers: true,
    }
}

module.exports = initVal;
```

## 如何使用
在需要操作 `mysql` 的文件中先引入
```
const { client } = require('./connection');
```

之后就可以使用 sql 语句进行愉快的操作了
```
async function getById(id) {
    let sql = `select * from demo where id = ?`;
    let args = [id];
    let res = await client.query(sql, args);
    return res[0];
}
```

需要注意的是 `return res[0]` 是返回的结果集，即对于查询有数据时，它返回的是数组。

## 使用心得
推荐在 `mysql` 文件夹下编写数据库连接操作部分，这样代码看起来更清晰。

在接口服务中使用 `mysql` 操作按照上面操作就可以了，如果是脚本操作，使用完毕需要关闭连接
```
client.end();
```
