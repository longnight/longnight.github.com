---
layout: post
published: true
title: Django Template中 添加 list 的办法
---

假设有一个无context 变量传入的静态模版，  
你需要加入一个自定义的list ： [1, 2, 3, 4]，  
并随机取出里面的item，  
可以使用这几行  


{% raw %}
```
{% with '1 2 3' as alist %}
    {% for i in alist.split|random %}
        <br /><img src="static/images/0{{ i }}.jpg" />
    {% endfor %}
{% endwith %}
```
{% endraw %}


这里提供了一个在template tag 中 获取python语句结果的方法

[https://djangosnippets.org/snippets/9/](https://djangosnippets.org/snippets/9/)  

ExprTag - Calculating python expression and saving the result to a variable
