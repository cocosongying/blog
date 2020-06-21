---
title: Flutter-第一个项目挖坑指南
meta:
  donate: true
categories: [编程学习]
tag: [Flutter]
abbrlink: 2c56
date: 2020-06-21 18:28:58
references:
  - name: Flutter SDK releases Download
    url: 'https://flutter.dev/docs/development/tools/sdk/releases?tab=macos#macos'
---

许久没学习 Android 开发，这次打算学习用 Flutter 创建应用，光是准备环境就挖了好多坑。

<!-- more -->

## Flutter 安装

从下面的地址选择最新的稳定版进行下载：

[Flutter SDK releases Download](https://flutter.dev/docs/development/tools/sdk/releases?tab=macos#macos)

解压缩之后放在自己指定的文件夹下

编辑 `.bash_profile` 文件
```bash
cd ~
open -e .bash_profile
```

我本机的内容如下：

```
export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export ANDROID_HOME=/Users/songying/android-sdk-macosx
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=/Users/songying/flutter/bin:$PATH
```

运行下面的命令让环境变量生效

```bash
source .bash_profile
```

之后运行下面的命令检查 flutter 有没有生效
```bash
flutter doctor
```

## Android Studio
这个之前已经安装好了，重新打开只要更新升级就可以了。关于 Android SDK 也是之前安装好的，这里只做了常规升级。

然后顺理成章使用 Android Studio 新建一个 Flutter 项目，结果失败了，报错内容大致如下：

```
pub.flutter-io.cn%0D is not a valid link-local address but contains %. Scope id should be used as part of link-local address. (at character 18)
pub.flutter-io.cn%0D                                                    
                 ^                                                      
package:pub/src/source/hosted.dart 437:7           BoundHostedSource._throwFriendlyError
package:pub/src/source/hosted.dart 188:7           BoundHostedSource._fetchVersions
===== asynchronous gap ===========================                      
dart:async                                         _CustomZone.runUnary 
package:pub/src/rate_limited_scheduler.dart 82:30  RateLimitedScheduler._processNextTask.runJob
package:pub/src/rate_limited_scheduler.dart 85:30  RateLimitedScheduler._processNextTask
dart:async                                         new Future.sync      
package:pool/pool.dart 126:18                      Pool.withResource.<fn>
This is an unexpected error. Please run                                 
                                                                        
    pub --trace '--verbosity=warning' get --no-precompile               
                                                                        
and include the logs in an issue on https://github.com/dart-lang/pub/issues/new
```

去他们的 issues 下面看到有不少人也是这个问题，而且还没解决。于是我又尝试安装了旧版本的 Flutter，依旧没有解决问题。关于 `pub.flutter-io.cn%0D` 这个是环境变量里配置的东西，配置的时候并没有后面的 `%0D` 尝试修改过也没有解决问题，期间尝试各种重启都不行。

## 折衷解决
本来打算放弃折腾，等之后更新版本了再搞，后来看到可以使用命令行创建，于是打开系统自带终端输入命令进行创建

```bash
flutter create 项目名
```

```bash
flutter clean
flutter run
```

在 `flutter run` 这个步骤又遇到错误
```
Running Gradle task 'assembleDebug'...                                  
                                                                
FAILURE: Build failed with an exception.                                                                                                                          * What went wrong:                                                               Could not determine the dependencies of task ':app:compileDebugJavaWithJavac'.   > Could not resolve all task dependencies for configuration ':app:debugCompileClasspath'.                          
   > Could not resolve io.flutter:arm64_v8a_debug:1.0.0-ee76268252c22f5c11e82a7b87423ca3982e51a7.                  
     Required by:                                                                         project :app                                                                  > Skipped due to earlier error                                                   > Skipped due to earlier error                                                   > Skipped due to earlier error                                                > Could not resolve io.flutter:x86_debug:1.0.0-ee76268252c22f5c11e82a7b87423ca3982e51a7.                        
     Required by:                                                                         project :app                                                                  > Skipped due to earlier error                                                   > Skipped due to earlier error                                                   > Skipped due to earlier error                                                > Could not resolve io.flutter:x86_64_debug:1.0.0-ee76268252c22f5c11e82a7b87423ca3982e51a7.                     
     Required by:                                                                         project :app                                                                  > Skipped due to earlier error                                                   > Skipped due to earlier error                                                   > Skipped due to earlier error                                                                                                                              * Try:                                                                           Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.
                                                                                 * Get more help at https://help.gradle.org                                                                                                                        BUILD FAILED in 2m 9s
```

网上查了一下，说是 gradle 配置问题，将下面几个文件中
+ 项目路径/android/build.gradle
+ flutter路径/packages/flutter_tools/gradle/aar_init_script.gradle
+ flutter路径/packages/flutter_tools/gradle/flutter.gradle
+ flutter路径/packages/flutter_tools/gradle/resolve_dependencies.gradle

```
google()
jcenter()
```

修改为

```
// google()
// jcenter()
maven { url 'https://maven.aliyun.com/repository/google' }
maven { url 'https://maven.aliyun.com/repository/jcenter' }
maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
```

```
https://storage.googleapis.com/download.flutter.io
```

替换为

```
http://download.flutter.io
```

在命令行运行
```bash
flutter run
```

手机上终于显示出项目内容了

## 写在最后
本文记录当时遇到问题的状态和尝试的解决办法，也许之后没有这些问题了，也许会有其他问题，方法很重要，算是积累挖坑经验吧。
