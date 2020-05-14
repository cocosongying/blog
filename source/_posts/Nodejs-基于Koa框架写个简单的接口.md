---
title: Nodejs-基于Koa框架写个简单的接口
meta:
  donate: true
categories: [编程学习]
tag: [Nodejs]
abbrlink: fb4f
date: 2020-05-14 12:02:44
---

本文主要介绍如何使用 Nodejs 快速搭建服务端并提供简单的接口程序。

<!-- more -->

## 准备工作
首先我们创建一个项目文件夹 `demo`，进入这个文件夹，运行 `npm init` 初始化项目
```bash
mkdir demo
cd demo
npm init
```

执行 `npm init` 命令会让你填写项目相关信息，可以一路默认，之后再改都可以。它主要就是在项目中新建一个 `package.json` 的文件并初始化一些项目信息。

其次需要安装 `Koa` 框架及一些插件
```
npm install koa --save
npm install koa-router --save
npm install koa-bodyparser --save
```

## 编写主程序
新建文件 `index.js`，这个文件名可以修改，同步修改一下 `package.json` 里 `main`项对应的文件名，表示这个文件是启动入口。`index.js` 代码如下：
```
const Koa = require('koa');
const router = require('koa-router')();
const bodyParser = require('koa-bodyparser');
const app = new Koa();

const index = router.get('/demo', ctx => {
    ctx.response.body = { code: 0, data: 'Hello World!' };
}).routes();

app.use(index);
app.use(bodyParser());

// 捕获未定义路由
app.use((ctx) => {
    ctx.response.body = { code: 404 };
});

// 启动项目
app.listen(3000, () => {
    console.log(`Server is starting at port 3000`);
});
```

## 运行使用
在 node 环境中可以直接通过下面的命令启动
```bash
node index.js
```

项目启动完毕就可以访问我们刚刚定义的接口 `http://localhost:3000/demo` 来查看返回信息了。
```
{code: 0, data: "Hello World!"}
```

## 跨域问题
前端页面调用接口可能会有跨域问题，解决方案有好几种，这里介绍简单一点的。

先安装一个插件
```bash
npm install koa2-cors --save
```

在启动入口文件 `index.js` 加上这两句就可以了。
```bash
const cors = require('koa2-cors');
app.use(cors());
```

## 使用心得
Nodejs 这个脚本语言还是很强大的，能写前端就算了，居然还能写服务端。想想之前用 java 的时候要写好多东西，还有各种配置，最后打包运行花费好长时间。

当然 java 有它的强大，如果只是学习写写简单的接口，像 `Nodejs` `Python` `Go` 都有很简单的操作方法，也够简单的使用了。
