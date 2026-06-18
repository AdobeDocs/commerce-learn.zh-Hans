---
title: 在API网格中创建GraphQL单一源网格
description: 了解如何在Adobe Commerce和Adobe App Builder上使用API Mesh。 了解如何使用单个GraphQL源创建网格并访问新端点。
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 创建具有单个源的网格

此视频可帮助开发人员了解如何在Adobe Developer App Builder的API Mesh中使用单个源创建网格。 要使此基本示例正常工作，您需要可公开访问的API或GraphQL端点。 此视频还介绍如何创建一个简单的`mesh.json`文件以用于您的Commerce实例。 有关更多详细信息和代码示例，请访问[创建网格](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}。

## 此视频面向谁？

* 任何不熟悉API mesh的人
* 对合并多个GraphQL和API源感兴趣的开发人员
* 需要了解如何按GraphQL筛选“网络”选项卡和过滤的人员

## 视频内容

* 使用API网格作为反向代理
* 从JSON配置文件创建网格
* 访问新创建的GraphQL端点

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## 创建JSON配置文件

API网格使用JSON配置文件来定义源处理程序。 JSON文件包含一个`sources`数组，其中包含网格的源。 下面是一个具有单一源的网格示例。

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
