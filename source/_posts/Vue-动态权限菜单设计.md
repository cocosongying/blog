---
title: Vue-动态权限菜单设计
meta:
  donate: true
categories: [编程学习]
tag: [Vue]
abbrlink: '8390'
date: 2020-05-23 17:02:39
---

在设计开发后台管理系统时，少不了会遇到不同用户角色拥有不同菜单访问权限的需求，本文介绍 Vue 的动态路由实现的动态权限菜单。

<!-- more -->

## 主要实现

在 `router/index.js` 先写下所有角色都共通的模块
```js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  mode: 'history',
  base: '/dashboard',
  routes: [
    {
      path: '/login',
      meta: {
        normal: true
      },
      component: () => import('@/components/Login.vue')
    }
  ]
})
```

这里的自定义代码是控制是否需要登录验证的，加上这个就不需要登录验证，目的是提供一些单页面的不需要登录的访问，在 `main.js` 里会用到判断。
```js
meta: {
  normal: true
},
```

在 `router/components.js` 里定义出所有的动态路由
```js
const Home = () => import('@/components/Home.vue');
const Blank = () => import('@/components/menu/Blank.vue');
const Profile = () => import('@/components/menu/Profile.vue');
const UserList = () => import('@/components/menu/UserList.vue');

export default {
    Home,
    Blank,
    Profile,
    UserList,
}
```

`main.js` 完整代码如下：
```js
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import App from './App.vue'
import router from './router';
import store from './store';
import consts from './consts'

import routemap from './router/components'
const genRoutes = function (menus, routes) {
  if (!routes) {
    routes = {
      path: '/',
      component: routemap['Home'],
      children: [
        {
          path: "",
          component: routemap['Blank']
        },
      ],
    }
  }
  if (menus.length > 0) {
    menus.forEach(menu => {
      if (menu.component) {
        menu.component = routemap[menu.component];
        routes.children.push({
          path: menu.path,
          component: menu.component,
        });
      }
      if (menu.children && menu.children.length > 0) {
        genRoutes(menu.children, routes);
      }
    });
  }
  return routes;
}

let fromApi = true;

axios.defaults.baseURL = consts.Api.BASE_URL;
Vue.use(VueAxios, axios)
Vue.prototype.Global = consts
Vue.config.productionTip = false

router.beforeEach((to, from, next) => {
  if (to.matched.some(data => data.meta.normal)) {
    next();
  } else if (store.getters.isLogin && fromApi) {
    fromApi = false;
    let menus = [
      {
        path: 'profile',
        component: 'Profile',
      }
    ];
    if (localStorage.getItem("store")) {
      try {
        let storeInfo = JSON.parse(localStorage.getItem("store"));
        let menu = JSON.parse(storeInfo.userInfo.menu);
        for (let i = 0; i < menu.length && i < consts.Menu.ALL.length; i++) {
          if (menu[i] === 1) {
            let info = {
              path: consts.Menu.ALL[i].url,
              component: consts.Menu.ALL[i].component
            }
            menus.push(info);
          }
        }
      } catch (error) {
        //
      }
    }
    const routes = genRoutes(menus);
    const notfound = {
      path: "*",
      redirect: '/'
    }
    router.addRoutes([routes, notfound]);
    router.push({
      path: to.path
    });
  } else if (!store.getters.isLogin) {
    next({
      path: '/login'
    });
  } else {
    next();
  }
});

new Vue({
  store,
  router,
  render: h => h(App),
}).$mount('#app')
```

这里是在路由访问前的处理，可以看到 `data.meta.normal` 控制了是否需要登录。下面注释了一段获取菜单的部分。这里有三种思路：
+ 一是不同角色对应的菜单定义在前端，这里根据角色获取
+ 二是这些动态权限定义在服务端，通过接口访问获取
+ 三是前端定义出所有权限数组，服务端返回角色权限下标，结合获取

```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(data => data.meta.normal)) {
    next();
  } else if (store.getters.isLogin && fromApi) {
    fromApi = false;
    let menus = [
      {
        path: 'profile',
        component: 'Profile',
      }
    ];
    
    // 获取菜单

    const routes = genRoutes(menus);
    const notfound = {
      path: "*",
      redirect: '/'
    }
    router.addRoutes([routes, notfound]);
    router.push({
      path: to.path
    });
  } else if (!store.getters.isLogin) {
    next({
      path: '/login'
    });
  } else {
    next();
  }
});
```

服务端设计的菜单结构，比如前端定义好权限数组如下：
```js
const Menu = {
    ALL: [
        {
            id: 1,
            name: '用户管理',
            url: 'userlist',
            component: 'UserList',
        },
        {
            id: 2,
            name: '用户管理2',
            url: 'userlist2',
        },
        {
            id: 3,
            name: '用户管理3',
            url: '#',
            child: [
                {
                    id: 301,
                    name: '用户管理301',
                    url: 'userlist301',
                },
                {
                    id: 302,
                    name: '用户管理302',
                    url: 'userlist302',
                },
            ]
        }
    ]
}

export default Menu
```

服务端返回的结构可以为 `[1,0,1]` 或是 `[1,0,[1,0]]` 这样的权限表示。`1` 表示有权限，`0` 表示没有权限。当然也可以直接返回数组下标或者是菜单id等方式都可以。如 `[0,2]` `[0,[0]]` 不建议权限嵌套太深，即使设计者能找到对应菜单，使用者找起来还是比较困难的。

下面是根据菜单获取路由的实现方法，把一些通用的部分定义在开头，后面根据菜单动态拼接。需要注意 404 页面要放在所有路由之后，见上面 `notfound` 的部分。

```js
const genRoutes = function (menus, routes) {
  if (!routes) {
    routes = {
      path: '/',
      component: routemap['Home'],
      children: [
        {
          path: "",
          component: routemap['Blank']
        },
      ],
    }
  }
  if (menus.length > 0) {
    menus.forEach(menu => {
      if (menu.component) {
        menu.component = routemap[menu.component];
        routes.children.push({
          path: menu.path,
          component: menu.component,
        });
      }
      if (menu.children && menu.children.length > 0) {
        genRoutes(menu.children, routes);
      }
    });
  }
  return routes;
}
```

## 写在最后

仅仅通过前端控制菜单权限肯定是不够的，服务端接口也需要设置角色权限，控制某些接口只允许某些角色访问。比如只有管理员可以添加用户，这样即使其他角色通过某些方法获取到了菜单权限，因为不具备添加用户接口访问权限，依然无法使用。
