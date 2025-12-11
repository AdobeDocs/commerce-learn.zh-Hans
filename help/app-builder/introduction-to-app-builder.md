---
title: Adobe Commerce的进程外可扩展性
description: 了解什么是 Adobe App Builder 以及为什么它是进程外可扩展性的一个重要方面。
landing-page-description: 了解什么是 App Builder 以及它如何帮助制定 Adobe Commerce 开发策略。
short-description: 了解什么是 App Builder 以及它如何帮助制定 Adobe Commerce 开发策略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 6%

---

# App Builder简介

从历史上看，Adobe Commerce开发一直采用进程内可扩展性。 进程内模型要求任何新代码与升级、服务器的PHP版本以及Commerce使用的许多其他基本服务器应用程序和服务兼容。 Adobe Developer App Builder使用进程外可扩展性以避免这些兼容性问题。

## 适用于Adobe Commerce的App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder是一个用于集成和创建自定义体验以扩展Adobe解决方案的无服务器可扩展性平台，现在可用于Adobe Commerce。 借助App Builder，您可以构建安全且可扩展的应用程序，以扩展Commerce原生功能并与第三方解决方案集成。 作为开发人员，您现在可以利用Adobe Commerce进程外的可扩展性，这反过来又会立即带来长期好处。

App Builder为集成和创建扩展[!DNL Adobe Commerce]的自定义应用程序提供了统一的第三方扩展框架。 由于此可扩展性框架是基于Adobe的基础架构构建的，因此开发人员可以构建自定义微服务，并跨其他Adobe解决方案和第三方集成扩展和集成[!DNL Adobe Commerce]。

App Builder为客户提供了在各种用例中扩展[!DNL Adobe Commerce]的方法：

* 中间件可扩展性 — 通过构建自定义连接器或利用预建的集成套件，将外部系统与Adobe应用程序相连接。
* 核心服务可扩展性 — 通过使用自定义功能和业务逻辑扩展默认行为，扩展核心应用程序功能。
* 用户体验可扩展性 — 扩展核心体验以支持业务需求或构建特定于客户的数字资产、店面和后台应用程序。

Adobe Developer App Builder是一个基于云的解决方案，这意味着它会自动扩展。 此服务也是全球分布的，无论您位于何处，都能提供最佳性能。

## 为何要进一步了解App Builder

由于Adobe Commerce不是完全的SAAS产品，因此您开发的代码可能会增加复杂性和升级问题。 通过使用进程外可扩展性(如App Builder)，您可以为Adobe Commerce存储提供自定义的独特功能，而无需进程内方法。

其他优势包括：

* 分离的功能可加快启动时间。
* 升级现在更轻松了。 自定义功能位于Commerce代码库之外，这可以防止升级时出现兼容性问题。
* 将功能和逻辑移出Commerce可释放通常由进程内开发方法使用的资源。

## 架构 {#architecture}

Adobe Developer App Builder提供了一个通用、一致和标准化的开发平台，用于扩展Adobe Cloud解决方案(例如Adobe Commerce)，而不是开箱即用的解决方案，该平台包括：

* Adobe Developer Console用于自定义微服务和扩展开发。 在访问创建插件和集成所需的所有工具和API时，构建和管理项目。
* 开源工具、SDK和库来构建自定义扩展和集成。 使用React Spectrum(Adobe的UI工具包)为所有Adobe应用程序具有一个通用UI。
* 一些服务，如用于在Adobe的无服务器平台上托管基础架构的I/O运行时，以及用于基于事件的集成的I/O事件。 Adobe还为存储数据和文件提供开箱即用支持。
* Adobe Experience Cloud，您可在此处提交扩展和集成以在Experience Cloud组织中发布。系统管理员可以审核、管理和批准这些扩展。 发布后，您的自定义App Builder扩展和工具将与其他Adobe Experience Cloud应用程序一起提供。

下图说明了在App Builder上构建的标准应用程序如何使用这些功能：

![架构](/help/assets/app-builder/app-builder-architecture.jpeg)

有关App Builder架构的更多详细信息，请参阅[架构概述](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}。

## App Builder入门 {#additional-resources}

可通过阅读以下博客帖子来查找可组合商务策略的概述，包括初始设置：

[App Builder如何帮助提高商务平台的业务敏捷性](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

为帮助您开始使用App Builder，Adobe已创建以下文档：

* [App Builder快速入门](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 通过文档继续学习 {#appbuilder-documentation}

App Builder为开发人员提供视频和文档，包括指南和参考文档，以帮助开发您自己的自定义应用程序：

* [App Builder文档](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder视频](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 试用其中一个示例应用程序 {#appbuilder-codesamples}

准备好开始开发了吗？ 以下链接包含帮助您入门的示例应用程序：

* Adobe Developer网站上的[App Builder代码实验室](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 支持 {#support}

对于开发人员支持请求，请使用[Experience League论坛](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"}寻求帮助。

{{$include /help/_includes/app-builder-related-links.md}}
