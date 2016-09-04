---
layout: post
published: true
title: Sublime text 2 中，拼音输入法不跟随光标的解决办法：
---
其实这个问题困扰我很久了，导致sublime text 一直不能扶正成为主力编辑器的重要原因，今晚随手搜一下，一个台湾网友的post已经给出答案了，原来有个日本人写了个小插件: IMEsupport,
windows only. 安装办法：在package control:install package中搜索这个名字即可。
原post 在：
[http://sublimetextlove.tumblr.com/post/41788388348/imesupport](http://sublimetextlove.tumblr.com/post/41788388348/imesupport)

插件在：
[https://github.com/chikatoike/IMESupport](https://github.com/chikatoike/IMESupport)

用了3分钟即发现一个小bug，layout为2 column 时,另一个窗口如没有打开文件，仍可能导致候选词列表不跟随。

![img]({{site.baseurl}}/images/2013-10-27-sublime-text2-input-bugs.jpg)
