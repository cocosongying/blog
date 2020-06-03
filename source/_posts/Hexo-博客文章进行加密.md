---
title: Hexo-博客文章进行加密
meta:
  donate: true
categories: [实用教程]
tag: [Hexo]
password: mojipanda
abstract: 试一下文章加密，进来看效果～
message: 尝试输入一下密码查看全文：mojipanda
wrong_pass_message: 哦吼，密码不对，请再试试。
wrong_hash_message: 好像有点问题，但也能看看。
abbrlink: '977'
date: 2020-06-03 16:52:02
---

## 简介
本文介绍一下如何对 hexo 博客进行加密，加密效果正是现在所看到的。

## 如何使用
先安装 `hexo-blog-encrypt`
```bash
npm install hexo-blog-encrypt --save
```

最简洁的使用方式就是在文章信息头添加 `password` 字段
```
---
title: Hexo-博客文章进行加密
password: mojipanda
---
```

可以自定义一下引导语，本文配置如下：
```
---
title: Hexo-博客文章进行加密
password: mojipanda
abstract: 试一下文章加密，进来看效果～
message: 尝试输入一下密码查看全文：mojipanda
wrong_pass_message: 哦吼，密码不对，请再试试。
wrong_hash_message: 好像有点问题，但也能看看。
---
```

其他高级设置请参考：
{% note link, [https://github.com/MikeCoder/hexo-blog-encrypt](https://github.com/MikeCoder/hexo-blog-encrypt) %}

## 使用心得
作为一个静态博客，有这样的加密功能可以说很不错了，值得体验尝试，但不可以过于依赖，毕竟加密都是有风险的。
