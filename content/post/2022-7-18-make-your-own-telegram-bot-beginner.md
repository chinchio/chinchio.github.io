---
title:       "開始學習如何寫一個telegram bot(誤"
subtitle:    "簡單telegram api使用"
description: "學習使用python-telegram-bot來接收來自user的msg及發送msg"
date:        2022-07-18T17:56:08+08:00
author:      "chinchio"
image:       "https://images.unsplash.com/photo-1521931961826-fe48677230a5?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
tags:        ["telegram", "bot"]
categories:  ["tech"]
---

# 引言
在用`tg`經常都可以看到每個channel多多少少都用bot來避免其他人轉發ads或者監控其他人的活躍程度，也能做發post, 自己羣裏面ads的管理，所以想借由終於把blog建立起來的決心來學一下如何寫tg bot，如果我恆心足夠應該會有一個demo出現在這裏吧（？

# Create new bot
要建立telegram bot，首先先跟`[@botfather](https://t.me/botfather)`打好關係

![create new with @botfather](https://i.imgur.com/j1LGm8A.png)

這次我是使用`v13.x`版本的`python-telegram-bot`所以就跟隨

[python-telegram-bot v13.x wiki| introduce api](https://github.com/python-telegram-bot/v13.x-wiki/wiki/Introduction-to-the-API)

> Bots can't initiate conversations with users. A user must either add them to a group or send them a message first.

Wiki寫得很清楚， telegram bot 是沒有辦法主動跟user互動。

# python-telegram-bot 應用

## 取得對話內容

1. 首先，要把`telegram.Bot`這個object init

```python
import telegram
bot = telegram.Bot(token='<BOT_TOKEN>')
```

2. 按著, 來`update`一下，再取最後一個對話msg
```python
updates = bot.get_updates() #先做一下update取得跟bot對話的內容
print(updates[-1]) #這裏跟wiki寫的不一樣，因為我要取得跟後一次跟bot的對話內容
```

3. 就會print 像下面的內容
```json
{
    "update_id":7245133,
    "message":{
        "supergroup_chat_created":false,
        "caption_entities":[
            
        ],
        "chat":{
            "id":5512443232326375,
            "first_name":"hoho",
            "type":"private"
        },
        "text":"test",
        "channel_chat_created":false,
        "group_chat_created":false,
        "new_chat_photo":[
            
        ],
        "entities":[
            
        ],
        "date":1658136743,
        "photo":[
            
        ],
        "message_id":2,
        "new_chat_members":[
            
        ],
        "delete_chat_photo":false,
        "from":{
            "id":5512443232326375,
            "first_name":"hoho",
            "language_code":"en",
            "is_bot":false
        }
    }
}
```

## 發出訊息
```python
bot.send_message(text='Hi!', chat_id=5512443232326375) #假的id 不然呢？
```

最後結果就像下圖

![telegram bot send msg example](https://i.imgur.com/MuqYRZK.png)


# 結論
我很早就已經想去學如何寫一個telegram bot, 但是一直都沒有恆心來學，這次借由寫blog來為自己帶來一點動力。

希望啦！

# thanks
Cover photos from [Christian Wiediger](https://unsplash.com/photos/GWkioAj5aB4)