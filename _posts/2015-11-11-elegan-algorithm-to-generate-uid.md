---
layout: post
published: true
title: 使用UID,和一个可以逆推的生成UID方法
---

在某些场合,暴露数据库的记录ID，譬如这样的： http://domain.me/user/7878 ,  
可能会带来危险，因此我们应该使用UID之类的东西来代替它。  
最简单的做法是：  
随机生成无规律、url安全的字符串作为UID， 然后保存在数据库里；  
需要时通过这个UID查询到相应的数据库记录。  
但是保存UID在数据库里，可以说是向数据库添加并使用不必要的东西来查询，因此是违反了数据库设计范式的。  
同时也因为增加查询而影响性能。  
更好的方案是，寻找一个可靠的算法，它应当能够：  
* 生成看上去无规则的随机字符；
* 可以指定生成UID的格式，长度；
* 可以逆推，即反向解出来。
 
这篇博客
[http://blog.cryptographyengineering.com/2011/11/format-preserving-encryption-or-how-to.html](http://blog.cryptographyengineering.com/2011/11/format-preserving-encryption-or-how-to.html)介绍了一种名为 *Format Preserving Encryption* 的算法，感觉正好满足以上需求。一个典型的应用场景是：一种商业机器只接受信用卡号码即16位数字格式的输入，而你希望在传输时加密真实的信用卡号码，到达后再还原出来，这种FPE算法就解决了这个问题。

![FPE]({{baseurl}}/images/2015-11-11-elegan-algorithm-to-generate-uid.jpg)
 
这个加密算法在python的实现叫libffx，为了方便使用，我对它包裹封装为如下代码，应用了在自己经手的网站生产环境中，减少触碰数据库，对网站速度有帮助。
是否应该这样使用UID，取决于自己网站性能瓶颈是出现在CPU，还是数据库。
注：这种算法 不可用于敏感数据，如用户密码的加密。
 
 
下载安装好：   
python-dev, python2-dev, or python3.4-dev 之一；  
*libmpc-dev*；  
*gmpy*： https://github.com/aleaxit/gmpy；  
*libffx*： https://github.com/kpdyer/libffx (手动安装 python setup.py install )；  

```
import ffx
 
K = 'can_any_strings'  # AES key must be either 16, 24, or 32 bytes long
 
def ffx_encrypt(id, blocksize=8):
    """FPE encrypt.
    :id Intege: ID to be encrypted
    :blocksize Intege: the result UID length
    """
    ffxObj = ffx.new(K, radix=10)
    D = ffx.FFXInteger(id, radix=10, blocksize=blocksize)
    return ffxObj.encrypt(K, D).to_str()
 
def ffx_decrypt(uid):
    """FPE decrypt.
    :uid string: UID to be decrypted
    """
    ffxObj = ffx.new(K, radix=10)
    D = ffx.FFXInteger(uid, radix=10, blocksize=len(uid))
    return ffxObj.decrypt(K, D).to_int()
```

运行例子：

```
>>> ffx_encrypt(1002, blocksize=8)
'24819966'
>>> ffx_decrypt('24819966')
1002
```
