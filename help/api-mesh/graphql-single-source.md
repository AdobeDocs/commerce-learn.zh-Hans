---
title: 在API网格中创建GraphQL单源网格
description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有一个源的网格。
landing-page-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有一个源的网格。
short-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有一个源的网格。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# 使用单个源创建网格

此视频可帮助开发人员了解如何在适用于Adobe Developer App Builder的API Mesh中使用单个源创建网格。 要使此基本示例按预期工作，您需要一个可公开访问的API或GraphQL端点。 此视频还介绍如何创建简单的 `mesh.json` 文件来与您的商务实例一起使用。 有关更多详细信息和代码示例，请访问 [创建网格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 这个视频给谁？

* 任何刚接触API网格的人
* 有兴趣组合多个GraphQL和API源的开发人员
* 需要了解如何过滤网络选项卡和按GraphQL过滤的人员

## 视频内容

* 将API Mesh用作反向代理
* 从JSON配置文件创建网格
* 访问新创建的GraphQL端点

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## 创建json配置文件

API Mesh使用JSON配置文件来定义源处理程序。 JSON文件包含 `sources` 包含网格源的数组。 以下是单个源的网格示例。

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
