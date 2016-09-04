---
layout: post
published: true
title: 利用cache在多个网站实例中共享数据
---

前文提到liyuwiki使用了一个全局变量来储存访客搜索过的关键词，  
由于网站是在nginx后跑起多个实例的，  
那么就必须让这个对象被共享，大家都可以读取、修改。  
解决办法就是利用cache，  
在django中，一旦在settings里定义好cache的后台之后，其他的用法都是一样的，可移植，  
跟orm一样可以随意写，非常方便。  

操作cache的伪代码如下：
```
the_cache = get_cache('default')

if the_cache.get('my_key'): # 如果该缓存已存在：
      if the_cache.get('my_key') == 'non_exist': # 检查该缓存是否为某个指定标记
          my_value = []
      else:
          my_value = the_cache.get('my_key')
else: # 该key尚未被计算过，开始：
      generate my_value code block... # 开始处理。这些计算仅发生在第一次；保存到cache后便不再发生
      if my_value: # 将计算结果写入cache
          the_cache.set('my_key', my_value,  60*20)
      else: # 如果计算结果为空集，则往cache写入一个值
          the_cache.set('my_key, 'non_exist',  60*20)
```

django文档里说“so-so traffic”的小型站点没必要使用cache，但是liyuwiki每个页面上都有些过于昂贵的计算开销，比如寻找相关词条，这种计算要么固化到数据库要么缓存，否则每次都运算一回那也太浪费，有必要优化一下。果然保存到cache里之后， 速度快了很多。
