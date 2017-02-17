---
layout: post
title: "浅谈JavaScript的函数与栈"
subtitle: ""
date: 2017-02-17 15:36:00
author: "mirillo"
header-img: "img/about-bg.jpg"
catalog: false
tags:
    - JavaScript
---

JavaScript中经常会用到setTimeout来推迟一个函数的执行，如：

```javascript
setTimeout(function() {
  alert("Hello World");
}, 1000);
```

会在执行到这句话后延迟1秒钟来弹出alert窗口。

那么在看这一段：

```javascript
function test() {
  setTimeout(function() {
    alert(1);
  }, 0);
  alert(2);
}
test();
```

注意这段代码中的setTimeout延迟设为了0，就是延迟0毫秒，貌似是不做任何延迟立刻执行，即1，2。

但实际的执行结果确实2，1。这得从JavaScript调用堆栈(call stack)和setTimeout的功能说起。

首先JavaScript是单线程的，即同一时间只执行一条代码，所以每一个JavaScript代码执行块会“阻塞”其他异步事件的执行。其次，和其他的编程语言一样，JavaScript中的函数调用也是通过堆栈实现的。在执行函数test的时候，test先入栈，如果不给alert(1)加setTimeout，那么alert(1)第2个入栈。最后是alert(1)。但现在给alert(1)加上setTimeout后，alert(1)就被加入到一个新的堆栈中等待，并“尽可能快”的执行。这个尽可能快就是指在a的堆栈完成后就立刻执行，因此实际的执行结果就是先alert(2)，再alert(1)。在这里setTimeout实际上就是让alert(1)脱离了当前函数的堆栈。



看下面一个例子：

```html
<input name="input" onkeydown="alert(this.value)" type="text" value="a" />  
```

这样一段函数意图是每输入一个字符就把当前input里的所有字符都alert出来，但实际效果却是alert出按键之前的内容。这里，我们就可以利用setTimeout(0)来实现。

```html
<input onkeydown="var me=this; setTimeout(function(){alert(me.value)}, 0)" name="input" type="text" value="a" /> 

```



这样当onkeydown事件触发的时候，alert就被放入了下一个调用堆栈，一旦onkdeydown事件触发的堆栈关闭后就开始执行。当然浏览器还有个onkeyup事件也可以实现我们的需求。

这样的setTimeout用法在实际项目中还是会时常遇到。比如浏览器会聪明的等到一个函数堆栈结束后才改变DOM,如果这个函数堆栈中把页面背景先从白色设为红色，再设为白色，那么浏览器会认为DOM没有发生任何改变而忽略这两句话，因此我们可以通过setTimeout把“设回白色”函数加入下一个堆栈，那么就可以确保背景颜色发生过改变了（虽然速度很快可能无法被察觉）。

总之，setTimeout增加了JavaScript函数调用的灵活性，为函数执行顺序的调度提供极大便利。