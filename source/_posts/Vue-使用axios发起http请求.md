---
title: Vue-使用axios发起http请求
meta:
  donate: true
categories: [编程学习]
tag: [Vue]
references:
  - name: Axios-Github
    url: 'https://github.com/axios/axios'
  - name: Vue-Axios-Github
    url: 'https://github.com/imcvampire/vue-axios'
abbrlink: '9643'
date: 2020-05-22 10:24:25
---

服务端提供好接口，前端需要发起 http 请求调用服务端接口。本文介绍在 Vue 中如何使用 axios 发起 http 请求来调用服务端接口。

<!-- more -->

## 如何使用

先安装
```bash
npm install axios --save
npm install vue-axios --save
```

在 `main.js` 中引入 axios
```
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

在 `component` 文件中可以通过下面的方式发起 `GET` 请求，`POST` 请求将 `get` 改成 `post` 即可：
```
Vue.axios.get(api).then((response) => {
  console.log(response.data)
}).catch((error) => {
  console.log(error);
});

this.axios.get(api).then((response) => {
  console.log(response.data)
}).catch((error) => {
  console.log(error);
});

this.$http.get(api).then((response) => {
  console.log(response.data)
}).catch((error) => {
  console.log(error);
});
```

## 我的写法
首先在 `consts/api.js`里定义好接口名称地址常量
```
const Api = {
    BASE_URL: 'http://localhost:3000',
    USER_LIST: '/user/list',
}

export default Api
```

可以在 `main.js` 里配置好全局 `baseURL`
```
axios.defaults.baseURL = consts.Api.BASE_URL;
```

在 `common/user.js` 里封装好调用方法
```
import axios from 'axios'
import Consts from '../consts'

const User = {
    async list(params) {
        let res = await axios.post(Consts.Api.USER_LIST, params);
        return res.data;
    },
}

export default User
```

在 `component` 里先引入
```
import UserApi from "../../common/user";
```

直接调用就可以了
```
let res = await UserApi.list({});
```

## 使用心得
这里先记录一下 Vue 调用服务端接口的方式，关于 `axios` 的高阶使用还是要看看官方文档的。目前的写法也够基本使用了。
