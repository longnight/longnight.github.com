---
layout: post
published: true
title: 使用Tornado实现网易评论的递归嵌套盖楼效果
---


为了探索Tornado的Coroutine、Future，写了这个玩意儿。  
思路基本是按照以前这两篇日志的  
[http://maruta.be/1984/94](http://maruta.be/1984/94)
[http://maruta.be/1984/95](http://maruta.be/1984/95)

原本基于Django实现，现在改为Tornado实现，  
改动的地方是ORM操作的东西变成了自己写些raw sql，  
Coroutine、Future这些概念一下子比较难透彻理解，  
又包含了递归函数，局面变得比一般练习代码复杂不少。   
因此还有改进的地方。  
这个项目保存于 [https://github.com/longnight/Netease_style_guestbook](https://github.com/longnight/Netease_style_guestbook)
 
 
共有3个方案  
1,  
app.py:   
tornado + MySQL  

2,  
app_coroutine.py:   
tornado + MySQL + coroutine  

3,  
app_motor.py:  
tornado + Mongodb + coroutine  

实际效果是这样的：  
 [tornado_netease3.jpg]({{baseurl}}/images/2014-09-23-tornado-implement-163-style-comment.jpg)
