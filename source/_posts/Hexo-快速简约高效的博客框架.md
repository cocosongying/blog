---
title: Hexo-快速简约高效的博客框架
meta:
  donate: true
categories: [编程学习]
tag: [工具]
references:
  - name: Hexo-官网
    url: 'https://hexo.io/'
  - name: Hexo-Github
    url: 'https://github.com/hexojs/hexo'
abbrlink: 1f8f
date: 2020-05-07 09:39:46
---


Hexo - 一个快速简约且高效的静态博客框架

<!-- more -->

## 简介
Hexo 是一个静态博客框架，将 Markdown 文件渲染转换成 html，支持很多功能强大的插件，支持很多各种有趣的主题，总有一款适合你。

## 简易教程
安装 `hexo-cli` 工具
```bash
npm install hexo-cli -g
```

初始化项目 `blog`
```bash
hexo init blog
```

启动项目，通过 `http://localhost:4000` 访问预览效果
```bash
cd blog
npm install
hexo server
```

写一篇新博客，推荐使用下面的命令行创建，也可以在 `source/_posts/` 文件夹内自行创建。命令行创建的文件会根据 `scaffolds` 文件夹里的模板创建。这里的模板可以自行修改，添加一些通用配置。
```bash
hexo new post "这是文章名称"
```

如果写的博客还不想立即发布，推荐使用下面的命令行创建草稿。也可以在 `source/_drafts/` 文件夹内自行创建。
```bash
hexo new draft "这是文章名称"
```
草稿写完可以执行命令来发布，当然也可以直接移动到 `source/_posts/` 文件夹中。
```bash
hexo publish post "这是文章名称"
```

写一个新的页面，例如关于页面，可以通过下面的命令行创建。
```bash
hexo new page "about"
```

写好的文章需要执行下面的命令来将 Markdown 文件生成 html 文件。第一条用来清空之前生成好的，如果只是文章修改可以直接执行第二条。如果主题有修改，推荐两条语句都执行一下。
`hexo g` 是 `hexo generate` 的缩写，效果一样。
```
hexo clean
hexo g
```

最后执行启动命令就可以预览了。
`hexo s` 是 `hexo server` 的缩写，效果一样。
```bash
hexo s
```

在 `_config.yml` 文件配置 `deploy` 信息之后，可以通过下面的命令行直接将生成好的博客网站部署到 `git` 上。
`hexo d` 是 `hexo deploy` 的缩写，效果一样。
```bash
hexo d
```
需要安装下面这个插件才能使用 git 进行自动部署
```
npm install hexo-deployer-git --save
```

## 博客主题
Hexo 有很多有趣的主题，每个人审美不同，需要自己去选择自己喜欢的主题，如果有开发能力，也可以自己写一套主题。先来看一波官方推荐的主题吧，还可以在 [Github](https://github.com/) 上通过 `hexo` 关键词进行搜索，发现更多主题。
{% note link, [Hexo 官方推荐主题](https://hexo.io/themes/) %}

将主题拷贝到 `themes` 文件夹下，在 `_config.yml` 配置 `theme` 项来指定主题。

Hexo 还提供了很多插件来使博客更加丰富有趣。使用插件需要先安装再配置最后生成并预览。
{% note link, [Hexo 插件](https://hexo.io/plugins/) %}

## 高级教程
{% note blue, 更多功能及使用请参考官方文档，官方地址见下方参考资料。不同主题也有不同的配置，需要学习一下该主题作者制定的规则。 %}

## 使用心得
我对前端样式之类的不怎么熟悉，想要搞一套好的博客出来只能膜拜前辈们的心血。当文章达到一定数量的时候，Hexo 可能在执行转换命令时会比较慢，但这个不怎么影响使用。而且不需要复杂的部署就可以呈现如此优美的博客站点，并且适配多端环境，本人可以说非常喜欢了。

在选择主题方面，我看了很多主题，官方推荐的主题也全部都预览过，还搜索过其他主题，其中有很多有趣的样式，可毕竟只能选择一个，最终选择了现在使用的样式 [Volantis](https://volantis.js.org/) 。选主题这种事，随缘，看对眼了就行。
