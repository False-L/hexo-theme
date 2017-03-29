---
layout: post
title: 百度云爬虫前置-获取百度云分享页面
description: "百度云爬虫前置-获取百度云页面，根据用户id(百度云的id是uk=，可以获取百度云的页面"
date: 2017-03-16
categories: [技术]
tag: 
- javascript
- Node.js
#images: images/120031.jpg
---
首先是获得百度云的api，基本是网上找的，当然自己也可以找到部分，比如随便打开一个百度云的分享页面，用firefox或chrome的调试工具就能找到。

![baiduyun1.gif](baiduyun1.gif)

<!--more-->
# 列表如下
### 热门用户列表
"http://yun.baidu.com/pcloud/friend/gethotuserlist?type=1&from=feed&ampstart=0&amplimit=24&bdstoken=ac95ef31d3979f6ee707ef75cee9f5c5&ampclienttype=0&ampweb=1"

### 粉丝列表
http://yun.baidu.com/pcloud/friend/getfanslist?query_uk=1949404764&limit=20&start=0

### 订阅列表
http://yun.baidu.com/pcloud/friend/getfollowlist?query_uk=1949404764&limit=20&start=0&bdstoken=d82467db8b1f5741daf1d965d1509181&channel=chunlei&clienttype=0&web=1

### 订阅列表2
http://yun.baidu.com/pcloud/friend/getfollowlist?query_uk=809803785&limit=20&start=0&bdstoken=d82467db8b1f5741daf1d965d1509181&channel=chunlei&clienttype=0&web=1

### album分享列表
http://yun.baidu.com/pcloud/album/getlist?start=0&limit=200&query_uk=1949404764&channel=chunlei&clienttype=0&web=1&bdstoken=d82467db8b1f5741daf1d965d1509181

### pc分享列表
http://pan.baidu.com/pcloud/feed/getsharelist?t=1474202771918&category=0&auth_type=1&request_location=share_home&start=0&limit=60&query_uk=1949404764&channel=chunlei&clienttype=0&web=1&logid=MTQ3NDIwMjc3MTkxOTAuMzA1NjAzMzQ4MTM1MDc0MTc=&bdstoken=e6f1efec456b92778e70c55ba5d81c3d

### wap分享列表
http://pan.baidu.com/wap/share/home?uk=1949404764&start=0&adapt=pc&fr=ftw

http://pan.baidu.com/wap/share/home?uk=2974844195&start=0&adapt=pc&fr=ftw

### pc版分享页面
https://pan.baidu.com/share/link?uk=1949404764&shareid=3867072473

### pc版 专辑页面
https://pan.baidu.com/pcloud/album/info?uk=1949404764&album_id=1481611372140163606

### wap版 专辑页面
https://pan.baidu.com/wap/album/info?uk=1949404764&album_id=1481611372140163606
