---
layout: post
title: 实现背景模糊方法
date: 2017-02-18
description: "实现背景模糊的方法,要使背景模糊，内容不模糊，主要用到...."
categories: [技术]
tges:
- css
- html
---
<h1>css中实现背景模糊以及背景图片固定的方式</h1>

```javascript
.divname{
	position:relative;
	overflow:hidden;//为了去除边缘模糊
}
.divname::before{
	content:'';
	position:absolute;
	top:0;right:0;bottom:0;left:0;
	background:url('你的图片url')  fixed;
	z-index:-1;
	-moz-filter: blur(4px);
    -webkit-filter: blur(4px);
    -o-filter: blur(4px);
    -ms-filter: blur(4px);
    filter: blur(4px);//模糊
	margin:-30px;//为了去除边缘模糊
}
```
<p>这样实现的divname的背景就是模糊的，原理实际上是在divname中加入了before伪元素，<br />
这个伪元素的的背景覆盖了divname，filter:blur(5px)d的作用使伪元素背景变模糊了。<br />

```javascript
.divname{
	position:relative;
	}
	.divname::before{
	content:'';
	position:absolute;
	top:0;right:0;bottom:0;left:0;
	background:url('你的图片url')  fixed;	
	-moz-filter: blur(4px);
    -webkit-filter: blur(4px);
    -o-filter: blur(4px);
    -ms-filter: blur(4px);
    filter: blur(4px);//模糊
	}
```
不过此时，你会发现背景确实模糊了，但是在divname上的元素也变模糊了，所以为了解决这个问题就要用z-index使伪元素在divname下面。<br />
```javascript
.divname{
	position:relative;
}
.divname::before{
	content:'';
	position:absolute;
	top:0;right:0;bottom:0;left:0;
	background:url('你的图片url')  fixed;
	z-index:-1;
	-moz-filter: blur(4px);
    -webkit-filter: blur(4px);
    -o-filter: blur(4px);
    -ms-filter: blur(4px);
    filter: blur(4px);//模糊
}
```
<p>还有一个问题就是你会发现divname的边缘也是模糊的这样很是不好看，<br />所以就要使这个伪元素边距要比divname边距大30px(margin:-30px的作用),由于overflow:hidden的作用边缘大的部分就被裁剪了</p>
```javascript
	.divname{
	position:relative;
	}
	.divname::before{
	content:'';
	position:absolute;
	top:0;right:0;bottom:0;left:0;
	background:url('你的图片url')  fixed;	
	-moz-filter: blur(4px);
    -webkit-filter: blur(4px);
    -o-filter: blur(4px);
    -ms-filter: blur(4px);
    filter: blur(4px);//模糊
	}
```