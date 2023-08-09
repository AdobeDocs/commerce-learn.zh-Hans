---
title: app.config.yaml文件
description: 了解此示例应用程序的app.config.yaml文件中的文件类型。
landing-page-description: 了解与Adobe Developer一起使用的Adobe Commerce App Builder以及app.config.yaml中会包含哪些类型的文件。
kt: 12929
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
source-git-commit: 01eb2abc854e7de4b3bbca9c0cd4d09ec43f9bf2
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# app.config.yaml文件的描述和用法 {#app-config-yaml}

此文件确定应用程序的配置。

## 此视频面向谁？

* 刚开始接触Adobe Commerce但对AdobeApp Builder经验有限的开发人员，他们正了解 `app.config.yaml` 在示例应用程序中。

## 视频内容

* 此 `app.config.yaml` 讨论的文件
* 定义如何链接到其他 `.js` 文件

>[!VIDEO](https://video.tv.adobe.com/v/3416592?quality=12&learn=on)

## 代码示例

```bash
# Specify your secrets here
# This file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can also include commerce OAuth credentials, our sample module will use the following example credentials:
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

您可以在文件中看到示例模块中使用的这些静态值 `actions/commerce.index.js`

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
