---
title: Vue-集成AdminLTE
meta:
  donate: true
categories: [编程学习]
tag: [Vue]
references:
  - name: AdminLTE-官网
    url: 'https://adminlte.io/'
  - name: AdminLTE-Github
    url: 'https://github.com/ColorlibHQ/AdminLTE'
  - name: Vue-CLI
    url: 'https://cli.vuejs.org/zh/'
  - name: Vue.js-官网
    url: 'https://cn.vuejs.org/'
abbrlink: 3c0d
date: 2020-05-12 12:03:52
---

本文介绍使用 `vue-cli3` 集成 `AdminLTE-2.4.18`

<!-- more -->

> AdminLTE 是基于 Bootstrap 的开源响应式前端模板。

我个人比较喜欢这套模板的原因主要有：
+ 美观，众多模板中一眼就看中它了
+ 开源免费
+ 响应式支持很好

我不是前端开发人员，搞样式这种繁琐的事情还是交给现成的吧。虽然 AdminLTE3 也发布了，但是看起来个人感觉没有 AdminLTE2好，响应式处理也有点问题。感兴趣可以去官网看看不同版本的效果。

## 使用vue-cli3创建项目

```bash
npm install -g @vue/cli
vue create my-project
```

初始化项目步骤就不介绍了，按照vue-cli一步步操作就完成了。

## 引入AdminLTE

接下来我们先安装 `admin-lte` 依赖包
```bash
npm i admin-lte@2.4.18 --save
```

修改 `public/index.html` 给 body 加上样式
```
<body class="hold-transition skin-blue sidebar-mini">
```

修改 `src/main.js`
{% codeblock src/main.js %}
import Vue from 'vue'
import App from './App.vue'

import 'admin-lte/bower_components/bootstrap/dist/css/bootstrap.min.css'
import 'admin-lte/bower_components/font-awesome/css/font-awesome.min.css'
import 'admin-lte/bower_components/Ionicons/css/ionicons.min.css'
import 'admin-lte/dist/css/skins/_all-skins.min.css'
import 'admin-lte/dist/css/AdminLTE.min.css'
import 'admin-lte/bower_components/bootstrap/dist/js/bootstrap.min.js'
import 'admin-lte/dist/js/adminlte.min.js'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
{% endcodeblock %}

修改 `src/App.vue`
{% codeblock src/App.vue %}
<template>
  <div id="app">
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
{% endcodeblock %}

引入部分到这里就完成了，新页面需要新建模板文件，对照源码复制就好了。
空白模板可以查看 admin-lte 源码包里的 `starter.html`，把 body 里面的 `<div class="wrapper">...</div>` 部分拷贝到新建的页面 `<template></template>` 中。

## 启动预览

```bash
npm run serve
```
这个时候网页还有错误，Bootstrap 需要引入 JQuery，新建一个文件 `vue.config.js` 配置一下就行了。

{% codeblock vue.config.js %}
const webpack = require('webpack');

module.exports = {
    configureWebpack: {
        plugins: [
            new webpack.ProvidePlugin({
                $: 'jquery',
                jQuery: 'jquery',
                'window.jQuery': 'jquery'
            })
        ]
    }
}
{% endcodeblock %}

## 使用心得
由于要对照预览样式来查看自己想要的组件，我先下载了一份源码，在浏览器中打开 `index.html`，这个时候可以看到官方提供的所有的组件样式，然后浏览器右键显示网页源代码，就可以找到需要的组件的代码是怎么写的了。

为了后续使用方便，因为有不少组件都是通用的，所以我将各部分组件拆开了，
```
<template>
  <div class="wrapper">
    <Header />
    <MainSidebar />
    <ContentWrapper />
    <Footer />
    <ControlSidebar />
  </div>
</template>
```
由于 ContentWrapper 部分是每个页面独有的，可以做成路由，这样新写页面时只要写这部分就行了，头部底部，菜单导航之类的都不用修改。
```
<template>
  <!-- Content Wrapper. Contains page content -->
  <div class="content-wrapper">
    <router-view />
  </div>
  <!-- /.content-wrapper -->
</template>
```
