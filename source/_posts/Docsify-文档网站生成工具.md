---
title: Docsify-文档网站生成工具
meta:
  donate: true
categories:
  - 编程学习
tag:
  - 工具
references:
  - name: Docsify-官网
    url: 'https://docsify.js.org/#/zh-cn/'
  - name: Docsify-Github
    url: 'https://github.com/docsifyjs/docsify/'
abbrlink: 942f
date: 2020-05-06 22:40:30
---

Docsify - 一个神奇的文档网站生成工具

<!-- more -->

## 简介
Docsify 是一个动态生成文档网站的工具。只需要一个 `index.html` 就可以写文档并部署成文档网站，Markdown 文件会在运行时自动转换。

## 简易教程
安装 `docsify-cli` 工具
```bash
npm i docsify-cli -g
```

在项目的 `./docs` 目录里写文档，通过 `init` 初始化项目
```bash
docsify init ./docs
```

项目初始化成功后，会在项目的 `./docs` 目录创建如下几个文件：
+ `index.html` 文档网站入口文件
+ `README.md` 主页内容
+ `.nojekyll` 用于阻止 GitHub Pages 会忽略掉下划线开头的文件

本地预览
```bash
docsify serve ./docs
```

执行完毕可以通过 `http://localhost:3000` 访问预览效果，`docsify serve` 让网站处于实时预览状态，本地有更新，网站会实时刷新。
如果不需要实时刷新，可以使用 `docsify start`
```bash
docsify start ./docs
```

默认端口号是 `3000`，如果需要更改端口号，可以通过在命令行后加 `--port 3001` 来指定端口
```bash
docsify serve ./docs --port 3001
```

`index.html` 里插件使用的源文件 cdn 加速地址，可以将文件下载到本地来引用，这样无法连接外网的环境一样可以部署啦。

## 高级教程
{% note blue, 支持结合 Github 实现在线编辑，添加评论功能等等，更多定制化功能及插件使用请参考官方文档，官方地址见下方参考资料。 %}

## 使用心得
我本人还是很喜欢这种简约的文档网站的，文档系统主要是简洁，既方便写也方便看。Markdown 本身就适合写文档，结合 Docsify 使用简直完美。不需要调整复杂的样式，专注于写就好了。

我在以前的工作中使用过 Docsify 一手搭建部署了文档系统，既可以通过脚本生成接口文档，也可以人工书写设计文档，迭代文档，优化方案等等。
