---
layout: post
published: true
title: 163 网易评论的无限嵌套 代码样例 之 2
---

之前的代码已经基本实现，但是很不优雅，因为传给模版系统的是字符串而非变量，后期就无法灵活改动。

经过思考，终于找到几近完美的办法，思路就是重写递归相关的函数，做到接收1个评论id，输出一个字典，字典结构：  `{'<div class="commant"><div class="commant">...':[querySet1,querySet2,querySet3...]}`，然后，在将这样的字典加入到列表中，最后将列表传到模版系统。模版系统方面进行多次迭代取出n个`<div class="comment">`以及相应的querySet，然后取值，逐个补上</div>结尾。最后就是一个非常灵活的局面，div wrap 层和querySet都很好的兼顾到了。  
下面附上代码和效果：  
 
递归函数：  
```
def mult_div_qs(qs_id, mdq_dict={}, sum=0, qs_list=[]):
    c = Comments.objects.all()
    head_str = '<div class="comment">'
    if not c.filter(id=qs_id):
        return mdq_dict
    else:
        qs = c.get(id=qs_id)
        sum = sum + 1
        qs_list.insert(0, qs)
        key = str(head_str * sum)
        mdq_dict = {}
        mdq_dict[key] = qs_list
        if not qs.quote_who:
            return mdq_dict
        else:
            qs_id = qs.quote_who
            return mult_div_qs(qs_id, mdq_dict, sum, qs_list)
```
view 的代码:  
```
def index(request):
    c = Comments.objects.all().order_by('-id')
    addcomform = Addcomform()
    qs_list = []
    for i in c[:80]:
        div_qs = mult_div_qs(i.id, {}, 0, [])
        qs_list.append(div_qs)
    context = {'c_list': c, 'addcomform': addcomform, 'qs_list': qs_list}
    return render_to_response('index.html', context, context_instance=RequestContext(request))
```

Template那边的代码（注意要加上autoescape off）:  
```
{% raw %}
{% for i in qs_list %}
    {% for j, k in i.items %}
       {% autoescape off %} {{ j }} {% endautoescape %}
            {% for l in k %}
                <span class="id">ID:{{ l.id }}</span>&nbsp;
                <span class="quote_who">quote_who:{{ l.quote_who|default:"未引用" }}</span>&nbsp;
                <span class="author">作者:{{ l.author|default:"匿名" }}</span>&nbsp;
                <span class="created_time">发表:{{ l.created_time|date:"H:i:s" }}</span><br />
                <span class="comment_text">{{ l.comment }}</span></div>
            {% endfor %}
    {% endfor %}
{% endfor %}
{% endraw %}
```
这样，通过递归思想，寥寥几十行就完成了很多程序无法做到的嵌套效果。  
并且模版系统接收到的数据是完整可迭代的queryset，也就意味着随意使用了。  
