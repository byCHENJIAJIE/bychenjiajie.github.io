---
layout:     post
title:      "canvas中strokeRect的渲染问题"
subtitle:   "strokeRect把一像素的边框渲染成两像素"
date:       2017-05-01 10:19:09
author:     "Jacob"
header-img: "img/post-bg-2015.jpg"
tags:
    - 前端学习
    - HTML5
---

## > 结论写在头

```js
var oC = document.getElementById('c1');
var oGC = oC.getContext('2d');

oGC.strokeRect(50,50,100,100);//默认绘制黑色一像素的线
```

像这个用canvas绘制出一个方形的时候，由于设置的top值和left值是50px，所以canvas会在第50和第51个像素之间**从中间开始绘制**一像素的线，第50和第51个像素各占0.5像素。

计算机并不能渲染0.5个像素，所以导致第50和第51个像素都被渲染了，渲染的颜色就成了灰色。（白加黑：背景色加线的颜色）

![](http://oorg1sbrd.bkt.clouddn.com/snipaste_20170501_105937.png)

## > 解决办法

既然会因为0.5像素的问题而渲染了两个像素，那么在设置top值和left值时，增加或减少0.5像素就可以解决了。想绘制在第51个像素就设置50.5，想绘制在第50个像素就设置49.5。



另外，值得注意的是，画出来的方形大小只有99\*99像素，要除去一边线的宽度。