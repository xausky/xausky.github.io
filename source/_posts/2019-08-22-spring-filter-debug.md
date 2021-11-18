---
title: Spring Filter 相关问题排查方式
tags:
  - spring
  - filter
  - security
  - debug
categories:
  - work
toc: false
thumbnail: '/images/ixvm5c1jlnbabmshcwbd.jpg'
date: 2019-08-22 15:57:26
---

> 系统上出现单元测试时候无法正常实现过滤器内的逻辑，但是正常运行时候又问题，在Google后没有解决方法，看来只能从源码调试了。

<!-- more -->

# 一. 查看所有 Filter Chain

断点下到 org.springframework.security.web.FilterChainProxy#FilterChainProxy 就可以在启动后看到所有的 Filter Chain 。

![Debug](/images/tolbq9smoe4d3hyug2fr.png)

对比正常设想或者环境的不同可以找到问题所在，这里对比正常环境是多了一条 Filter Chian 也就是第一条，那么就可以找到入手点再调试多出来的 Filter Chian 是如何产生的，有没有禁用办法。

# 二. 定位不一致的 Filter Chain 创建方式

上一步调试发现多出来的是 DefaultSecurityFilterChain ，继续下断点到其构造函数，重启查看调用链。

![Filter Debug](/images/fvrgkytdkd0zlcqoe8wg.png)

找到构造函数的调用链观察到红色箭头部分是 WebSecurityConfiguration 也就是说这个链是由 SecurityConfigurer 配置来的，跳到对应栈，找到多余的 SecurityConfigurer 为测试类中多加的一个 Security 配置，在分析功能后成功解决问题。