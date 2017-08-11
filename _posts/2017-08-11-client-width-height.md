---
layout: post
title: '用JS准确获取手机屏幕的宽度和高度'
subtitle: ''
date: 2017-08-11 15:47:00
author: 'marillo'
header-img: 'img/about-bg.jpg'
catalog: true
tags:
    -JavaScript
---



这次做的项目有一个需求是：

背景是一个渐变的边框，网页内容较少不能超过手机屏幕的时候，这个北京得自适应占满整个屏幕。

我Google了一下如何准确的获取，在知乎上找到了答案。

> 手机宽：document.documentElement.clientWidth;
>
> 手机高：document.documentElement.clientHeight;

**其中`documentElement`是文档根元素，就是`<html>`标签。**

这个得到的是设备像素可见宽高，比如IPHONE4S在微信内设置了viewport为1的时候为320\*416（手机480，微信状态栏64），IPHNE5里为320\*504。

其他更多参考 [关于client-height-width的博客](http://harttle.com/2016/04/24/client-height-width.html)。

