---
title: Yaml 文件
description: 了解应用程序此示例的 app.config 文件中的文件类型。
landing-page-description: 了解与 Adobe Systems 商务一起使用的 Adobe Systems 开发人员应用程序生成器以及应用程序 yaml 中的文件类型。
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# Yaml 文件的描述和使用情况 {#app-config-yaml}

此文件可确定应用程序的配置。

## 此视频是谁？

* 开发人员 Adobe Systems 商务的新功能，具有有限的体验，Adobe Systems 应用程序生成器 `app.config.yaml` ，以了解示例应用程序。

## 视频内容

* 讨论的 `app.config.yaml` 文件
* 如何将定义关联其他 `.js` 文件

>[!VIDEO](https://video.tv.adobe.com/v/3416592?quality=12&learn=on)

## Code 示例

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

您可以查看文件中示例模块使用的静态值 `actions/commerce.index.js`

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
