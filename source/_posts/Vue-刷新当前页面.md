---
title: Vue-刷新当前页面
meta:
  donate: true
categories: [编程学习]
tag: [Vue]
abbrlink: '1506'
date: 2020-05-23 10:50:59
---

前端页面在用户执行某个动作之后，可能更新了些数据或者状态，此时就需要重新刷新页面来渲染出最新结果。

<!-- more -->

## 推荐写法
这种方式用于执行某个动作之后刷新当前页面，并且页面不会有一闪的不好体验。

首先，在文件 `App.vue` 写上如下代码。通过控制 `router-view` 的显示或隐藏，达到控制页面再次加载的目的。
```
<template>
  <div id="app">
    <router-view v-if="isRouterAlive" />
  </div>
</template>

<script>
export default {
  name: "App",
  provide() {
    return {
      reload: this.reload
    };
  },
  data() {
    return {
      isRouterAlive: true
    };
  },
  methods: {
    reload() {
      this.isRouterAlive = false;
      this.$nextTick(function() {
        this.isRouterAlive = true;
      });
    }
  }
};
</script>
```

在需要刷新的页面，先注入 `reload` 依赖

```
<script>
export default {
  inject: ["reload"],
}
</script>
```

在执行操作的动作里直接调用 `this.reload();` 即可刷新当前页面。

## 其他写法
下面是刷新页面的其他写法，强制刷新页面，会有短暂的闪烁。存储在 `store` 的数据会丢失，需要注意处理。

```
location.reload();
```

```
this.$router.go(0);
```

## 使用心得
我在写登录跳转，并且需要刷新跳转页的时候，使用上面的方法并没有达到我想要的效果，不是没有跳转过去就是跳转之后页面没有刷新。然后找到了下面的写法解决了我的问题。

```
window.open("/dashboard", "_self");
```

登录之后数据会存储到 `localStorage` 中，这个跳转，相当于关闭了登录页直接在本页打开新跳转页。在 `App.vue` 的 `created` 方法里会从 `localStorage` 重新加载数据。

```
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
  },
};
</script>
```
