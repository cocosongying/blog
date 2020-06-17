---
title: Spring典型注解简介
meta:
  donate: true
categories: [编程学习]
tag: [Spring]
abbrlink: ed68
date: 2020-06-17 21:10:42
---

本文介绍 Spring 典型注解 @Controller @Component @Service @Repository 的异同。

<!-- more -->

## 相同点
@Controller @Component @Service @Repository 这四个注解都可以说成是 Component 级别的注解。被这些注解修饰的类都会被 Spring 扫描到并注入到 Spring 容器中进行管理。

## 异同点
| 注解 | 含义 |
| -- | -- |
| @Controller | 作用于表现层 (spring-mvc) |
| @Service | 作用于业务逻辑层 |
| @Repository | 作用于数据库访问持久层 |
| @Component | 基础注解，可以被注入到spring容器进行管理 |

### @Controller
标记这个类是一个 SpringMVC Controller 对象，用于暴露给前端的入口，对前端请求进行处理，转发，重定向等。通常在这里调用 Service 层的方法。

### @Service
标记这个类属于业务逻辑层。通常在这里调用数据库操作。

### @Reposotory
标记这个类是用来直接访问数据库的持久层。这个类作为DAO对象（数据访问对象，Data Access Objects）可以直接对数据库进行操作。

### @Component
通用注解，用来表示一个平常的普通组件，当一个类不合适用以上的注解定义时用这个组件修饰。

## 总结
用这些注解对应用进行分层之后，就可以将请求处理，业务逻辑处理，数据库操作处理分离出来，为代码解耦，方便以后项目的维护和开发。
