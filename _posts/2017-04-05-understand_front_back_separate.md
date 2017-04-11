---
layout: post
title: 正确理解前后端分离
date: 2016-04-05
tags: 工具
---

### 区分前后端

首先，我们必须意识到：

1. 浏览器有 MVC 业务抽象概念
2. 服务器也有 MVC 业务抽象概念

> MVC 是对于软件构成的抽象划分

- M主要负责数据与模型
- V主要负责显示
- C主要负责交互与业务

前后端的MVC 归纳如下：

| |前端|后端|
|-|-|-|
|M|json xml html数据 等|数据库 文件 等|
|-|-|-|
|V|模板引擎 模板片段等|HTML模板|
|-|-|-|
|C|JS 业务逻辑 HTTP请求交互（AJAX, JSONP, WEBSOCKET)|HTTP请求路由 搜索引擎 数据分析 文件服务|


来张图就更加清晰明了

![separate](/images/posts/front_back_separate/img1.jpg)

总结自 [如何简单区分Web前后端与MVC](https://github.com/calidion/calidion.github.io/issues/3)
