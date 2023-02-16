---
title: 创建要在API Mesh中使用的多个源GraphQL
description: 了解如何在Adobe Commerce上为API网格使用多个源，以及 [!DNL Adobe App Builder]. 了解一些常见错误以及如何解决它们。
landing-page-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何创建具有多个源的网格以及如何解决一些常见错误。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b3d5b22a597b342df6bf9846346d656dd4ce1383
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# 使用多个源创建网格

此视频可帮助开发人员了解如何在Adobe Developer App Builder的API Mesh中使用多个源创建网格。 此视频演示了如何使用多个源创建网格并识别错误。 有关更多详细信息和代码示例，请访问 [创建网格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 这个视频给谁？

* 任何刚接触API网格的人
* 有兴趣组合多个API和GraphQL源的开发人员

## 视频内容

* 使用方法 [转换](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/) 修改默认源架构
* 如何解决错误，如名称冲突、架构可用性和其他架构语法问题
* 使用修改的配置更新网格

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## 创建json配置文件

API Mesh使用JSON配置文件来定义源处理程序。 JSON文件包含 `sources` 包含网格源的数组。 以下是多源网格的示例。

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
