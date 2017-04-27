---
layout: post
title: 'HTML5 重力感应deviceorientation与orientationchange(转载)'
subtitle: '今天做移动端横竖屏照片，视频自适应。这是找到的一个资料。写的很好'
data: 2017-04-27 17:28
author: 'marillo'
header-img: "img/about-bg.jpg"
catalog: true
tags: 
    - JavaScript
---

#### 一、前言

***

随着智能手机越来越普及，而一般手机都开始提供了重力感应喝重力加速的接口，我们在制作HTML5页面的过程中，可以利用deviceorientation(检测设备方向事件)、orientationchange(屏幕旋转事件)，来做指南针，摇一摇，地图等这些应用。

#### 二、主要内容

***

> 1、orientationchange事件在屏幕发生旋转时触发，window.orientation可获得设备的方向，一共有三个值0：竖直，90：右旋，-90：右旋
>
> 2、deviceorientation提供设备的物理方向信息，表示为一系列本地坐标系的旋角
>
> 3、MozOrientation(firefox专用)事件中可获得三个值z,x,y，分别代表垂直加速度，左右的倾斜角度，前后的倾斜角度（取值范围：－1～1）

#### 三、用法简介

***

要接受设备方向变化信息，需要注册监听`deviceorientation`事件：

注册一个`deviceorientation`事件的接收器：

```javascript
window.addEventListener("deviceorientation", handleOrientation, true);
```

注册完事件监听处理函数后（对应这个例子中的`handleOrientation`)，监听函数会定期地接受到最新的设备方向数据。

方向事件对象中包含四个值

> DeviceOrientationEvent.absolute
>
> DeviceOrientationEvent.alpha //表示设备沿z轴上的旋转角度，范围为0~360
>
> DeviceOrientationEvent.beta //表示设备在x轴上的旋转角度，范围为-180~180。它描述的是设备由前向后旋转的情况
>
> DeviceOrientationEvent.gamma //示设备在y轴上的旋转角度，范围为-90~90。它描述的是设备由左向右旋转的情况

手机中的方位轴：

![](http://img.pfan123.com/deviceorientation.png)

![](http://img.pfan123.com/deviceorientation1.png)

![](http://img.pfan123.com/deviceorientation2.png)

![](http://img.pfan123.com/deviceorientation3.png)



下面是一个事件处理函数的例子：

```javascript
function handleOrientation(orientData) {
  var absolute = orientData.absolute;
  var alpha = orientData.alpha;
  var beta = orientData.beta;
  var gamma = orientData.gamma;

  // Do stuff with the new orientation data
}
```

判断屏幕是否旋转

```javascript
function orientationChange() {
    switch (window.orientation) {　　
        case 0:
            alert("肖像模式 0,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;　　
        case -90:
            alert("左旋 -90,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;　　
        case 90:
            alert("右旋 90,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;　　
        case 180:
            　　alert("风景模式 180,screen-width: " + screen.width + "; screen-height:" + screen.height);　　
            break;
    };
};
```

我这次在vue中使用的这个方法。旋转的时候，根据当前屏幕的宽度来调整游记照片和视频的高度,以下是部分代码

```javascript
  mounted() {
    window.addEventListener('deviceorientation', this.handleOrientation, true);
  },
  methods: {
    handleOrientation() {
      const resizeWidth = document.body.clientWidth - 30;
      if (window.orientation === 90 || window.orientation === -90) {
        this.isResize = true;
        if (this.wp) {
          if (this.wp.video) {
            const videoPercent = this.wp.video.width / this.wp.video.height;
            this.videoHeight = Math.ceil(resizeWidth / videoPercent);
          }
        }
      } else if (window.orientation === 180 || window.orientation === 0) {
        this.isResize = true;
        if (this.wp) {
          if (this.wp.video) {
            const videoPercent = this.wp.video.width / this.wp.video.height;
            this.videoHeight = Math.ceil(resizeWidth / videoPercent);
          }
        }
      }
    },
```

