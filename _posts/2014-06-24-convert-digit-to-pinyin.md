---
layout: post
published: true
title: python 小程序：转换阿拉伯数字到拼音
---

 python上提供转换汉字到拼音的包有很多，但是好像都没有针对阿拉伯数字转化到拼音的方法，  
  我这里写了一个，供有需要的朋友参考。  

  函数 `digi_to_py`  

  输入字符串： '上海1943'、'1024'、'1949大江大海'  ...  
  得到字符串： '上海 yi jiu si san'、'yi ling er si'、'yi jiu si jiu 大江大海'  ....  
 
```
import re

def digi_to_py(word):
      digidict = {
        '1': u'yi', '2': u'er', '3': u'san', '4': u'si', '5': u'wu', 
        '6': u'liu', '7': u'qi', '8': u'ba', '9': u'jiu', '0': u'ling'
        }
      patten = '([0-9]{1})'
      for i in word:
           if re.search(patten, i):
                py = digidict[i]
                word = word.replace(i, (u' ' + py + u' '))
     return word
```

  之后再使用其他汉字转拼音函数，即可得到不包含阿拉伯数字的完全的拼音字符串。
 
  这样就避免了在按字母顺序排列的场合中，阿拉伯数字开头的item优先的情况。
  具体实例：[http://liyuwiki.com/577297.html](http://liyuwiki.com/577297.html)  
  如不经过这一步转化  “1024”这样的词条就排到所有条目的前面去。


补充:  
这个函数可以重写，直接用key匹配目标，，而不必使用正则表达式。
