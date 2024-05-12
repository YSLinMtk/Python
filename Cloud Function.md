---
title: 'Project documentation template'
disqus: hackmd
---

How to build a GCP Cloud Fuction
===
<!-- ![downloads](https://img.shields.io/github/downloads/atom/atom/total.svg)
![build](https://img.shields.io/appveyor/ci/:user/:repo.svg)
![chat](https://img.shields.io/discord/:serverId.svg) -->

<!-- ## Table of Contents -->

[TOC]

## Beginners Guide

How to start it?

1. Prepare Your Development Environment
2. Write Your Cloud Function
3. Deploy Your Cloud Function
4. Test Your Cloud Function



GCP Account
---
[Start with GCP](https://cloud.google.com/free/?utm_source=google&utm_medium=cpc&utm_campaign=japac-TW-all-en-dr-BKWS-all-core-trial-EXA-dr-1605216&utm_content=text-ad-none-none-DEV_c-CRE_644095273669-ADGP_Hybrid+%7C+BKWS+-+EXA+%7C+Txt+-GCP-General-core+brand-main-KWID_43700074766895907-kwd-87853815&userloc_1012825-network_g&utm_term=KW_gcp&gad_source=1&gclid=EAIaIQobChMI9dXbpbuHhgMVM-gWBR3BiQEkEAAYASAAEgJPS_D_BwE&gclsrc=aw.ds)

新帳號有90天試用期，以及一定額度的抵免額，你各位用起來吧！！
[免費用量限制](https://cloud.google.com/free/docs/free-cloud-features?_gl=1*11a09g4*_up*MQ..&gclid=CjwKCAjwrvyxBhAbEiwAEg_KgjxFB2AEalV8ZCwHZ0PJA3N-8lYvFt_6DDwUrTZcX68g-Zu1qWcGURoCG_oQAvD_BwE&gclsrc=aw.ds)


Write a Cloud Function
---
### Requirements
Python >= 3.9


##### main.py
```gherkin=Python
import functions_framework

@functions_framework.http
def hello_http(request):
    """HTTP Cloud Function.
    Args:
        request (flask.Request): The request object.
        <https://flask.palletsprojects.com/en/1.1.x/api/#incoming-request-data>
    Returns:
        The response text, or any set of values that can be turned into a
        Response object using `make_response`
        <https://flask.palletsprojects.com/en/1.1.x/api/#flask.make_response>.
    """
    request_json = request.get_json(silent=True)
    request_args = request.args

    if request_json and 'name' in request_json:
        name = request_json['name']
    elif request_args and 'name' in request_args:
        name = request_args['name']
    else:
        name = 'World'
    return 'Hello {}!'.format(name)

```



Deploy Your Cloud Function
---
### 建立函式
#### Setting
![Cloud Functions Settings](https://hackmd.io/_uploads/ryfd41RzR.png)
* 選擇 第1代/第2代 都可以，差異說明如下：
    > Cloud Functions offers two product versions: Cloud Functions (1st gen), the original version, and Cloud Functions (2nd gen), a new version built on Cloud Run and Eventarc to provide an enhanced feature set. This page describes new features introduced in Cloud Functions (2nd gen) and provides a comparison between the two product versions.
    >
    > We recommend that you choose Cloud Functions (2nd gen) for new functions wherever possible. However, we plan to continue supporting Cloud Functions (1st gen).
* ***地區*** 慎選，跨洲呼叫的費用可是不便宜的
* URL可當作Line Bot Webhook使用
* "驗證" 若是要作為Line Bot Webhook使用，這邊需勾第一點


![Cloud Functions Settings](https://hackmd.io/_uploads/r13x81RzR.png)
* 執行階段環境變數：程式碼中若有os.getenv('YOUR_KEY', None)，則須於此新增變數

#### Next Step
選擇執行階段 (用哪種語言)，確認程式碼無誤即可部署
![python](https://hackmd.io/_uploads/SkKrdy0G0.png)



###### tags: `Cloud Function` `GCP` `Python`
