---
title: 适用于Adobe Commerce的流程外可扩展性
description: 了解Adobe应用程序生成器，以及它为何是流程外扩展性的一个重要方面。
landing-page-description: 了解什么是应用程序生成器，以及它如何帮助制定Adobe Commerce开发策略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-11T00:00:00Z
source-git-commit: ef0fa95e776b97ddbaf30e0acd1340e30f12738f
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 0%

---


# 流程外可扩展性

Adobe Commerce开发过去使用与主应用程序相同的存储库来完成。  这称为进程中。  该技术非常好，为开发者提供了扩展应用程序的预期机制。  然而，这是有代价的。  每次向代码库添加新代码时，它都必须与任何升级都兼容。  您还必须与服务器PHP版本以及商务将利用的许多其他服务器应用程序和服务兼容。  Adobe Developer App Builder对扩展功能提出了相同的要求，但将其移离了站点。  代码和逻辑是完全外部的，这种方法称为过程。

## 适用于Adobe Commerce的App Builder {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder为开发人员提供了一个可扩展性框架来扩展 [!DNL Adobe Commerce] 提供过程外的可扩展性。

App Builder提供了统一的第三方可扩展性框架，用于集成和创建可扩展的自定义应用程序 [!DNL Adobe Commerce]. 由于此扩展性框架是基于Adobe的基础架构构建的，因此开发人员可以构建自定义微服务，以及扩展和集成 [!DNL Adobe Commerce] 跨Adobe解决方案和其他第三方集成。

应用程序生成器为客户提供了一种扩展方式 [!DNL Adobe Commerce] 在各种用例中：

* 中间件可扩展性 — 通过构建自定义连接器或利用一套预建集成，将外部系统与Adobe应用程序连接起来。
* 核心服务可扩展性 — 通过使用自定义功能和业务逻辑扩展默认行为来扩展核心应用程序功能。
* 用户体验可扩展性 — 扩展核心体验以支持业务需求或构建特定于客户的数字资产、店面和后台应用程序。

应用程序生成器（以前称为项目Firefly）是基于云的解决方案，这意味着它会自动缩放。 此服务还在全球范围内进行分发，无论您的地理位置如何，都可提供最佳性能。

## 为什么您应进一步了解App Builder

由于Adobe Commerce不是完全SAAS，因此您开发或安装的代码可能会增加复杂性和升级问题。 通过使用流程外的可扩展性（如应用程序生成器），您可以为Adobe Commerce存储提供自定义的独特功能，而无需使用流程内方法。

其他好处包括：

* 去耦功能可加快启动速度。
* 升级现在更轻松。 自定义功能不在商务代码库之外，这可以防止升级时出现兼容性问题。
* 在商务之外移动功能和逻辑可释放通常用于在进行开发方法的资源。

## 架构 {#architecture}

Adobe Developer App Builder提供了一个通用、一致且标准化的开发平台，用于扩展Adobe云解决方案(如Adobe Commerce)，它不是现成的解决方案，它包括：

* Adobe Developer控制台用于自定义微服务和扩展开发。 构建和管理项目，同时访问创建插件和集成所需的所有工具和API。
* 用于构建自定义扩展和集成的开源工具、SDK和库。 使用React Spectrum(Adobe的UI工具包)为所有Adobe应用程序提供一个通用UI。
* 服务，如用于在Adobe的无服务器平台上托管基础架构的I/O运行时，和用于基于事件的集成的I/O事件。 Adobe还提供用于存储数据和文件的开箱即用支持。
* Adobe Experience Cloud ，您可以在其中提交要在Experience Cloud组织中发布的扩展和集成。系统管理员可以审核、管理和批准这些扩展。 发布后，您的自定义App Builder扩展和工具可以与其他Adobe Experience Cloud应用程序一起使用。

下图说明了在App Builder上构建的标准应用程序如何使用这些功能：

![架构](/help/assets/app-builder/firefly-architecture.jpeg)

有关应用程序生成器架构的更多详细信息，请参阅 [架构概述](https://developer.adobe.com/app-builder/docs/guides/).

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

