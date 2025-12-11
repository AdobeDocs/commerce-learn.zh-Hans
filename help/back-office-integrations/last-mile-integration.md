---
title: Commerce集成入门工具包中的最后一英里集成。
description: Commerce中的“最后一公里”集成，突出显示验证、转换、预处理、发送和后处理等可扩展性挂钩​。
landing-page-description: 了解Commerce系统最后一英里集成中可扩展挂接的结构和功能。
kt: 15869
doc-type: video
duration: 465
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# 使用Adobe Starter Kit的最后一英里集成

了解在开始与Adobe Commerce进行最后一英里集成时要考虑的项目，重点放在使用可扩展性挂钩以增强与第三方系统的连接。 此视频概述了结构化方法，其中各种挂接（如验证、转换、预处理、发送和后处理）可确保无缝的数据流和系统同步。 每个挂接都有不同的用途，包括：

* 根据架构验证传入数据
* 在系统之间转换数据对象
* 在发送相关信息之前执行计算
* 将数据发送到目标系统

请务必为每个块维护单独的JavaScript文件，以维护业务逻辑完整性和便于将来升级框架，从而确保强大且适应性强的集成设置。

通过后处理挂接，了解后处理活动的重要性，该挂接使用户能够在数据同步后执行其他操作，例如向订单添加注释或存储外部ID。 此视频包括最佳实践，如将API请求封装在特定库中，以简化与第三方系统的连接。 您还将了解每个挂接的典型用例以及有关处理不同场景的指导。

## 受众

* 希望了解可扩展性挂接的结构和功能的开发人员，以及这些挂接如何增强与第三方系统的连接。
* 开发人员希望了解与每个可扩展性挂接相关的典型用例和最佳实践（如验证、转换、预处理、发送和后处理），以促进无缝数据流、系统同步和高效的集成设置维护。&#x200B;AEM

## 视频内容

* 了解最后一英里集成中调用操作的结构。
* 了解验证挂接中的典型用例，包括根据架构验证传入数据以及根据特定条件跳过特定事件。&#x200B;AEM
* 了解转换挂接在原始系统和目标系统之间转换数据对象方面的作用。
* 了解发送挂接对于促进实际数据发送到目标系统的重要性。

>[!VIDEO](https://video.tv.adobe.com/v/3451938?captions=chi_hans&learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
