---
layout: post
published: true
title: Django实践：获取随机图片地址的自定义templat tag
---

这个自定义的tag应用在 [liyuwiki.com](http://liyuwiki.com)，当用户搜索返回没有结果的页面时，该tag会在指定目录下随机获取一个图片文件地址。
比如：[http://liyuwiki.com/search/?q=non-exist](http://liyuwiki.com/search/?q=non-exist) 这个搜索结果页面每次呈现的图片都不同。

1，在app目录下建立文件夹templatetags\，再在其中建立__init__.py、random_img.py
2，random_img.py 内容如下：
```
import os
import random
import posixpath
from django import template
from django.conf import settings

register = template.Library()

def is_image_file(filename):
    """Does `filename` appear to be an image file?"""
    img_types = [".jpg", ".jpeg", ".png", ".gif"]
    ext = os.path.splitext(filename)[1].lower()
    return ext in img_types

@register.simple_tag
def random_img(path):
    """
    Select a random image file from the provided directory
    and return its href. `path` should be relative to MEDIA_ROOT.
    Usage:  {% raw %}<img src='{% random_image "images/whatever/" %}'>{% endraw %}
    """
    fullpath = os.path.join(settings.MEDIA_ROOT, path)
    filenames = [f for f in os.listdir(fullpath) if is_image_file(f)]
    pick = random.choice(filenames)
    return posixpath.join(settings.MEDIA_URL, path, pick)
```

3，在template中导入这个tag 后即可使用:
{% raw %}
```
{% load random_img %}

<img src='{% random_img "your_path" %}' />
```
{% endraw %}

这段代码实际出自[这里](https://djangosnippets.org/snippets/150/)，我作了非常轻微的一点小修改，避免了原作者忽略大写文件扩展名的疏忽。
不仅可以用于随机图片，也可以随意选择其他文件，css，js，mp3，html等，这样一想似乎可以产生一些有趣的点子。  
补记：以上这段自定义tag写于2007年。2008出版的《Python Web Development with Django》(Jeff 
Forcier,Paul Bissex,Wesley Chun) 有一个几乎完全相同的版本，可能就是借鉴前面的吧。  
```
import os
import random
import posixpath
from django import template
from django.conf import settings

register = template.Library()

def files(path, types=[“.jpg”, “.jpeg”, “.png”, “.gif”]):
    fullpath = os.path.join(settings.MEDIA_ROOT, path)
    return [f for f in os.listdir(fullpath) if os.path.splitext(f)[1] in types]
@register.simple_tag
def random_image(path):
    pick = random.choice(files(path))
    return posixpath.join(settings.MEDIA_URL, path, pick)
```

这本书还提供了一个可以选择扩展名的进化版：  

```
@register.simple_tag
def random_file(path, ext):
    pick = random.choice(files(path, ["." + ext]))
    return posixpath.join(settings.MEDIA_URL, path, pick)
```

在模板里这样用：  

{% raw %}
```
<img src="{% random_file "special/icons" "png" %}" />
```
{% endraw %}
