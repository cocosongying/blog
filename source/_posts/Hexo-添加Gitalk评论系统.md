---
title: Hexo-添加Gitalk评论系统
meta:
  donate: true
categories: [实用教程]
tag: [Hexo]
abbrlink: da9d
date: 2020-05-19 21:17:23
references:
  - name: Gitalk-演示
    url: 'https://gitalk.github.io/'
  - name: Gitalk-Github
    url: 'https://github.com/gitalk/gitalk'
---

Gitalk - 一个基于 Github Issue 和 Preact 开发的评论插件

<!-- more -->

## 简介
静态博客其实有很多评论系统，每种评论系统都有自己各自的优势。我使用的这套主题已经可以支持下面这些评论系统了。

+ [Disqus](https://disqus.com/)
+ [Gitalk](https://github.com/gitalk/gitalk)
+ [Valine](https://valine.js.org)
+ [MiniValine](https://github.com/MiniValine/MiniValine/)
+ [Livere](https://www.livere.com/)
+ [Vssue](https://vssue.js.org/zh/)

## 我为什么选择Gitalk
`Disqus` 还是不错的，以前使用过，不过现在在国内访问不了。

`Valine` 第一次看到的时候还是很惊艳的，我也发现有不少博客都在使用，在使用方面，支持 `markdown` 语法，可以发一些内置表情包，不需要登录认证就可以进行评论。最后一点我个人比较介意，所以忍痛放弃。

`Gitalk` 是基于 `Github Issue` 的，评论必须使用 `Github` 账号登录。我这个博客目前技术内容偏多，所以受众也很明确。即使没有账号进行评论，还是可以通过微博，邮件或者公众号等方式进行联系。而且部署也比较简单，所以就选择它了，剩下几个也就没了解了，感兴趣的可以去看看。

## 如何使用
首先登录你的 `Github` 账号，直接访问下面的地址：
{% note link, [https://github.com/settings/applications/new](https://github.com/settings/applications/new) %}

填写 `*` 号内容并提交，注意这两个 URL 是填写你要使用评论系统的网址。

![](https://cdn.jsdelivr.net/gh/cocosongying/cdn-assets/blog/da9d/01.png)

提交完成你会获得 `Client ID` 和 `Client Secret`，这两个是用来填写在配置里的。

![](https://cdn.jsdelivr.net/gh/cocosongying/cdn-assets/blog/da9d/02.png)

我用的主题配置文件 `themes/volantis/_config.yml` 修改下面几项。

+ `clientID` 和 `clientSecret` 是上面获取的
+ `repo` 填写你的github建的仓库名称，因为要用到 `issues`
+ `owner`和 `admin` 写自己账号就行了，注意 `admin` 格式是数组
```
# Gitalk
# https://gitalk.github.io/
gitalk:
  clientID: yourClientID
  clientSecret: yourClientSecret
  repo: blog
  owner: cocosongying
  admin: [cocosongying]
```

配置完成执行 `hexo clean && hexo g` 来重新生成网站，`hexo s` 本地看一下效果。这个时候显示 `未找到相关的 Issues 进行评论` `请联系 @yourname 初始化创建`，点击 `使用 Github 登录` 会跳转到网站首页，没有用。

所以，这一步很重要，先执行 `hexo d` 把网站部署上去。

![](https://cdn.jsdelivr.net/gh/cocosongying/cdn-assets/blog/da9d/03.png)

在部署到线上的网站点击 `使用 Github 登录` 会跳转到下面的页面提示你授权。

![](https://cdn.jsdelivr.net/gh/cocosongying/cdn-assets/blog/da9d/04.png)

授权完成再看你的评论框就已经部署成功了。

![](https://cdn.jsdelivr.net/gh/cocosongying/cdn-assets/blog/da9d/05.png)

## 使用心得
部署起来还是挺简单的，而对于使用评论者而言，需要 `使用 Github 登录` 之后进行授权才可以评论。

我这个流量小的网站，评论可能会一直是 `0 条评论` 吧 😂
