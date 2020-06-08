---
title: Java-Mac系统配置JDK环境变量
meta:
  donate: true
categories: [编程学习]
tag: [Java]
abbrlink: 47db
date: 2020-06-08 22:06:34
---

记录一下在 Mac 系统配置 JDK 环境变量。

<!-- more -->

1. 首先从官网中下载需要的版本安装包，并按照提示进行安装。

{% note link, [Java SE Downloads](https://www.oracle.com/java/technologies/javase-downloads.html) %}

2. 运行下面的命令查看 Java 安装路径。
```bash
/usr/libexec/java_home
```

比如结果为：
```
/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
```

3. 如果是第一次配置环境变量，可以创建一个 `.bash_profile` 文件，如果之前已经创建过则直接到下面一步。
```bash
cd ~
touch .bash_profile
```

4. 执行下面的命令会打开编辑窗口
```bash
open -e .bash_profile
```

5. 粘贴以下配置到窗口中，其中 JAVA_HOME 的配置路径从步骤2获得，不同版本号路径不一样。
```
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home

PATH=$JAVA_HOME/bin:$PATH:.

CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.

export JAVA_HOME

export PATH

export CLASSPATH
```

6. 执行下面的命令使配置生效。
```bash
source .bash_profile
```

7. 输入下面的命令检查一下配置的路径是否正确。
```bash
echo $JAVA_HOME
```

8. 最后通过查看 java 版本来检验配置是否生效。
```bash
java -version
```

运行结果应该会给出类似下面的信息：
```
java version "1.8.0_40"
Java(TM) SE Runtime Environment (build 1.8.0_40-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.40-b25, mixed mode)
```
