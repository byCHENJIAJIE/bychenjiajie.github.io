---
layout:     post
title:      "关于beginPath()和closePath()的关系"
subtitle:   "canvas的beginPath和closePath分析总结，包括多段弧的情况"
date:       2017-05-01 15:52:57
author:     "cnblogs-妙音天女"
header-img: "img/post-bg-2015.jpg"
tags:
    - 前端学习
    - HTML5
    - 转载
---

今天查了一下beginPath()和closePath()关于区域的划分问题，发现到一篇解释得很明白的文章，我就直接转载到这里了。

原文请看：[canvas的beginPath和closePath分析总结，包括多段弧的情况](http://www.cnblogs.com/xuehaoyue/p/6549682.html)，作者是：[妙音天女](http://www.cnblogs.com/xuehaoyue/)



先看两个例子

## > 例1：          

```js
<canvas id="myCanvas" width="300" height="300" style="border:1px solid #000000;">
    您的浏览器不支持 HTML5 canvas 标签。
</canvas>
<script>
    var ctx = document.getElementById("myCanvas").getContext('2d');
    ctx.beginPath();
    ctx.moveTo(100.5,20.5);
    ctx.lineTo(200.5,20.5);
    ctx.strokeStyle = 'black';//默认strokeStyle='black', lineWidth=1, 此处可省略
    ctx.stroke();

    ctx.beginPath();
    ctx.moveTo(100.5,40.5);
    ctx.lineTo(200.5,40.5)
    ctx.strokeStyle = 'red';
    ctx.stroke();
</script>
```

结果：

![](http://upload-images.jianshu.io/upload_images/4334050-554f5b0120f78c32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=1025222963)

## > 例2：去掉第2个beginPath()           

```js
<canvas id="myCanvas" width="300" height="300" style="border:1px solid #000000;">
    您的浏览器不支持 HTML5 canvas 标签。
</canvas>
<script>
    var ctx = document.getElementById("myCanvas").getContext('2d');
    ctx.beginPath();
    ctx.moveTo(100.5,20.5);//①
    ctx.lineTo(200.5,20.5);//②
    ctx.strokeStyle = 'black';//默认strokeStyle='black', lineWidth=1, 此处可省略
    ctx.stroke();

    ctx.moveTo(100.5,40.5);//③
    ctx.lineTo(200.5,40.5)//④
    ctx.strokeStyle = 'red';
    ctx.stroke();
</script>
```

 结果：

![](http://upload-images.jianshu.io/upload_images/4334050-afd7dc5060ecfb26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=676773787)

## > beginPath

- canvas中的绘制方法（如stroke, fill），都会以“上一次beginPath”之后的所有路径为基础进行绘制。比如例2中stroke了两次，都是以第一次beginPath后的所有路径为基础画的。 
  - 第一次stroke：画①②，黑色
  - 第二次stroke：画①②③④，红色（其中①②红色覆盖之前的黑色）
- 不管你用moveTo把画笔移动到哪里，只要不beginPath，那你一直都是在画一条路径（注：此处『一条路径』并非指连在一起）
  - fillRect与strokeRect这种直接画出独立区域的函数，也不会打断当前的path.

## > beginPath

- closePath的意思不是结束路径，而是关闭路径，它会试图从当前路径的终点连一条路径到起点，让整个路径闭合起来。但是，这并不意味着它之后的路径就是新路径了
- 与beginPath几乎没有关系：不要企图通过闭合现有路径来开始一条新路径，而开始一条新路径，以前的路径也不会闭合。



对于绘制多段弧，看下面几个例子：

## > 例3：

```js
var context = document.getElementById("myCanvas").getContext('2d');
context.strokeStyle="#005588";
for (var i = 0; i < 10; i ++){
    context.beginPath();
    context.arc(50 + i*100,60,40,0,2*Math.PI*(i+1)/10);
    context.closePath();
    context.stroke();
}
```

 结果：

![](http://upload-images.jianshu.io/upload_images/4334050-25cad9e0ab38c434.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=258733916)

## > 例4：在例3的基础上只去掉closePath()           

```js
var context = document.getElementById("myCanvas").getContext('2d');
context.strokeStyle="#005588";
for (var i = 0; i < 10; i ++){
    context.beginPath();
    context.arc(50 + i*100,60,40,0,2*Math.PI*(i+1)/10);
    //context.closePath();
    context.stroke();
}
```

 结果：

![](http://upload-images.jianshu.io/upload_images/4334050-0bb5fd510067c93b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=258733916)

## > 例5：在例3的基础上去掉beginPath()和closePath()           

```js
var context = document.getElementById("myCanvas").getContext('2d');
context.strokeStyle="#005588";
for (var i = 0; i < 10; i ++){
    //context.beginPath();
    context.arc(50 + i*100,60,40,0,2*Math.PI*(i+1)/10);
    //context.closePath();
    context.stroke();
}
```

  结果：

![](http://upload-images.jianshu.io/upload_images/4334050-1d19f2514750f19e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=676773787)

可见，在这种情况下，每个弧画完都会连到下一个弧的起点

## > 例6：在例3的基础上只去掉beginPath()

```js
var context = document.getElementById("myCanvas").getContext('2d');
context.strokeStyle="#005588";
for (var i = 0; i < 10; i ++){
    //context.beginPath();
    context.arc(50 + i*100,60,40,0,2*Math.PI*(i+1)/10);
    context.closePath();
    context.stroke();
}
```

   结果：![](http://upload-images.jianshu.io/upload_images/4334050-d4a3e7c9adcf0a67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=1026329061)

 这样看不太清晰，我们将`i < 10`改为`i < 3`，只显示前三个：

![](http://upload-images.jianshu.io/upload_images/4334050-67a18e6ce10735a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=1025222963)

可见，在这种情况下，每个弧画完都会先回到第一个弧的起点，然后再连到下一个弧的起点

## > 例7：在例3的基础上将stroke()改为fill()

```js
var context = document.getElementById("myCanvas").getContext('2d');
    context.fillStyle="#005588";
    for (var i = 0; i < 10; i ++){
        context.beginPath();
        context.arc(50 + i*100,60,40,0,2*Math.PI*(i+1)/10);
        //context.closePath();
        context.fill();
}
```

 结果：

![](http://upload-images.jianshu.io/upload_images/4334050-ea46f5c7a95d81e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=258733916)

## > 例8：在例7的基础上去掉closePath()

```js
var context = document.getElementById("myCanvas").getContext('2d');
    context.fillStyle="#005588";
    for (var i = 0; i < 10; i ++){
        context.beginPath();
        context.arc(50 + i*100,60,40,0,2*Math.PI*(i+1)/10);
        context.closePath();
        context.fill();
}
```

结果：

![](http://upload-images.jianshu.io/upload_images/4334050-c686b61bb947cf6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240&_=1025222963)

## > 比较例7和例8可知：

无论是否closePath()，结果都一样。
因为closePath()对于fill()是没有用的：无论是否closePath()，调用fill()时，canvas会自动把没有封闭的路径首尾相连，之后进行填充。