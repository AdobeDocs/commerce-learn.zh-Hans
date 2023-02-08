---
title: 创建要在API Mesh中使用的多个源GraphQL
description: 了解如何在Adobe Commerce上为API网格使用多个源，以及 [!DNL Adobe App Builder]. 了解一些常见错误以及如何解决它们。
landing-page-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有多个源的请求以及如何解决一些常见错误。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 创建多个源GraphQL API网格

该视频可帮助开发人员了解如何使用多个源创建GraphQL反向代理。 此视频演示如何拼合不同的源、识别错误并将更改保存到git。 有关视频中使用的基本代码示例，请访问 [创建网格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 这个视频给谁？

* 任何刚接触API网格的人
* 有兴趣使用多个图形源的开发人员
* 需要知道如何过滤网络选项卡和按graphql过滤的任何人

## 视频内容

* 如何从第二个源中复杂的自定义属性API架构可以覆盖默认的源架构
* 修改api网格配置以考虑第二个覆盖架构
* 如何排除在命名冲突、模式可用性和其他SDL语法等过程中可能出现的错误
* 尝试拼合架构后出现的常见错误示例
* 编辑后重建api网格
* 在修改API网格配置后将更改保存到git

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## 创建json配置文件

为了让Adobe应用程序生成器了解您的所有源，您可以在JSON配置中定义它们。 每个源都是数组中的一个元素，您可以有一个或多个。 以下是多个源请求的示例，这些源请求相互啮合以形成单个响应。

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
      },
      {
        "name": "ERP",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
