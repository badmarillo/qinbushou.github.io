---
layout: post
title: "柱状图－快速排序"
subtitle: "使用html和css绘制柱状图，并在柱状图中表现出快排的整个过程"
date: 2017-02-10 17:35:00
author: "mirillo"
header-img: "img/about-bg.jpg"
catalog: true
tags:
    - 前端面试考察
---



![quickSort](/Users/yyl/Desktop/qinbushou.github.io/img/in-post/quickSort/quickSort.gif)

#### html 部分

```html
<div class="container">	
  <div class="cylinder h110"></div>
  <div class="cylinder h90"></div>
  <div class="cylinder h130"></div>
  <div class="cylinder h70"></div>
  <div class="cylinder h50"></div>
  <div class="cylinder h30"></div>
</div>
<div class="controller">
  <div class="rebuild">Rebuild</div>
  <div class="sort">Sort</div>
</div>
```



#### CSS部分 （使用flex)

```scss
$color: #ddd;
.container {
  margin:30px 0;
  display: flex;
  justify-content:space-between;
  align-items: flex-end;
}
.cylinder {
  display: flex;
  //flex: 0 0 10%;
  position:relative;
  width: 40px;
  height: 90px;
  background-color:$color;
  &::before, &::after {
    content: '';
    position: absolute;
    //background-color:red;
    background-color:darken($color, 10%);
    width: 100%;
    height: 20px;
    transform:translateY(-50%);
    border-radius: 100%;
  }
  &::after {
    bottom: 0;
    transform:translateY(50%);
    background-color:darken($color, 10%);
  }
}
.h30 {
  height: 30px;
}
.h50 {
  height: 50px;
}
.h70 {
  height: 70px;
}
.h90 {
  height: 90px;
}
.h110 {
  height: 110px;
}
.h130 {
  height: 130px;
}
.controller {
  display: flex;
  justify-content:space-around;
  div {
    background-color:#fff;
    padding: 10px;
    border-radius: 100%;
    cursor: pointer;
    box-shadow:2px 2px #ddd;
    &:hover {
      box-shadow:4px 4px #ddd;
    }
  }
}
```



#### JS部分

```javascript
var cs = window.getComputedStyle;
var dur = 1 * 1000,
    requestAnimationFrame = window.webkitRequestAnimationFrame ||
            window.mozRequestAnimationFrame ||
            window.msRequestAnimationFrame ||
            window.oRequestAnimationFrame;

function QuickSort() {
  this.init();
}
QuickSort.prototype.init = function() {
  this.container = document.querySelector('.container');
  this.containerWidth = getNodeWidth(this.container);
  this.cylinderWidth = getNodeWidth(this.container.querySelector('.cylinder'));
  this.childrenNum = this.container.children.length;
  this.spaceWidth = (this.containerWidth -  this.childrenNum*this.cylinderWidth)/(this.childrenNum-1);  
}
QuickSort.prototype.swap = async function(a, b) {
  let dis = this.cylinderWidth + this.spaceWidth;
  return new Promise((resolve, reject)=> {
    let indexOfa = getIndexOfParent(a),
        indexOfb = getIndexOfParent(b);
    requestAnimationFrame(indexOfa > indexOfb ? 
                          genAnim(b, a,(indexOfa-indexOfb)*dis) :
                          genAnim(a, b,(indexOfb-indexOfa)*dis)
                         );
    function genAnim(a, b, length) {
      var startTime = new Date,
      timerId = 1;
      return function animFun(time) {
        if(!timerId) return false;
        var per = Math.min(1.0, (new Date - startTime) / dur);
        if(per >= 1) {
            timerId = null;
            swapNode(a, b);
            a.style.left = 'auto';
            b.style.right = 'auto';
            resolve('finish!');
        } else {
            a.style.left = Math.round(length * per) + "px";
            b.style.right = Math.round(length * per) + "px";
            requestAnimationFrame(animFun);
        }
      }
    }
  });
}
QuickSort.prototype.run = async function() {
  let container = this.container;
  let self = this;
  async function paritial(left, right) {
    let i = left, j = right, privot = left;
    let flag = true;
    while(i < j) {
      let freshArr = container.children;
      if (flag) {
        if (getNodeHeight(freshArr[privot]) > getNodeHeight(freshArr[j])) {
          await self.swap(freshArr[privot], freshArr[j]);
          i++;
          privot = j;
          flag = !flag;
        } else {
          j--;
        }
      } else {
        if (getNodeHeight(freshArr[privot]) < getNodeHeight(freshArr[i])) {
          await self.swap(freshArr[i], freshArr[privot]);
          j--;
          privot = i;
          flag = !flag;
        } else {
          i++;
        }
      }
    }
    return privot;
  }
  async function boot(left, right) {
    if (left >= right)
      return;
    let index = await paritial(left, right);
    await boot(left, index-1);
    await boot(index+1, right);
  }
  await boot(0, this.childrenNum-1);
};

function getNodeWidth(node) {
  return cs(node).getPropertyValue('width').replace('px', '') - 0;
}
function getNodeHeight(node) {
  return cs(node).getPropertyValue('height').replace('px', '') - 0;
}
function getIndexOfParent(node) {
  let i = 0, p = node, name = node.nodeName;
  while(p) {
    if (p.nodeName === name)
    	i++;
    p = p.previousSibling;
  }
  return i;
}

function swapNode(a, b) {
  if (a === b)
    return;
  let parent = a.parentNode;
  if (getIndexOfParent(a)+1 === getIndexOfParent(b)) {
    parent.insertBefore(b ,a);
  }
  else {
    let h = a.nextSibling;
    parent.replaceChild(a, b);
    parent.insertBefore(b, h);
  }
}

function Container() {
  this.container = document.querySelector('.container');
  this.quickSort = new QuickSort();
}
Container.prototype.rebuild = function(num) {
  let count = num || 5,
      container = this.container;
  while(container.firstChild) {
    container.removeChild(container.firstChild);
  }
	while(count--) {
    let div = document.createElement('div');
    div.classList.add('cylinder');
    div.style.setProperty('height', Math.random()*(200 - 30) + 30+'px');
		container.appendChild(div);
  }
  this.quickSort.init();
}
Container.prototype.sort = async function() {
  await this.quickSort.run();
}

var container = new Container();
container.sort();

document.querySelector('.rebuild').addEventListener('click', function(){
  container.rebuild(20);
});

document.querySelector('.sort').addEventListener('click', function(){
  container.sort();
});
```

