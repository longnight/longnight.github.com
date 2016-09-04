---
layout: post
published: true
title: template Philosophy, Django vs Tornado
---

最近因为工作需要，接触了Tornado的模板，感受到了与Django的不同，
tornado的模板几乎可以将任何python语句塞进去，比如：

判断：
{% raw %}
```
{% if variable %} 
    blabla...
{% else %}
    blabla...
{% end %}
```
{% endraw %}

迭代：
{% raw %}
```
{% for item in somethings %}
    blabla
{% end %}
```
{% endraw %}

赋值：
{% raw %}
```
{% set somelist = range(1,10) %}
```
{% endraw %}

列表推导：
{% raw %}
```
{% set a = [x for x in v if x % 2 == 0] %}
```
{% endraw %}

lambda：
{% raw %}
```
{% set x = filter(lambda ...blabla ) %}
```
{% endraw %}
......  
根据tornado的官网文档，还有更多复杂用法，但！当你手中握着锤子时，全世界看上去都像是钉子了，tornado这种自由感让人不知不觉间忘记了逻辑与表现须尽可能分离的祖训，前端同事看到的是到处散落着python语句和变量的html。

再回过头看django 模板的理念：“the template system is meant to express presentation, not program logic..... and the template system will not execute arbitrary Python expressions.”django 不让自己的模板系统发育成另一门语言，觉得这种坚守相当高瞻远瞩。
