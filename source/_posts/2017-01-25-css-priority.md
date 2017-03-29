---
layout: post
title: css优先级
description: "css优先级不可忽视"
date: 2017-01-25
categories: [技术]
tags: 
- css
images: images/119548.jpg
  
---
<h1>css优先级</h1>
<p>css优先级:内联样式>内部样式>外部样式></p>
<!--more-->
<pre>
&lt;head&gt;
&lt;style type="text/css"&gt;
/*内部样式*/
.pcss{ color:green;}
&lt;/style&gt;
&lt;link rel="stylesheet" type="text/css" href="style.css"/&gt;
&lt;!--引用的样式 外部样式--&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div class="pcss" style="color:blue"&gt;
&lt;!--内联样式 style=""--&gt;
颜色
&lt;/div&gt;
&lt;/body&gt;
</pre>
<p>.pcc颜色为内联样式的蓝色，而内部样式和外部样式无法作用</p>
<P>选择器优先权：内联样式>id选择器>类选择器>元素选择器</p>




