---
layout: post
published: true
title: Django cache 系统再思考
---

Django 的cache 系统提供了多个粒度的cache手段，从整站cache ，到 views cache，再到 template层上还可以局面代码cache，简直是想缓存哪里，就缓存哪里。

但问题来了：如果我们的views函数包括了复杂的计算和频繁读取，那么在template层面上的缓存能节省这方面的开支吗？  
答案是不能，因为每次页面request时调用这个view函数，该计算的还得计算，即使你在模板中缓存了局部表现代码，那也只是浪费掉后台的结算结果而已。  
解决办法是？  
这里的这篇博客详细解释了解决思路（需要翻墙）：  
[http://migaat.blogspot.com/2011/11/different-ways-for-low-level-caching-in.html](http://migaat.blogspot.com/2011/11/different-ways-for-low-level-caching-in.html)

![django-cache]({{baseurl}}/images/2014-09-05-thinking-in-django-cache.png)

有点复杂，但核心不外乎检查缓存是否存在，如果不存在再去计算。  
与我前面的实现相比，大同小异，他多了与template 缓存对照的一步，少了检查计算结果为空集的一步。  

在俚语维基的实践表明：  
1，页面载入时间大部分消耗于计算相似词条、投票统计等上面，开启缓存节省掉这俩的计算之后，所有页面载入速度提升了几倍，部分内容复杂的词条提升了近10倍；  
2，每个页面即使执行数据库100-300次queries，速度仍然非常快，与后台的计算相比，这块不是导致打开网页缓慢的第一因素，  
但使用恰当的缓存方案之后，数据库查询骤降至20多次，进一步榨取载入速度——从300-600毫秒减少到30毫秒。  
 
以上数据是本地破电脑跑的，由 django-debug-toolbar 提供。到了服务器上差距不至于有这么大。  
 
PS：  
通常提到cache都是基于指定时间来让它过期的，后来仔细想了下，其实可以按照存取频率来决定过期。  
比如某个视图函数，当被访问100次之后，就更新cache。  
这个不难实现，将原本要读写的缓存键值对加入计数器，然后判断，若>100 即执行删除、再计算即可。  
stackoverflow 上就有人提到过相似的需求，当有访客发表评论时，执行更新缓存：  
[http://stackoverflow.com/questions/2268417/expire-a-view-cache-in-django](http://stackoverflow.com/questions/2268417/expire-a-view-cache-in-django)
