---
title: Out-of-process extensibility for Adobe Commerce
description: 了解什么是 Adobe App Builder 以及为什么它是进程外可扩展性的一个重要方面。
jira: KT-11433
doc-type: Tutorial
duration: 322
last-substantial-update: 2023-02-16T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
TQID: https://experienceleague.adobe.com/c3dl6gZ7Jtje5rZCB9HrwmCbFrcu8bllL0z0B9cl5Jg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 777
ht-degree: 2%

---

# Introduction to App Builder

Historically, Adobe Commerce development has used in-process extensibility. The in-process model requires any new code to be compatible with upgrades, the server&#39;s PHP version, and many other essential server applications and services that Commerce uses. Adobe Developer App Builder uses out-of-process extensibility to avoid these compatibility issues.

## 适用于Adobe Commerce的App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?learn=on)

Adobe Developer App Builder is a serverless extensibility platform for integrating and creating custom experiences to extend Adobe solutions, and it&#39;s now available for Adobe Commerce. With App Builder, you can build secure and scalable apps that extend Commerce-native functionality and integrate with third-party solutions. As a developer, you can now take advantage of out-of-process extensibility with Adobe Commerce and that in turn provides immediate and long-term benefits.

App Builder provides a unified third-party extensibility framework for integrating and creating custom applications that extend [!DNL Adobe Commerce]. Since this extensibility framework is built on Adobe&#39;s infrastructure, developers can build custom microservices, and extend and integrate [!DNL Adobe Commerce] across other Adobe solutions and third-party integrations.

App Builder provides a way for customers to extend [!DNL Adobe Commerce] in various use cases:

* middleware extensibility - Connect external systems with Adobe applications by building custom connectors or take advantage of a suite of pre-built integrations.
* core services extensibility - Extend core application capabilities by extending the default behavior with custom features and business logic.
* user experience extensibility - Extend core experience to support business requirements or build customer-specific digital properties, storefronts, and back-office applications.

Adobe Developer App Builder is a cloud-based solution, which means that it automatically scales. This service is also globally distributed to allow the best performance regardless of your geographic location.

## Why should you learn more about App Builder

Since Adobe Commerce is not a fully SAAS product, the code you develop can add complexity and upgrade issues. By using out-of-process extensibility, such as App Builder, you can provide custom, unique functionality to your Adobe Commerce store without requiring in-process methods.

Other benefits include:

* Decoupled features allow for faster time to launch.
* Upgrades are now easier. 自定义功能位于Commerce代码库之外，这可以防止升级时出现兼容性问题。
* 将功能和逻辑移出Commerce可释放通常由进程内开发方法使用的资源。

## 架构 {#architecture}

Adobe Developer App Builder提供了一个通用、一致和标准化的开发平台，用于扩展Adobe Cloud解决方案（例如Adobe Commerce），而不是开箱即用的解决方案，该平台包括：

* Adobe Developer Console用于自定义微服务和扩展开发。 在访问创建插件和集成所需的所有工具和API时，构建和管理项目。
* 开源工具、SDK和库来构建自定义扩展和集成。 使用React Spectrum（Adobe的UI工具包）为所有Adobe应用程序具有一个通用UI。
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
* [App Builder videos](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Try Out One of the Sample Applications {#appbuilder-codesamples}

Ready to start developing? The following link contains sample applications to help get you started:

* [App Builder Code Labs on Adobe Developer Website](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 支持 {#support}

For developer support requests, use the [Experience League forum](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} for assistance.

{{$include /help/_includes/app-builder-related-links.md}}
