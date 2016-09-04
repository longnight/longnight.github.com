---
layout: post
published: true
title: 记录下现实中遇到的陈列商品问题的解决思路
---

设有n个商品需要一并展示在网站首页，每行3个，n的值是变动的。不分页。  
那么情形大致如下图：  

![goods]({{baseurl}}/images/2014-08-27-how-to-list-goods.jpg)

面临的问题是：当n不是3的倍数时，末行总会出现空档，即问号所示，有时为1个，有时为2个.  
怎么解决？  
有2个办法：  

1，在末行补上广告图片placeholder:

```
if n % 3 != 0:
     codes to show first placeholder img...
         if n % 3 == 1:
             codes to show second placeholder img...
```

2，删除多出的一两个商品，不予展示。用函数来表达，就是已知有n个商品，求n下最大的3的倍数m，展示m个商品。  

```
def multiple_3(n):
      while n > 2 and n % 3 != 0:
          n -= 1
      return n
```

以上应可以用lambda一行到位，这里只是记录一下思路。
