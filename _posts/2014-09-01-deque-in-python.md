---
layout: post
published: true
title: 应用：python的deque
---

网站有一个需求，需要这么一个全局的（容器）对象，可迭代内部的item，可限制数量，当在1端加入新item时，末端相应地就自动删除1个。
利用这个容器，可以将最近用户搜索过的关键词展示出来，展示最近10个，作为全局变量随时使用，更新，不固化到文件或数据库里。

python 自带了一个这样的东西：deque （音 deck）

```
from collections import deque

a_deque = deque([], 10) # 生成一个空的deque，限制为10个元素
```

当发生了搜索行为时:
```
a_deque.appendleft(keyword)
```

如果该关键词已存在于这个deque：
```
a_deque.remove(keyword)
```

python 的list也能实现相似的操作，但使用deque更高效，无论从哪一端增加元素，时间复杂度都是O(1),删除指定元素也是O(1)，并可以指定最大长度。
有了deque，就不用自己去实现这个“一端进、一端出”的队列对象。
 
[liyuwiki.com](http://liyuwiki.com) 上的效果:
 
![deque in liyuwiki]({{baseurl}}/images/2014-09-01-deque-in-python.jpg)

ps，推荐书籍：
《Problem Solving with Algorithms and Data Structures》这本书很详细地解释了python中几个数据结构的例子，实现代码。其中就包括了deque。另外还有stack, queue, unordered list, ordered list等，对理解数据结构很有帮助（但此书中的deque是教学案例，与python真正的deque也就是我前面应用的略有差异）。全书使用python 3作为唯一教学语言，跟传统的用C、C++解释数据结构的书相比，此书完全不涉及硬件或内存，依我看，就是更加抽象。行文也较为细致详尽，完全面向新人。不管怎么样，从python的角度来教授数据结构、算法，在市面上凤毛麟角，打算深入python的程序员们，不必要再学其他语言了，就这本已很好。免费。篇幅不大，pdf 236页A4。
[http://interactivepython.org/runestone/static/pythonds/index.html](http://interactivepython.org/runestone/static/pythonds/index.html)
