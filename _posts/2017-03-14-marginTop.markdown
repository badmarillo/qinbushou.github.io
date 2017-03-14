---
layout: post
title: "margin-top失效常见症状及解决方法"
subtitle: ""
date: 2017-03-14 17:23:00
author: "marillo"
header-img: "img/about-bg.jpg"
catalog: true
tags:
    -DIV+CSS
---

#### 原因一：

外边距合并margin-top属性失效。代码如下

```html
<style type="text/css">
.first{
  width:100px;
  height:100px;
  background-color:red;
  margin-bottom:60px;
}
.second{
  width:100px;
  height:100px;
  background-color:green;
  margin-top:40px;
}
</style>
</head>
<body>
<div class="first"></div>
<div class="second"></div>
```

> 以上代码的运行可以看出，第二个div设置的margin-top并没有生效，起作用的是第一个div的设置的margin-bottom，这里有个规律，那就是**合并后的外边距的高度等于外边距的高度中较大的一个**，所以遇到这种情况可以格外注意外边距大小的设置



#### 原因二：

子元素和父元素也可能会导致设置的子元素上外边距失效情况，代码如下：

```html
<style type="text/css">
.father{
  width:300px;
  height:300px;
  background-color:red;
  margin-top:20px;
}
.children{
  width:100px;
  height:100px;
  background-color:blue;
  margin-top:10px;
}
</style>
</head>
<body>
<div class="father">
  <div class="children"></div>
</div>
```

> 在两个嵌套的div，如果外层div的父容器padding值为0，那么内层div的margin-top或者margin-bottom的值会“转移”给外层div，也就是父容器的父容器。

解决办法：

​	1：设置父容器的的样式加上：overflow:hidden。
​	2：把对父容器的margin-top外边距改成padding-top内边距。
​	3：给父容器div加样式， padding-top: 1px。
​	4：给父容器div加样式，position: absolute。
​	5：把父元素变成一个 block formating context ，下面是可选的方法
​		a、float: left/right
​		b、position: absolute
​		c、display: inline-block/table-cell
​		d、overflow: hidden/auto	