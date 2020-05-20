---
title: Nginx-记一次http转发https配置错误
meta:
  donate: true
categories: [编程学习]
tag: [nginx]
abbrlink: '8e63'
date: 2020-05-20 09:08:51
---

这是一次由于 nginx 配置错误导致的网页无法访问事故。

<!-- more -->

## 背景
在浏览器通过 `http` 访问网址会在地址栏左边提示 `不安全` 字样，而通过 `https` 则会显示一个带 🔒 的图标。我的网站刚建的时候就已经可以同时支持 `http` 和 `https` 了。

由于百度收录站点要填写好多个人隐私信息，不高兴填，于是就只提交了首页地址，也没抱什么希望，有次搜索出了我的网站，不过是通过 `http`方式访问的，于是我就想能不能做个跳转，把 `http` 请求全部转发到 `https`上。

## 搞事
我查到了下面的配置

```
rewrite ^(.*)$ https://$host$1 permanent;
```

然后我就直接复制到我的配置文件里：

```
server { 
    listen 80;
    listen 443 ssl http2;
    server_name mojipanda.com www.mojipanda.com;
    rewrite ^(.*)$ https://$host$1 permanent;
    ssl_certificate cert/mojipanda.com_chain.crt;
    ssl_certificate_key cert/mojipanda.com_key.key;
    ssl_session_timeout 4h;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    error_page 500 502 503 504 404 /404.html;
    location / { 
        alias /mojipanda/blog/;
        index index.html;
    }
}
```

执行一下 `nginx` 的热加载
```
nginx -s reload
```

从百度搜索 `磨叽熊猫` 然后点击我的主页地址 [http://mojipanda.com](http://mojipanda.com) ，果然跳转到 [https://mojipanda.com](https://mojipanda.com) 。

## 异常
这下可以休息一会了，于是我拿起手机，随手访问了一下我的网站，无法加载！！！

什么情况！微信试了，两个不同的手机浏览器也试了都不行，急忙打开电脑，电脑浏览器访问又是正常的。想想刚刚只改了那一个配置，赶紧先撤回修改查查原因。

恢复到之前版本之后，发现微信还是打开不了，不过手机浏览器已经可以访问了，这应该是微信缓存问题，先不管了，查查配置原因。

## 解决
原来 `http` 的 配置要和 `https` 的配置分开，在 `http` 的配置里做跳转到 `https` 就可以了。推荐使用 301 重定向方式，而不是 `rewrite` 网址。

```
return 301 https://mojipanda.com$request_uri;
```

修改后的配置文件如下：
```
server { 
    listen 80;
    server_name mojipanda.com www.mojipanda.com;
    return 301 https://mojipanda.com$request_uri;
}

server {
    listen 443 ssl http2;
    server_name mojipanda.com www.mojipanda.com;
    ssl_certificate cert/mojipanda.com_chain.crt;
    ssl_certificate_key cert/mojipanda.com_key.key;
    ssl_session_timeout 4h;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    error_page 500 502 503 504 404 /404.html;
    location / { 
        alias /mojipanda/blog/;
        index index.html;
    }
}
```

## 验证
首先电脑浏览器访问正常，`http` 请求也成功跳转到 `https` 了。

再试试手机浏览器访问也正常，`http` 请求也成功跳转到 `https` 了。

再打开微信，点一点以前的连接，发现还是不行，也没找到清缓存的地方，退出重启清后台，还是没有效果，最后只能等等看了。（当时应该找另外一个手机验证的）。等了一会，链接终于访问成功了，而分享的旧链接还不行，再等一会，终于恢复正常。

## 总结
遇到问题不用慌，对于不熟悉的地方，多尝试，多找找资料，还有多备份。发现问题，先恢复，在收集信息查找原因，定位问题找到解决办法，最后充分验证。遇到问题并没有啥，发现问题解决问题之后，感觉自己又积累了一份经验啦～
