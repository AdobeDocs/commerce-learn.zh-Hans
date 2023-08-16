---
title: 在API网格中创建GraphQL单一源网格
description: 了解如何在 Adobe Commerce 和  [!DNL Adobe App Builder] 上使用 API Mesh。了解如何创建具有一个源的网格。
landing-page-description: 了解如何在 Adobe Commerce 和  [!DNL Adobe App Builder] 上使用 API Mesh。了解如何创建具有一个源的网格。
short-description: 了解如何在 Adobe Commerce 和  [!DNL Adobe App Builder] 上使用 API Mesh。了解如何创建具有一个源的网格。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 12%

---

# 创建具有单个源的网格

此视频可帮助开发人员了解如何在Adobe Developer App Builder的API Mesh中通过单个源创建网格。 要使此基本示例按预期工作，您需要可公开访问的API或GraphQL端点。 此视频还介绍如何创建简单的 `mesh.json` 与您的Commerce实例一起使用的文件。 有关更多详细信息和代码示例，请访问 [创建网格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 此视频面向谁？

* 任何不熟悉API mesh的人
* 对合并多个GraphQL和API源感兴趣的开发人员
* 需要了解如何按GraphQL筛选“网络”选项卡和过滤的人员

## 视频内容

* 使用API网格作为反向代理
* 从JSON配置文件创建网格
* 访问新创建的GraphQL端点

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## 创建json配置文件

API网格使用JSON配置文件来定义源处理程序。 JSON文件包含 `sources` 包含网格源的数组。 以下是带有单个源的网格示例。

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
