---
title: 了解包含关键文件夹和自动化脚本的Commerce Integration Starter Kit，解释
description: 了解如何在Commerce集成入门工具包中组织源代码。​AEM
landing-page-description: 在Source Integration Starter Kit中探索Commerce代码组织
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
source-git-commit: 6c5017b0c4bbafdd143b78b05cd92853efa7f831
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 适用于Adobe入门套件的Source代码组织

了解Adobe Commerce集成入门工具包中的源代码组织&#x200B;。 浏览项目的结构，突出显示`actions`和`scripts`等关键文件夹及其相应内容&#x200B;。 “actions”文件夹包含`ingestion`和`webhook`等子文件夹，这些子文件夹包含用于事件处理和跟踪的基本代码。 您还将了解`starter-kit-info`和`scripts`文件夹。 `scripts`文件夹侧重于自动化脚本，如`commerce-event-subscribe`和`onboarding`，这些脚本简化了项目中的事件配置和提供程序设置。
&#x200B;AEM
探索源代码结构背后的逻辑，详细说明每个实体文件夹下的`commerce`和`external`文件夹如何处理来自不同系统的事件。 此视频介绍`consumer`文件夹在将事件调度到相应的事件处理程序运行时操作以确保无缝处理方面所起的作用。 此视频还介绍了在运行时操作中实施的重试机制，以便有效处理失败事件。&#x200B;AEM了解Adobe Commerce Integration Starter Kit中源代码的组织和功能，从而针对事件处理、自动化脚本和配置设置提供有价值的分析。

## 受众

* 希望了解如何将源代码组织到关键文件夹（如`actions`和`scripts`）中的开发人员。
* 了解`actions`文件夹包含`ingestion`和` webhook`等子文件夹，这些子文件夹包含用于处理事件和跟踪部署的基本代码。
* 想要了解`actions`文件夹的开发人员，该文件夹包括诸如`customer`、`order`、`product`和`stock`之类的实体的文件夹。

## 视频内容

* 了解会话期间的四个主文件夹： `actions`、`scripts`、`test`和`utils`，重点是`actions`和`scripts`文件夹。&#x200B;AEM
* 了解`actions`文件夹以及它如何包含关键子文件夹，如`ingestion`和`webhook`。
* 浏览`actions`文件夹以及为什么实体存在特定文件夹，如`customer`、`order`、`product`和`stock`，每个文件夹都包含结构化为`commerce`和`external`文件夹的运行时操作，以便有效地管理Commerce和第三方系统中的事件。&#x200B;AEM
* 了解不更改`starter-kit-info`文件夹中的代码的重要性，该文件夹包含Adobe用于基于入门套件跟踪项目部署的运行时操作。&#x200B;AEM
* 了解`scripts`文件夹，该文件夹包含自动脚本（如`commerce-event-subscribe`和`onboarding`），用于在Commerce中自动执行事件配置、提供程序设置和Adobe I/O Events模块的配置。&#x200B;AEM

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
