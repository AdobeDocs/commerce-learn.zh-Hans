---
title: 创建要在API Mesh中使用的GraphQL单个源请求
description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有一个源的请求。
landing-page-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有一个源的请求。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# 创建单源GraphQL API网格

该视频可帮助开发人员了解如何创建GraphQL反向代理，并且只有一个源。 请记住，要使GraphQL Mesh正常工作，需要具有有效GraphQL架构的公开访问URL。 此视频还说明了如何设置初始json以用于您的商务网站。 有关视频中使用的基本代码示例，请访问 [创建网格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 这个视频给谁？

* 任何刚接触API网格的人
* 有兴趣使用多个图形源的开发人员
* 需要知道如何过滤网络选项卡和按graphql过滤的任何人

## 视频内容

* 将API用作反向代理
* 使用Adobe Developer命令行界面进行JSON配置
* 访问新创建的GraphQL端点

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## 创建json配置文件

为了让Adobe应用程序生成器了解您的所有源，您可以在JSON配置中定义它们。 每个源都是数组中的一个元素，您可以有一个或多个。 以下是单个源的示例

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
