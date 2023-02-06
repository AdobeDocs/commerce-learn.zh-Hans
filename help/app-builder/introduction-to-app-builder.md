---
title: 适用于Adobe Commerce的流程外可扩展性
description: 了解Adobe应用程序生成器，以及它为何是流程外扩展性的一个重要方面。
landing-page-description: 了解什么是App Builder，以及它如何帮助制定Adobe Commerce开发策略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-24T00:00:00Z
source-git-commit: 336581ac6b695d8b847d88daadeb3784ece97ae7
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---


# 应用程序生成器简介

以前，Adobe Commerce开发使用过进程内可扩展性。 进程中模型要求任何新代码与升级、服务器的PHP版本以及商务使用的许多其他基本服务器应用程序和服务兼容。 Adobe Developer App Builder使用过程外的可扩展性来避免这些兼容性问题。

## 适用于Adobe Commerce的App Builder {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder是一个无服务器的扩展性平台，用于集成和创建自定义体验以扩展Adobe解决方案，现在可用于Adobe Commerce。 借助App Builder，您可以构建安全且可扩展的应用程序，这些应用程序扩展了商务原生功能并与第三方解决方案集成。 作为开发人员，您现在可以利用Adobe Commerce的流程外可扩展性，这反过来又提供了即时和长期的优势。

App Builder提供了统一的第三方可扩展性框架，用于集成和创建可扩展的自定义应用程序 [!DNL Adobe Commerce]. 由于此扩展性框架是基于Adobe的基础架构构建的，因此开发人员可以构建自定义微服务，以及扩展和集成 [!DNL Adobe Commerce] 跨其他Adobe解决方案和第三方集成。

应用程序生成器为客户提供了一种扩展方式 [!DNL Adobe Commerce] 在各种用例中：

* 中间件可扩展性 — 通过构建自定义连接器或利用一套预建集成，将外部系统与Adobe应用程序连接起来。
* 核心服务可扩展性 — 通过使用自定义功能和业务逻辑扩展默认行为来扩展核心应用程序功能。
* 用户体验可扩展性 — 扩展核心体验以支持业务需求或构建特定于客户的数字资产、店面和后台应用程序。

应用程序生成器（以前称为项目Firefly）是基于云的解决方案，这意味着它会自动缩放。 此服务还在全球范围内进行分发，无论您的地理位置如何，都可提供最佳性能。

## 为什么您应进一步了解App Builder

由于Adobe Commerce不是完全SAAS产品，因此您开发的代码可能会增加复杂性并升级问题。 通过使用流程外的可扩展性（如App Builder），您可以为Adobe Commerce存储提供自定义的独特功能，而无需使用流程内方法。

其他好处包括：

* 去耦功能可加快启动速度。
* 升级现在更轻松。 自定义功能位于商务代码库之外，这可防止升级时出现兼容性问题。
* 将功能和逻辑移出Commerce可释放通常用于在进行开发方法的资源。

## 架构 {#architecture}

Adobe Developer App Builder提供了一个通用、一致且标准化的开发平台，用于扩展Adobe云解决方案(如Adobe Commerce)，它不是现成的解决方案，它包括：

* Adobe Developer控制台用于自定义微服务和扩展开发。 构建和管理项目，同时访问创建插件和集成所需的所有工具和API。
* 用于构建自定义扩展和集成的开源工具、SDK和库。 使用React Spectrum(Adobe的UI工具包)为所有Adobe应用程序提供一个通用UI。
* 服务，如用于在Adobe的无服务器平台上托管基础架构的I/O运行时，和用于基于事件的集成的I/O事件。 Adobe还提供用于存储数据和文件的开箱即用支持。
* Adobe Experience Cloud ，您可以在其中提交要在Experience Cloud组织中发布的扩展和集成。系统管理员可以审核、管理和批准这些扩展。 发布后，您的自定义App Builder扩展和工具可以与其他Adobe Experience Cloud应用程序一起使用。

下图说明了在App Builder上构建的标准应用程序如何使用这些功能：

![架构](/help/assets/app-builder/firefly-architecture.jpeg)

有关应用程序生成器架构的更多详细信息，请参阅 [架构概述](https://developer.adobe.com/app-builder/docs/guides/).

## AmazonSales Channel扩展 {#amazon-sales-channel-extension}

以下教程演示了如何使用App Builder扩展将Adobe Commerce连接到AmazonSales Channel。

* [应用程序生成器技术概述](../app-builder/app-builder-technical-overview.md)
* [可扩展性框架](../app-builder/extensibility-framework-commerce-eventing.md)
* [功能演示App Builder](../app-builder/app-builder-functional-demonstration.md)

## 应用程序生成器入门 {#additional-resources}

为了帮助您开始使用应用程序生成器，Adobe创建了以下文档：

* [应用程序生成器快速入门](https://developer.adobe.com/app-builder/docs/getting_started/)

## 继续学习文档 {#appbuilder-documentation}

应用程序生成器为开发人员提供视频和文档，包括可帮助开发您自己的自定义应用程序的指南和参考文档：

* [应用程序生成器文档](https://developer.adobe.com/app-builder/docs/overview/)
* [应用程序生成器视频](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## 试用其中一个示例应用程序 {#appbuilder-codesamples}

准备好开始开发了吗？ 以下链接包含帮助您入门的示例应用程序：

* [Adobe Developer网站上的应用程序生成器代码实验室](https://developer.adobe.com/app-builder/docs/resources/)

## 支持 {#support}

对于开发人员支持请求，请使用 [Experience League论坛](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) 以寻求帮助。
