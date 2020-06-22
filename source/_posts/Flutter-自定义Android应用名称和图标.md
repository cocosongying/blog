---
title: Flutter-自定义Android应用名称和图标
meta:
  donate: true
categories: [编程学习]
tag: [Flutter]
abbrlink: f9ad
date: 2020-06-22 21:47:45
---

记录一下在 Flutter 项目中如何自定义 Android 应用名称和图标。

<!-- more -->

新建的 Flutter 项目默认的应用名称是创建的项目名称，图标也是 Flutter 默认的图标。我们往往希望应用名称是中文名称，图标也可以是自定义图标。

首先在项目中找到下面路径的文件
```
项目路径/android/app/src/main/AndroidManifest.xml
```

将图标拷贝到资源目录，这里有不同分辨率的文件夹
```
项目路径/android/app/src/main/res/mipmap-hdpi/logo.png
```

在 `application` 节点下
```
android:name="io.flutter.app.FlutterApplication"
android:label="磨叽熊猫"
android:icon="@mipmap/logo">
```

其中 `android:label` 对应的值就是应用名称，`android:icon` 对应的值表示自定义的图标资源路径。

设置好这两个值重新打包运行就可以了。
