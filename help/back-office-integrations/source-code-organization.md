---
title: Commerce入门工具包中的Source代码组织
description: 了解Commerce集成入门套件中的源代码组织，包括关键文件夹，如操作和脚本、自动化脚本和事件处理。
doc-type: Technical Video
duration: 534
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15868
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
TQID: https://experienceleague.adobe.com/P6-sK18TcpC91YXJcXohIvzmii3N66ZKh3nZha-RYQY
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 252
ht-degree: 0%

---

# 适用于Adobe Starter Kit的Source代码组织

了解Adobe Commerce Integration Starter Kit中的源代码组织。&#x200B;浏览项目的结构，突出显示关键文件夹（如`actions`和`scripts`）及其相应内容。&#x200B;“actions”文件夹包含子文件夹（如`ingestion`和`webhook`），这些子文件夹包含用于事件处理和跟踪的基本代码。您还将了解`starter-kit-info`和`scripts`文件夹。`scripts`文件夹侧重于自动化脚本，如`commerce-event-subscribe`和`onboarding`，这些脚本可简化项目中的事件配置和提供程序设置。
&#x200B;AEM
探索源代码结构背后的逻辑，详细描述每个实体文件夹下的`commerce`和`external`文件夹如何处理来自不同系统的事件。此视频介绍`consumer`文件夹在将事件调度到相应的事件处理程序运行时操作以确保无缝处理方面所起的作用。此视频还介绍了在运行时操作中实施的重试机制，以便有效处理失败事件。了&#x200B;解Adobe Commerce集成入门工具包中源代码的组织和功能，提供了有关事件处理、自动化脚本和配置设置的宝贵见解。

## 受众

* 希望了解如何将源代码组织到关键文件夹（如`actions`和`scripts`）中的开发人员。
* 了解`actions`文件夹包含`ingestion`和` webhook`等子文件夹，这些子文件夹包含用于处理事件和跟踪部署的基本代码。
* 想要了解`actions`文件夹的开发人员，该文件夹包括诸如`customer`、`order`、`product`和`stock`之类的实体的文件夹。

## 视频内容

* 了解会话期间的四个主文件夹： `actions`、`scripts`、`test`和`utils`，重点是`actions`和`scripts`文件夹。&#x200B;AEM
* 了解`actions`文件夹以及它如何包含关键子文件夹，如`ingestion`和`webhook`。
* 浏览`actions`文件夹以及为什么实体存在特定文件夹，如`customer`、`order`、`product`和`stock`，每个文件夹都包含结构化为`commerce`和`external`文件夹的运行时操作，以便有效地管理Commerce和第三方系统中的事件。&#x200B;AEM
* 了解不更改`starter-kit-info`文件夹中的代码的重要性，该文件夹包含Adobe用于基于入门工具包跟踪项目部署的运行时操作。&#x200B;AEM
* 了解`scripts`文件夹，该文件夹包含自动脚本（如`commerce-event-subscribe`和`onboarding`），用于在Commerce中自动执行事件配置、提供程序设置和Adobe I/O Events模块的配置。&#x200B;AEM

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
