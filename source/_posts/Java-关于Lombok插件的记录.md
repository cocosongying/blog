---
title: Java-关于Lombok插件的记录
meta:
  donate: true
categories: [编程学习]
tag: [Java]
references:
  - name: Lombok-官网
    url: 'https://projectlombok.org/'
  - name: Lombok使用介绍
    url: 'https://objectcomputing.com/resources/publications/sett/january-2010-reducing-boilerplate-code-with-project-lombok'
  - name: 为什么有的程序员不推荐使用Lombok！
    url: 'http://blog.itpub.net/69908877/viewspace-2676272/'
abbrlink: ad2d
date: 2020-06-10 20:23:08
---

遇到一个项目中使用了Lombok插件，这里记录一下如何使用以及自己的几点看法。

<!-- more -->

## 背景
最近遇到一个项目，打算把项目跑起来看一下效果，结果导入项目到IDE中就一片红叉，报错的地方几乎都是使用 get set 的地方，打开类文件确实没有 get set 方法，只有成员属性。于是搜索了一下什么情况下可以去掉 get set。最终揪出了 lombok 这个东西。

## 解决方案
以我使用 eclipse 举例，其他 IDE 在官网有介绍使用步骤。

1. 在下面的链接下载 `lombok.jar`
{% note link, [lombok.jar 下载](https://projectlombok.org/download) %}

2. 将下载好的 `lombok.jar` 复制到 eclipse.ini 所在文件目录，找一下 eclipse 安装目录，很容易找到。

3. 编辑 `eclipse.ini`，在最后添加以下代码并保存
```
-javaagent:lombok.jar
```

4. 重启 eclipse 后 clean 一下项目解决

## 关于使用
lombok 主要是用注解的方式达到精简代码的目的。首先使用 maven 导入依赖
```
<dependencies>
	<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>1.18.12</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```

之后就可以使用 `@Data` 注解，相当于同时使用了 `@ToString`、`@EqualsAndHashCode`、`@Getter`、`@Setter` 和 `@RequiredArgsConstrutor` 这些注解。
```java
import lombok.Data;

@Data
public class Person {
    private String name;

    private Integer sex;

    private String address;

}
```

## 几点看法
这种方式确实省了不少代码，但是有利有弊。这个必须要 IDE 支持，所有开发人员都要安装这个插件，毕竟不安装的都会报错。究竟用不用，使用规模，还是视情况而定吧，不要过度依赖。参考资料里关于lombok的使用看法还是比较中肯的，我也比较认同。
