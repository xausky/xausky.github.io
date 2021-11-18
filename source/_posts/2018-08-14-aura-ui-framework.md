---
title: Aura UI Framework 浅析
tags:
  - aura
  - ui
  - framework
categories:
  - work
toc: false
thumbnail: '/images/n6dfvysylorglglec5si.jpg'
date: 2018-08-14 13:44:54
---

> Aura是Salesforce构建的开源UI框架，用于为移动和桌面设备开发动态Web应用程序。

<!-- more -->

## 1.1 组件概览图

![auraconponentsumrary.png](https://i.loli.net/2019/08/13/hC9o3TYNvUOLerA.png)

## 2.1 前端组件

![aurafrontcomponent.png](https://i.loli.net/2019/08/13/gwrby17LWjxPIFQ.png)

* 前端主要功能组件有Component和Controller。
* Component是xml格式，类似html，可以包含标准html标签和其他Component。
* Controller是js格式，是一个Object里面包含供Component调用的方法。
* CSS和标准css一样可以定义component外形。
* DOC是组件的描述文档。
* 除此之外还有：Model, Renderer, Helper, Provider, TestCases等组件

## 2.2 前端组件的编译

![aurafrontconponentcomplie.png](https://i.loli.net/2019/08/13/TFWruqQx5gPsDcA.png)

* Arua后端服务包含对前端资源的编译，将各种组件编译为标准的html，css和js返回浏览器执行展示。
* 主要会生成三个js文件，其html包含于js文件中。
appcore.js app.js 由服务器端动态生成，inline.js 和 页面html为静态模板，负责加在js和启动其逻辑。
* 组件编译过程如下：

![aurafrontconponentcomplieflow.png](https://i.loli.net/2019/08/13/aovyYfA4jz9TMdg.png)


## 3.1 后端的执行逻辑

![aurabackrpc.png](https://i.loli.net/2019/08/13/jLxCRewFNSvd2ah.png)

* 后端代码主要为后端的Controller代码，提供前端组件调用实现后端逻辑。
前端组件会写后端Controller的完整包名，然后使用HTTP POST请求的方式传递包名和方法名以及参数，最后后端返回方法的返回值完成RPC调用。
* 使用@AuraEnabled注解表示可供前端进行RPC调用的方法，在服务启动时扫描通用Controller方法并缓存，在第一次RPC调用时缓存自定义Controller方法。
* 调用方法的参数和方法的返回值都序列化为JSON数据传输。

## 4 总结

Aura作为一个前后端统一的Web UI框架使用自定义的前端框架和RPC调用实现了一套完整的动态组件解决方法，有很多优势也存在一些问题。

优势：
* 屏蔽了前后端互交的复杂性，如同想调用一个前端方法一样来调用后端方法。
* 组件可重用，重用的不仅是组件的前端，后端也可以进行重用。
* 完善的测试和文档方案，可以写组件的测试代码和文档，提供测试和展示文档的界面和机制。

问题：
* 由于使用自定义RPC调用，对和其他系统对接造成困难，基本上都需要重新实现代码。
* 前端组件可以在运行时进行修改和新增，但无法在运行时新增后端Controller，需要其他方案解决。