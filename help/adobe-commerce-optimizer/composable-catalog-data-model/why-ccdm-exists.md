---
title: 为什么存在可组合目录数据模型(CCDM)
description: 了解CCDM如何保持一个统一的目录，同时店面使用目录视图和促销服务接收过滤的产品、价格和规则。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 259
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-18624
source-git-commit: bfe282e4f1ef04985cffb109bce90bc05a70fda0
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# 为什么存在可组合目录数据模型

现代商务团队经常跨&#x200B;**品牌**、**地区**、**经销商**&#x200B;和&#x200B;**数字渠道**&#x200B;进行销售。 当每个渠道都有自己的目录副本时，团队在协调SKU、价格和可用性方面花费的时间会多于改善购物者体验的时间。 **Adobe Commerce Optimizer**&#x200B;后面的&#x200B;**Adobe可组合目录数据模型(CCDM)**&#x200B;旨在反转该模式：在SaaS层中&#x200B;**一个统一目录**，具有&#x200B;**目录视图**&#x200B;和&#x200B;**策略**，可决定每个店面或集成允许查看的内容。

## 此视频面向谁？

* 刚开始使用Adobe Commerce Optimizer且在配置目录源、视图和策略之前需要业务上下文的Commerce解决方案架构师和开发人员

## 视频内容

* 为什么复制，特定于渠道的目录会带来运营风险，并导致创新变慢
* CCDM如何在保持产品数据统一的同时支持多品牌和多区域方案
* 目录视图如何充当共享基础目录与特定店面或受众之间的“镜头”
* Merchandising Services API如何使用这些视图，使Headless体验与配置的目录保持一致

>[!VIDEO](https://video.tv.adobe.com/v/3491285?learn=on)

## 孤立的目录带来的挑战

当每个经销商站点、区域店面或品牌资产维护&#x200B;**自己的目录数据库**&#x200B;时，几个问题会复杂化：

* **重复** — 多次输入相同的SKU、描述和媒体。
* **漂移** — 价格更新、新属性或停产项目位于一个渠道中，但不位于其他渠道中。
* **启动速度较慢** — 每个新的接触点都会重复大量数据工作，而不是重复使用单个产品记录。

CCDM存在，因此产品信息可以存在于其他系统扩充的&#x200B;**一个可组合目录**&#x200B;中，而店面仍接收&#x200B;**适合渠道的**&#x200B;分类和定价。

## 可组合目录数据模型的变化

在Adobe Commerce Optimizer中，产品数据从一个或多个&#x200B;**目录源**（例如区域设置，如`en-US`，或上游系统，如PIM或ERP）中&#x200B;**摄取到统一的基本目录**&#x200B;中。 该源提供原始属性和值。

**目录视图**&#x200B;然后定义该统一数据如何为业务上下文&#x200B;**组织和公开**：哪些产品传递您的&#x200B;**策略**，哪些&#x200B;**价格手册**&#x200B;适用，哪些&#x200B;**目录源**&#x200B;支持视图。 因此，相同的基础记录可以支持&#x200B;**多个投影**，例如每个经销商、地区或品牌的单独视图，而无需为每个站点克隆整个目录。

这种分离 — **数据来自** （目录源）而不是&#x200B;**其呈现方式** （目录视图） — 是团队采用CCDM而不是为每个渠道维护并行目录的核心原因。

## 作为店面镜头的目录视图

如[促销服务的目录视图](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}中所述，目录视图的行为类似于&#x200B;**镜头**：购物者只能看到视图允许的产品、价格和规则，而&#x200B;**基本目录**&#x200B;仍然是共享记录系统。 该模型直接与&#x200B;**促销服务**&#x200B;配对，因此API客户端传递正确的视图（和相关标头）并为每个体验接收一致的策略驱动响应。

有关这些部分如何适合端到端流程的更深入演练，请参阅开发人员演练[为您的店面创建可编写的目录](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}。

## 相关内容

* [了解目录视图](./learn-about-the-ccdm-feature-catalog-views.md)
* [Merchanding Services的目录视图](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [为您的店面创建可组合目录](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [促销API快速入门](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
