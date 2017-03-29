---
layout: post
title: 第一个Ajax调用
date: 2017-01-12
description: 本次尝试首先是在php+mysql+Apache环境下，通过javascript实现Ajax例子。
categories: [技术]
tags: 
- Ajax
- javascript
---
<h1>第一个Ajax调用</h1>
<p>第一个Ajax调用：</p>
本次尝试首先是在php+mysql+Apache环境下，通过javascript实现Ajax例子。
以下是代码核心:<br />
<h2>javascript部分</h2><br />
<p>
<pre>
function Ajax(){
var xmlHttpReq= null;
if(window.ActiveXObject){
xmlHttpReq =new ActiveXObject("Microsoft.XMLHTTP");
}else if(window.XMLHttpRequest){
xmlHttpReq =new XMLHttpRequest();
}
if(xmlHttpReq != null){
xmlHttpReq.open("GET","test.php",true);
xmlHttpReq.onreadystatechange= RequestCallBack;
xmlHttpReq.send(null);
				}
function RequestCallBack(){
if(xmlHttpReq.readyState == 4){
if(xmlHttpReq.status == 200){
document.getElementById("resText").innerHTML =xmlHttpReq.responseText;
			}
		}
	}
}
</pre>
</p>
<h2>html代码</h2>
<p>以下是html的完整代码</p>
<p>
<pre>
&lt;!DOCTYPE HTML&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;title&gt;Ajax测试页&lt;/title&gt;
&lt;script type="text/javascript"&gt;
&lt;/script&gt;
&lt;style type="text/css"&gt;
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
	&lt;input type="button" value="Ajax提交" onclick="Ajax();" /&gt;
	&lt;div id="resText"&gt;&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>
</p>

<h2>PHP代码</h2>
<p>php代码较为简单,保存为text.php</p>
<p>
<pre>
&lt;?php
echo "hello Ajax!";
?&gt;
</pre>
</p>
代码粘贴的一个小问题：如何在markdown格式下粘贴完整的html代码？
目前我摸索的解决方式：使用转义符。

