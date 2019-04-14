---
layout: post
title: '隐藏Safari中input输入框内自带的小人图标'
subtitle: '项目中遇到的一个小问题'
date: 2017-09-11 17:40:00
author: 'marillo'
header-img: 'img/about-bg.jpg'
catalog: false
tags: 
    -Css
---

一开始是input-type="number"的输入框里面出现的上下箭头，需求需要隐藏。

加上了一段代码解决了如下：

~~~css
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none !important;
}
~~~

然后测试又发现在safari下input输入框右侧会出现一个小人，点击后会自动补充联系人的信息，需要隐藏。搜了一个解决方案如下：

~~~css
input::-webkit-contacts-auto-fill-button {
  visibility: hidden;
  display: none !important;
  pointer-events: none;
  position: absolute;
  right: 0;
}
~~~

