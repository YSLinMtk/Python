---
title: 'Line Bot'
disqus: hackmd
---

Start with Line Bot
===

[TOC]


Create an Account
---
前往 https://developers.line.biz/console/

### Create a new provider
名稱可隨意取
![Create a new provider](https://hackmd.io/_uploads/r1XvA6TMA.png)

### Create a new Messaging API channel
Step by step填寫，Channel name & icon會顯示於Line官方帳號
![Create a new Messaging API channel](https://hackmd.io/_uploads/r1fbyRpfA.png)

### Get Channel secret (串接SDK API時會用到)
Basic setting > Channel secret 
![Get Channel secret](https://hackmd.io/_uploads/SJGDf06zA.png)


### Get Channel access token (串接SDK API時會用到)
Issue點擊就可以看到token
![Get Channel access token](https://hackmd.io/_uploads/Byap-RpfA.png)


Developed with Python.
---

```gherkin=Python
import os
import json
import sys
import functions_framework
from linebot import LineBotApi, WebhookHandler
from linebot.exceptions import InvalidSignatureError
from linebot.models import MessageEvent, TextMessage, TextSendMessage

@functions_framework.http
def linebot_msg(request):
    try:
        secret = os.getenv('LINE_CHANNEL_SECRET', None)
        access_token = os.getenv('LINE_CHANNEL_ACCESS_TOKEN', None)
        if secret is None:
            print('Specify LINE_CHANNEL_SECRET as environment variable.')
            sys.exit(1)
        if access_token is None:
            print('Specify LINE_CHANNEL_ACCESS_TOKEN as environment variable.')
            sys.exit(1)
        body = request.get_data(as_text=True)
        json_data = json.loads(body)
        line_bot_api = LineBotApi(access_token)
        handler = WebhookHandler(secret)
        signature = request.headers['X-Line-Signature']
        handler.handle(body, signature)

        event = json_data['events'][0]
        tk = event['replyToken']
        user_id = event['source']['userId']
        msg_type = event['message']['type']
        
        # text ( 文字 )
        if msg_type == 'text':
            msg = event['message']['text']
            line_bot_api.reply_message(tk, TextSendMessage(text=msg))
        
        else:
            reply_msg = TextSendMessage(text='我看不懂你的意思，可以再說一次嗎？')
            line_bot_api.reply_message(tk, reply_msg)
    except:
        print(request.args)
    return 'OK'
```

> Read more about Line bot here: https://github.com/YSLinMtk/line-bot-sdk-python

Messaging API settings
---
### Set Webhook URL
#### Messaging API > Webhook settings
有很多方法可以建制Webhook，對我來說最簡單的是將上方程式碼部署成GCP Cloud Function，部署完成後再將URL填入即完成
![Webhook](https://hackmd.io/_uploads/r1_p8CpfA.png)


### LINE Official Account features
* Auto-reply messages -> Disabled
* Greeting messages -> Disabled
點擊右方 Edit 進入設定關閉
![Auto Setting Off](https://hackmd.io/_uploads/BJcBORaMC.png)


> Read more about GCP Cloud Function here: https://hackmd.io/@LeftCat/GCP_Cloud_function

###### tags: `LineBot` `Python`