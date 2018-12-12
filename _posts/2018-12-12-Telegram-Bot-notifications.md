---
layout: post
published: true
title: 使用Telegram Bot来实现推送通知
---

运营UGC时通常都需要某种消息推送服务，当服务器发生关键性事件时，能够即时通知。过去我常使用的email推送，最近发现好像telegram推送也很强大好玩，现在分享一下如何实现。

1, Telegram方面的设置, 首先你得有个telegram账号并安装好桌面端.

1.1 整一个telegram bot, 可以通过点击这里来创建:[TelegramBotFather](https://telegram.me/botfather), 在与它的对话框中输入 `/start` `/newbot` 等指令,按照提示一步步,到最后获得一个新bot,以及一串形如以下的token:

```
987654321:FEDCBA_dfoiuweSWEczgxT7-l4r9Y

```
这串东西不应泄露给他人,否则被人滥用的话会导致该bot被禁止.

1.2  找到你的telegram chat_id:

1.2.1 用客户端往bot发言, 内容是什么不重要

1.2.2 将前面获得的token替换掉这个url中的XYZ部分,
`https://api.telegram.org/botXYZ/getUpdates`

它实际上应该看上去应该是这样的:
`https://api.telegram.org/bot987654321:FEDCBA_dfoiuweSWEczgxT7-l4r9Y/getUpdates`
然后访问这个url, 在返回的json中你很容易可以找到**id**这个键(与username,first_name)相近. 那串数字(139000174)就是要找的id

1.2.3 好了, 有了token和 chat_id 就可以从`bot`往`自己`发消息了, 试一试:

```
curl -k --data chat_id="139000174" --data "text=Have a good journey, Mr. Weyland." "https://api.telegram.org/bot987654321:FEDCBA_dfoiuweSWEczgxT7-l4r9Y/sendMessage"
```
此时客户端应立即收到来自David的消息: "Have a good journey, Mr. Weyland."  哈哈哈.


2, 实现推送的脚本代码
好了, 基本所需变量已经准备就绪, 测试也通了, 接下来需要好用的脚本使得跑起来更顺手. github上有很多不同语言实现的telegram bot, 这里我选用了[pyTelegramBotAPI](https://github.com/eternnoir/pyTelegramBotAPI)

2.1 安装 `pip install pyTelegramBotAPI`

2.2 使用.这里我们因为是给自己推消息,所以可以略去他家 readme 中的大部分内容,直奔api而去:

```
import telebot

bot = telebot.TeleBot("YOUR TOKEN")

bot.send_message("YOUR CHAT_ID", text)

```

就是这么简单了!
最后,应参考[telegram bot 的官方 api](https://core.telegram.org/bots/api), 看看是否用得上多媒体/图片/声音/HTML/markdown 等参数, 按照自己实际需求定义个函数包裹一下.
需要提醒的是, 如果发送html格式的话, 能用的tag有限, 而且不能嵌套tag, 还是参考api 文档吧!
事情就是这么简单,且又好使. 微信那边要实现类似功能,得用公司资质申请个啥服务号, 每年300元, 不便宜又有风险.
Telegram 结合channels，group，bot，构成了一个极其多样化并且提供强大能力的平台，除开被墙这个因素，很值得自媒体从业人员和开发者投入精力。