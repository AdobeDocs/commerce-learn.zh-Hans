---
title: .env文件
description: 了解此示例应用程序的.env文件中的文件类型
landing-page-description: 了解与Adobe Commerce一起使用的Adobe Developer App Builder以及.env文件中使用的内容类型
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: 037a7571c87f328dde0f39dd830c7379bd2230b6
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# 的 `.env` 文件 {#env-file}

的 `.env` 是一个特殊文件，它不是示例模块的一部分，但对于在Adobe Developer App Builder应用程序中使用很重要。 此文件包含机密和其他信息。 避免将此文件提交到任何代码存储库。

## 这个视频给谁？

* 初次接触Adobe Commerce的开发人员，在使用Adobe应用程序生成器方面具有有限的经验，他们希望了解 `.env` 文件。

## 视频内容

* .env文件简介及其用途
* 如何生成.env文件
* 如何附加文件以添加新密钥
* 避免提交此文件，因为它包含敏感信息

>[!VIDEO](https://video.tv.adobe.com/v/3416593)

## 代码示例

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

您可以在文件的示例模块中看到这些静态值 `actions/commerce.index.js`.

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
