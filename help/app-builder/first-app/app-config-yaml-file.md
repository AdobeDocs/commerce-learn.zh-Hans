---
title: app.config.yaml文件
description: 了解此示例应用程序的app.config.yaml文件中的文件类型。
landing-page-description: 了解与Adobe Commerce一起使用的Adobe Developer App Builder以及app.config.yaml中会显示哪些类型的文件。
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: 037a7571c87f328dde0f39dd830c7379bd2230b6
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---


# 的 `app.config.yaml` 文件 {#app-config-yaml}

此文件确定应用程序的配置。

## 这个视频给谁？

* 初次接触Adobe Commerce、在Adobe应用程序生成器方面经验有限的开发人员，他们正在了解 `app.config.yaml` 中。

## 视频内容

* 的 `app.config.yaml` 讨论的文件
* 定义如何链接到其他 `.js` 文件

>[!VIDEO](https://video.tv.adobe.com/v/3416592)

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

您可以在文件的示例模块中看到这些静态值 `actions/commerce.index.js`

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
