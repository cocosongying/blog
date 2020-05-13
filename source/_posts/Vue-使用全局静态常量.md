---
title: Vue-使用全局静态常量
meta:
  donate: true
categories:
  - 编程学习
tag:
  - Vue
abbrlink: aa27
date: 2020-05-13 18:17:37
---

有很多共通的东西，需要我们使用全局静态常量。在 Vue 中是如何实现的呢？一起来看看吧～

<!-- more -->

## 背景
服务端定义了接口给前端调用，各个模块可能会调用到相同的接口，即使是不同的接口，如果服务端修改了接口定义，前端就要跟着改，这个时候，如果这些定义在一个指定文件，那么我们只要修改这一个文件就好了，其他模块都可以调用到。

## 如何使用
首先在 `src` 目录新建文件夹 `consts` 并创建 `index.js`
```
const Api =  {
    BASE_URL: 'https://mojipanda.com',
    DEMO_SHOW: '/demo/show',
}

export default {
    Api
}
```

在 `main.js` 中引入
```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import consts from './consts'

Vue.config.productionTip = false
Vue.prototype.Global = consts

new Vue({
  store,
  router,
  render: h => h(App),
}).$mount('#app')
```

主要是添加下面两行，这里 `Global` 的名字可以修改成其他命名，当然 `consts` 文件夹命名也可以修改
```
import consts from './consts'
Vue.prototype.Global = consts
```

准备工作已经完成，接下来只要通过下面的语句就可以获取到我们在 `consts/index.js` 中定义的常量了。
```
this.Global.Api.BASE_URL
```

## 使用心得
总体感觉还是比较简单的，对于 `consts/index.js`，当常量越来越多，也是不方便管理的，我们可以进行拆分。对例子中的 `Api` 来说，我们可以在 `consts` 文件夹新建一个 `api.js`，在这里定义常量
```
const Api = {
    BASE_URL: 'https://mojipanda.com',
    DEMO_SHOW: '/demo/show',
}

export default Api
```

修改 `consts/index.js`
```
import Api from '@/consts/api'

export default {
    Api
}
```

这样拆分就完成了，当我们需要新增其他类型的常量，只要再新建一个文件并在 `consts/index.js` 引入就可以正常使用了。
