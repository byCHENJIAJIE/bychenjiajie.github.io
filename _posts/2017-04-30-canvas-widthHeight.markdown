---
layout:     post
title:      "为什么canvas宽高要设置在标签内"
subtitle:   "宽高设置在style和设置在canvas的区别"
date:       2017-04-30 22:08:36
author:     "Jacob"
header-img: "img/post-bg-2015.jpg"
tags:
    - 前端学习
    - HTML5
---

一直很困惑为什么`canvas`标签和其他标签不一样，宽高需要设置在`canvas`标签里，设置在`style`里就会有问题。



纯粹个人理解，有错误欢迎指出。

## > 结论写在头

设置在style里有问题其实是因为：

`canvas`标签里的宽高是相当于定义画布的大小（默认宽300px，高150px）。在定义了画布之后，`canvas`就相当于一张图片了，类似于`img`，所以这个时候再设置宽高，就会把`canvas`拉伸成`style`里设置的宽高了。



下面看例子：

## > 默认状态

```js
<style>
	body{background:black;}
	#c{background: yellow;}
	div{background: pink;width: 300px;height: 150px;}
</style>
<script>
	window.onload = function(){
		var oC = document.getElementById('c');
		var oGC = oC.getContext('2d');

		oGC.strokeRect(50,50,100,100);
	}
</script>

<body>
	<canvas id="c"></canvas>
	<div>width: 300px; height: 150px;</div>
</body>
```

![](http://oorg1sbrd.bkt.clouddn.com/snipaste_20170430_232839.png)

在canvas画布下加了一个DIV，可以看到画布的默认大小是300*150px的。

## > 在canvas标签里设置300*300px

```html
<!--其他代码同上-->
<body>
	<canvas id="c" width = "300px" height = "300px""></canvas>
	<div>width: 300px; height: 150px;</div>
</body>
```

![](http://oorg1sbrd.bkt.clouddn.com/snipaste_20170501_093457.png)

这里要注意在canvas标签里设置宽高的方式，是直接写`width = "300px" height = "300px"`，而不是以行间样式的方式设置的。

## > 在canvas标签里设置300\*300px后，style设置成300\*150px

```js
<style>
	body{background:black;}
	#c{background: yellow;width: 300px;height: 150px;}
	div{background: pink;width: 300px;height: 150px;}
</style>
<script>
	//同上
</script>

<body>
	<canvas id="c" width = "300px" height = "300px""></canvas>
	<div>width: 300px; height: 150px;</div>
</body>
```

![](http://oorg1sbrd.bkt.clouddn.com/snipaste_20170501_094457.png)

可以看到，canvas画布的大小的确是被设置成300\*150px了，但里面的内容却被压缩了。

相当于一张300\*300px的图片，被设置为300\*150px，所以参考的长方形被压短了一半。