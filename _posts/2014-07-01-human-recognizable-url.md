---
layout: post
published: true
title: Django实践：设计人类易于识别的url
---

`http://liyuwiki.com/term/?q=%E7%8C%B4%E6%9C%BA`  
`http://liyuwiki.com/929619.html`  

以上两条url，显然第二条更容易理解、记忆、并在各种场合中传播。相信很多人都有过这样的经历：把网址拷贝下来贴到论坛或QQ里，却被对方截短，失效了。  
基于这样的想法，我认为把标题写到url里这种方案不适合中文。
 
那么在url中用数字如何？  
见过不少网站直接在url中展示该篇文章在数据表中的primary key，妾以为甚不可取，似乎向黑客们暴露了什么。
所以我使用了这个一个方案：生成每条数据除了数据库分配给它1个pk id之外，还额外生成1个6位随机阿拉伯数字构成的uid。为什么不使用（大小写）英文字母组合？我认为普通用户会更排斥后者，字母远比数字令普通用户退避三舍。
这个生成uid的函数写起来也不复杂。但作为生产环境需要多做1步：检测生成的结果是否已存在于数据表，若是，则再来1次，直至得到这个独一无二的随机6位数。

函数：term_uid()  
输入：无  
输出：1个与表内已有的记录不冲突的6位数。  
```
import random

def term_uid():
    uid = random.randint(100000, 999999)
    while Terms.objects.filter(uid=uid).exist():
        uid = random.randint(100000, 999999)
    return uid
```
