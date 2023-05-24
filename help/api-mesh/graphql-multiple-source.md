---
title: 创建要在API网格中使用的多源GraphQL
description: 了解如何在Adobe Commerce上为API Mesh使用多个源 [!DNL Adobe App Builder]. 了解一些常见错误以及如何解决它们。
landing-page-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有多个源的网格，以及如何解决一些常见错误。
short-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有多个源的网格，以及如何解决一些常见错误。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 创建具有多个源的网格

此视频可帮助开发人员了解如何在Adobe Developer App Builder的API Mesh中使用多个源创建网格。 本视频说明如何创建具有多个源的网格并识别错误。 有关更多详细信息和代码示例，请访问 [创建网格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 此视频面向谁？

* 任何不熟悉API Mesh的人
* 对合并多个API和GraphQL源感兴趣的开发人员

## 视频内容

* 使用方法 [转换](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} 修改默认源架构
* 如何解决名称冲突、架构可用性和其他架构语法问题等错误
* 使用修改后的配置更新网格

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## 创建json配置文件

API网格使用JSON配置文件来定义源处理程序。 JSON文件包含 `sources` 包含网格源的数组。 以下是具有多个源的网格示例。

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
        "name": "Example",
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
