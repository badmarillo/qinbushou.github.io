---
layout: post
title: "数组去重"
date: 2017-03-07 10:32:00
author: "老攻阿白"
header-img: "img/about-bg.jpg"
catalog: true
tags:
    - JavaScript
---





#### for循环删除后面重复的

```javascript
function uniqueFor(arr) {
  for (var i = 0; i < arr.length - 1; i++) {
    var item = arr[i];
    for (var j = i + 1; j < arr.length; j++) {
      item === arr[j] && (arr.splice(j, 1), j--);
    }
  }
  return arr;
}
```

#### 判断对象属性

```javascript
function uniqueObject(arr) {
  var val, 
      newArray = [], 
      obj = {};
  for (var i = 0; (val = arr[i]) !== undefined; i++) {
    !obj[val] && (newArray.push(val), obj[val] = true);
  }
  return newArray;
}
```

#### 数组过滤重复项filter

```javascript
function uniqueFilter(arr) {
  return arr.filter(function(value, index, arr) {
    return self.indexOf(value) === index;
  })
}
```

