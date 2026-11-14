---
title: Commerce与Adobe的组合
description: 了解带Adobe平台选项、Edge Delivery Services店面、API Mesh、App Builder和Commerce开发MCP工具的可组合商务。
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# 可组合商务

可组合商务是一种通过API将商务功能结合在一起的架构方法。 您可以选择并集成用于店面体验、目录管理、产品发现、结账和其他业务需求的组件，而不是要求每项功能都来自一个紧密耦合的应用程序。

Headless商务将店面展示层与商务后端分离。 可组合商务构建于该分离之上，允许团队选择要保持在一起的功能和独立集成的功能。 Headless店面本身不会使每个后端组件都可以独立替换。

Adobe通过commerce平台选项、店面技术以及集成和开发工具支持这种方法。 Adobe Commerce as a Cloud Service (ACCS)、Adobe Commerce Optimizer (ACO)、Edge Delivery Services、API Mesh、App Builder和Commerce开发MCP有不同的用途。 它们不是可互换的产品或所需的组合栈栈。

## 好处和取舍

组合式方法可以帮助您：

* **调整客户体验：**&#x200B;选择店面组件并集成符合您业务要求的服务。
* **逐步现代化：**&#x200B;替换选定功能，同时保留其他系统及其数据。
* **独立扩展和演进：**&#x200B;设计集成和扩展，使其更改不需要发布整个商务应用程序。
* **重复使用商务功能：**&#x200B;使用受支持的平台功能和API，而不是重建每个业务功能。

这些好处取决于实施。 评估技术成熟度、交付成本、许可、安全性和数据隐私。 定义跨服务的API合同、数据同步、身份验证、性能和故障的所有权。 模块化程度更高的体系结构还可以增加集成和操作复杂性。

## Adobe Commerce架构选项

首先，决定您是需要托管商务平台，还是希望围绕现有事务型后端打造现代化客户体验。

有关视频比较，请参阅[Adobe可用的商务选项](./adobes-available-commerce-options.md)。

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Service是一个托管的SaaS商务平台。 Adobe管理核心应用程序、基础架构和更新。 其架构包括Commerce Foundation、Headless店面功能、促销服务和开发人员工具。

在评估具有支持的扩展点的托管商务基础时，使用此选项。 对于自定义业务逻辑，应规划受支持的进程外可扩展性，而不是假定可以不更改地部署现有PHP扩展。 ACCS不支持Luma店面；如果适用，请在实施计划中纳入店面迁移。

请参阅[Adobe Commerce as a Cloud Service概述](https://experienceleague.adobe.com/zh-hans/docs/commerce/cloud-service/overview)。

### Adobe Commerce Optimizer

Adobe Commerce Optimizer支持目录管理、产品发现、搜索、推荐和店面体验，同时允许您保留现有的商务后端。 目录数据可能来自外部源，并且第三方系统仍可负责购物车和结账生命周期。

如果目标是实现面向客户的现代化，并且不需要替换事务型后端，则使用此选项。 明确规划目录摄取以及购物车和结账集成。 ACCS和ACO是独立的体系结构选项，而不是所需的组合栈栈。 在为您的实施选择服务之前，请验证当前集成支持。

查看[Adobe Commerce Optimizer概述](https://experienceleague.adobe.com/zh-hans/docs/commerce/optimizer/overview)。

### 现有Adobe Commerce部署

云基础架构和内部部署上的Adobe Commerce还可以逐步采用Headless存储前端和受支持的基于API的集成。 根据部署，您可以保留现有店面或采用受支持的Headless店面。 将现有的商务功能保持在一起可以减少集成工作，同时允许选定的服务单独发展。

功能可用性、扩展兼容性、许可和设置要求因产品和版本而异。 在选择架构之前，请比较[Adobe Commerce平台和服务](https://experienceleague.adobe.com/zh-hans/docs/commerce/information-center/explore-platforms-and-services/platform-and-services)。

## 由Edge Delivery Services提供支持的Commerce店面

由Edge Delivery Services提供支持的Commerce Storefront提供店面体验和内容交付层。 其Headless架构使团队可以独立于商业基础开发前端。 基于文档的创作和可视化编辑支持内容更新，而店面组件使用商务API进行购物功能。

区分负责目录数据、定价、购物车操作、结账和订单的店面界面和系统。 Edge Delivery Services不会替换事务性商务后端。 在实施店面之前，确定所选后端支持的服务和集成路径。

请参阅[Commerce店面文档](https://experienceleague.adobe.com/en/tools/commerce-storefront/)。

## 集成性和可扩展性

API Mesh和App Builder有助于连接和扩展商务功能，而无需将每个自定义设置嵌入核心商务应用程序中。

### API网格

Adobe Developer App Builder的API Mesh将多个Adobe、专用和第三方API源整合到一个可查询的GraphQL端点中。 使用它来编写前端体验的数据并协调对现有服务的访问。

API组合不会消除定义API约定、身份验证、错误处理或事务性业务逻辑所有权的需要。 在集成设计中明确规定这些职责。

查看[API网格概述](https://developer.adobe.com/graphql-mesh-gateway/)。

### Adobe App Builder

Adobe App Builder为扩展Adobe解决方案并在Adobe基础架构（包括Adobe I/O Runtime）上运行的应用程序提供了一个平台。 您可以将其用于进程外的自定义以及与系统（如CRM、ERP和PIM）的集成。

为您使用的Commerce产品选择支持的扩展点、活动和Webhook。 与核心商务版本生命周期分开设计和测试应用程序。

查看[App Builder概述](https://developer.adobe.com/app-builder/docs/intro_and_overview/)。

## Commerce开发MCP和技能

Commerce开发MCP和技能支持人工智能辅助的App Builder应用程序和店面自定义的开发。 在迁移到ACCS时，它们还可以帮助将现有Adobe Commerce PHP扩展迁移到App Builder应用程序。

模型上下文协议(MCP)服务器将兼容的人工智能编码客户端连接到开发工具。 Adobe的工具包括Commerce Developer MCP Server、App Builder MCP Server和Dropins AI Tools，以及特定于Commerce的技能。

Commerce开发MCP是开发工具，而非storefront投放、购物车、结账或订单处理服务。 它们不会使应用程序自动兼容每种Commerce产品。 审查生成的更改，并针对目标环境测试这些更改。

查看[Commerce开发MCP和技能](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools)。

## 规划可组合的实施

1. 定义业务目标、客户体验、安全要求以及团队可用的技能。
2. 选择Commerce平台，并确定要保留或替换的现有系统。
3. 选择受支持的店面并定义演示、推销和事务性服务之间的边界。
4. 为目录信息、价格、库存、客户、购物车、结账和订单建立数据所有权和API合同。
5. 跨集成规划身份验证、数据同步、缓存、监控和故障处理。
6. 在每次发布之前逐步实施并验证功能、性能和安全性。

当组件的组件满足业务要求并可靠地协作时，可组合实施会取得成功。 选择您需要的功能，而不是单纯地添加服务来使体系结构更加模块化。

## 相关资源

探索这些教程、视频、实施指南和播放列表，规划您的架构并构建商务集成。

### Commerce平台和店面

* [Adobe Commerce as a Cloud Service架构](../commerce-as-a-cloud-service/architecture.md)
* [组合式店面和模块化体系结构](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Adobe Commerce Optimizer概述](../adobe-commerce-optimizer/overview.md)
* [为什么存在可组合目录数据模型](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [Edge Delivery Services的购物者体验](../edge-delivery-services/shopper-experience.md)

### 集成与开发

* [App Builder的进程外可扩展性](../app-builder/foundations/introduction-to-app-builder.md)
* [API Mesh入门](../api-mesh/getting-started-api-mesh.md)
* [创建具有多个源的API网格](../api-mesh/graphql-multiple-source.md)
* [使用AI和MCP工具制定定制运费](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### 架构和开发文档

* [迁移到Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/zh-hans/docs/commerce/cloud-service/migration/overview)
* [Adobe Commerce Optimizer集成](https://experienceleague.adobe.com/zh-hans/docs/commerce/optimizer/integrations-overview)
* [Commerce Storefront开发入门](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Commerce开发MCP和技能的最佳实践](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### 播放列表

* [使用Adobe App Builder和Adobe Commerce开始使用API Mesh](https://experienceleague.adobe.com/zh-hans/playlists/commerce-get-started-app-builder-and-api-mesh)
