---
title: 创建要在API Mesh中使用的多源GraphQL
description: 了解如何在Adobe Commerce和 [!DNL Adobe App Builder]上为API Mesh使用多个源。 了解一些常见错误以及如何解决它们。
jira: KT-21677
doc-type: Tutorial
duration: 381
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
TQID: https://experienceleague.adobe.com/O6ONn4NzMP-VqN0nsCoD-OPkZGMBelLWB-KNP1fZqmA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: 198
ht-degree: 0%

---

# 创建具有多个源的网格

此视频可帮助开发人员了解如何在Adobe Developer App Builder的API Mesh中使用多个源创建网格。 本视频说明如何创建具有多个源的网格并识别错误。 有关更多详细信息和代码示例，请访问[创建网格](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}。

## 此视频面向谁？

* 不熟悉API mesh的任何人
* 对合并多个API和GraphQL源感兴趣的开发人员

## 视频内容

* 如何使用[转换](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/transforms/){target="_blank"}来修改默认源架构
* 如何排除错误，如名称冲突、架构可用性和其他架构语法问题
* 使用修改后的配置更新网格

>[!VIDEO](https://video.tv.adobe.com/v/3414125?learn=on)

## 创建JSON配置文件

API网格使用JSON配置文件来定义源处理程序。 JSON文件包含一个`sources`数组，其中包含网格的源。 以下是具有多个源的网格示例。

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
