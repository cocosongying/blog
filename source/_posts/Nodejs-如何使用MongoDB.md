---
title: Nodejs-如何使用MongoDB
meta:
  donate: true
categories: [编程学习]
tag: [Nodejs,mongodb]
abbrlink: 4ef0
date: 2020-05-18 09:54:41
---

本文主要介绍在 Nodejs 中如何连接并使用 MongoDB

<!-- more -->

## 安装准备
我们需要安装 `mongoose` 依赖包
```bash
npm install mongoose --save
```

可以在项目中新建一个 `mongo` 文件夹用来管理 `mongodb` 的相关操作。

## 具体实现
在 `mongo` 文件夹下新建 `connection.js`，内容如下：
```js
const mongoose = require('mongoose');
const mongoUrl = "mongodb://username:123456@localhost:27017/demo";

mongoose.Promise = global.Promise;

function getOptions() {
    return {
        useCreateIndex: true,
        useNewUrlParser: true,
        useFindAndModify: false,
        useUnifiedTopology: true
    };
}

mongoose.connect(mongoUrl, getOptions());
const client = mongoose.connection;
client.on('connected', () => { console.log("mongo connected") });
client.on('reconnected', () => { console.log("mongo reconnected") });
client.on('disconnected', () => { console.log("mongo disconnected") });
client.on('error', (error) => { console.log("connect to " + mongoUrl + "failed") });

function close() {
    client.close();
}

module.exports = {
    client,
    close
};
```

在 `mongo` 文件夹下创建文件夹 `model` 用来记录表定义，`mongo/model/demo.js` 举例如下：
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const demoSchema = new Schema({
    name: String,
    desc: String,
})

const db = mongoose.model('demo', demoSchema, 'demo');
module.exports = db;
```

## 如何使用
在需要操作 `mongodb` 的文件中先引入，如果是 web 接口服务，可以在主程序中引入一次，后面直接操作 `model` 就可以了
```
const conn = require('./connection');
```

举一个查询的例子：
```
const DemoDB = require('./model/demo');
async function getAll() {
    let res = await DemoDB.find();
    return res;
}
```

## 使用心得
推荐在 `mongo` 文件夹下编写数据库连接操作部分，为每个 `model` 编写好操作方法，统一管理，这样代码看起来更清晰，维护起来更方便。

在接口服务中使用 `mongodb` 操作按照上面操作就可以了，如果是脚本操作，使用完毕需要关闭连接
```
conn.close();
```
