---
layout: post
title: 'input输入框的光标定位的问题'
subtitle: '项目中遇到的一个小问题'
date: 2017-07-05 14:26:00
author: 'marillo'
header-img: 'img/about-bg.jpg'
catalog: false
tags: 
    - JavaScript
---

前两天的项目里面遇到一个问题，就是点击回复，先给输入框赋值，然后再触发focus事件，在浏览器上显示在后端，但是在手机就一直在最前端。

现在我想要使光标都显示在最后端。Google了一下找到了一个方法。

> 1. 调用focus事件。
> 2. Value赋值为空。
> 3. 给input再赋值一次即可。

~~~javascript
input.focus(); 
input.value = ''; 
input.value = val;
~~~

