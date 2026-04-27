---
title: 配置、部署和自定义摄取Webhook
description: 了解如何设置和自定义引入webhook，以便于Commerce和第三方后台系统之间的通信。
landing-page-description: 了解如何使用Commerce Integration Starter Kit通过引入webhook将Commerce与第三方后台系统集成。
kt: 15870
doc-type: video
duration: 697
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 432
ht-degree: 0%

---

# 配置、部署和自定义摄取webhook

了解用于将Commerce与第三方后台系统集成的引入webhook的设置和自定义&#x200B;。此视频介绍webhook如何通过提供可公开使用的端点来使来自第三方系统的消息适应Adobe IO事件API，从而解决系统之间的事件通信限制。 此过程包括在`actions.config.yaml`文件中配置webhook，在`app.config.yaml`文件中启用它，并将其部署以确保正常运行。

此视频介绍了修改webhook代码的步骤，以便将第三方事件转换为与集成的订阅事件类型兼容的格式。 此视频讨论了添加`event-mapping.json`文件以进行此转换，并强调了进行更改后重新部署运行时操作的重要性。&#x200B;此视频还强调了验证和转换传入事件有效负载以符合预期架构的重要性，从而确保成功处理并与Commerce API集成以创建客户。

## 受众

* 希望设置引入webhook的开发人员
* 任何想要自定义用于事件翻译的代码的人
* 希望了解身份验证和有效负载管理重要性的开发人员和架构师

## 视频内容

* 配置和部署：视频强调在`actions.config.yaml`文件中配置引入webhook并在`app.config.yaml`文件中启用它的重要性。 它还强调了在进行更改后重新部署项目的需要，以确保webhook正常运行。
* 兼容性自定义：自定义webhook代码以将第三方事件转换为与集成的订阅事件类型一致的格式至关重要。 这&#x200B;种自定义方式可确保系统之间的无缝通信以及成功的事件处理。
* 身份验证实施：企业负责实施符合其需求的身份验证机制，以防止在使用引入webhook时发出未经授权的请求。 此步骤对于维护集成的安全性和完整性至关重要。
* 有效负载验证和转换：验证和转换传入事件有效负载以匹配预期架构对于成功处理和与Commerce API集成至关重要。 通过适当地&#x200B;裁剪和映射字段，集成可以使用所需的数据高效地运行。

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 代码示例

* [自定义摄取webhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [添加摄取计划程序](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
