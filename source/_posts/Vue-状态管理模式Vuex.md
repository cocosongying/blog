---
title: Vue-状态管理模式Vuex
meta:
  donate: true
categories:
  - 编程学习
tag:
  - Vue
references:
  - name: Vuex-官网
    url: 'https://vuex.vuejs.org/zh/'
abbrlink: '6604'
date: 2020-05-13 16:35:32
---

简单介绍一下在 Vue 项目中如何使用 Vuex 实现状态管理。比如我从服务端获取的个人信息，有很多模块都用到，这个时候相当于全局存储，方便各个模块调用。

<!-- more -->

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。

## 背景
我在学习使用 `Vue` 的过程中，遇到了一个存储问题，在登录的时候获取到个人信息，我需要在多个模块展示，容易想到的就是弄个全局变量，各个模块都方便调用。这个时候我查到了 `Vuex`，下面我们就来看看如何使用。

## 如何使用

先安装
```bash
npm install vuex --save
```

为了结合 `cookie` 使用我们还可以安装一下 `vue-cookies`
```bash
npm install vue-cookies --save
```

在 `src` 文件夹下创建文件夹 `store` 并新建文件 `index.js`

举个例子用，先上代码，内容如下：
```
import Vue from 'vue';
import Vuex from 'vuex';
import cookie from 'vue-cookies';

Vue.use(Vuex);
const store = new Vuex.Store({
    state: {
        userInfo: {},
        isLogin: cookie.get('isLogin')
    },
    getters: {
        userInfo: state => state.userInfo,
        isLogin: state => state.isLogin
    },
    mutations: {
        setUserInfo(state, data) {
            state.userInfo = data;
        },
        setLoginState(state, data) {
            state.isLogin = data;
            cookie.set('isLogin', data);
        }
    },
    actions: {
        setUserInfo({ commit }, data) {
            commit('setUserInfo', data);
        },
        setLoginState({ commit }, data) {
            commit('setLoginState', data);
        }
    }
});

export default store;
```
在 `main.js` 中将 `store` 引入进来
```
import Vue from 'vue'
import App from './App.vue'
import router from './router';
import store from './store';

Vue.config.productionTip = false

new Vue({
  store,
  router,
  render: h => h(App),
}).$mount('#app')
```

## 代码分析
在 `store/index.js` 文件中

`state` 表示要要管理的状态，以 key-value 键值对的形式存储。

`getters` 是提供我们获取状态的入口。可以使用下面的语句获取信息
```
let userInfo = this.$store.getters.userInfo;
```
`mutations` 是用来修改 `state` 管理的值。

`actions` 支持异步操作，官方简易通过这里提交修改信息，最终会调用 `mutations`。可以使用下面的语句修改信息，这里就是调用的 `actions`
```
this.$store.dispatch("setLoginState", true);
```

## 本地存储
使用上面的方法操作之后，我发现刷新一下页面就会导致之前存储的状态全部丢失。然后找到了下面的解决方案，就是在 `localStorage` 中存储 `store` 的值，创建的时候再获取。这样前端的状态信息就不会丢了。代码如下：
```
<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  created() {
    if (localStorage.getItem("store")) {
      this.$store.replaceState(
        Object.assign(
          {},
          this.$store.state,
          JSON.parse(localStorage.getItem("store"))
        )
      );
    }
    window.addEventListener("beforeunload", () => {
      localStorage.setItem("store", JSON.stringify(this.$store.state));
    });
  }
};
</script>
```
| 特性 | 数据生命周期 | 数据存放大小 | 与服务器通信 |
| -- | -- | -- | -- |
| cookie | 一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效 | 4K左右 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据，会带来性能问题 |
| sessionStorage | 仅在当前会话下有效，关闭页面或浏览器后被清除 | 一般5M | 仅在浏览器中保存，不参与和服务器的通信 |
| localStorage | 除非被清除，否则永久保存 | 同上 | 同上 |

## 使用心得
当 `store/index.js` 管理的状态越来越多时，这个文件肯定会很长，不方便维护。可以将 `getters` `mutations` `actions` 单独拆分出来。
