---
title: 了解可组合目录数据模型中的目录视图
description: 了解Adobe可组合目录数据模型(CCDM)中的目录视图如何将一个基本目录映射到具有不同产品、价格和规则的每个店面。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: 96a1356a399fa5cdca9d9befd7c14ebad1b0162f
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Adobe可组合目录数据模型中的目录视图

目录视图是您以不同于单个可组合目录的方式为每个受众提供服务的方式。 本教程将介绍目录视图是什么、Adobe Commerce Optimizer如何将其应用于购物者，以及为什么唯一的视图ID是产品、价格和规则的锚点 — 使用&#x200B;**Carvelo Automots**&#x200B;演示场景。

## 此视频面向谁？

* 在Adobe Commerce Optimizer上为多品牌或多经销商目录建模的Commerce解决方案架构师和开发人员

## 视频内容

* Carvelo汽车方案（品牌、经销商和协议）
* 什么是目录视图，以及统一基础目录上的“镜头”隐喻
* 店面如何使用目录视图来筛选产品和定价（例如Celport）
* 目录视图唯一ID和单一真实来源的业务价值

>[!VIDEO](https://video.tv.adobe.com/v/3491261?learn=on)

## 情景：Carvelo汽车

**Carvelo Automoles**&#x200B;是一家虚构的汽车部件公司，通常用于Adobe Commerce演示、文档和教程。 Carvelo通过三个经销商（**Aurora**、**Bolt**&#x200B;和&#x200B;**Cruz**）销售跨三个品牌的部件：

* **Arkbridge**&#x200B;属于West Coast Inc.
* **Kingsbluff**&#x200B;和&#x200B;**Celport**&#x200B;属于East Coast Inc.

每家经销商在允许销售哪些产品方面都有自己的协议。 这会创建一个非常复杂的设置，并且它仍然可以从包含大约600万个SKU的&#x200B;**单个基础目录**&#x200B;运行。 使此功能成为可能的功能是&#x200B;**目录视图**。

## 什么是目录视图？

**目录视图**&#x200B;是您针对特定业务上下文配置的目录视图。 将其视为&#x200B;**镜头**。 您的统一基础目录位于中间，包含每个品牌的每个SKU。 每个经销店都有自己的镜头 — 自己的目录视图。

在示例中：

* **Arkbridge**&#x200B;镜头可以显示所有三个品牌：Aurora、Bolt和Cruz部件。
* **Celport**&#x200B;镜头仅显示Bolt和Cruz部分的子集。

每个目录视图还可以控制&#x200B;**定价**。 **基础目录数据不会更改**，只会更改该上下文的可查看方面（分类、价格和规则）。

## Commerce Optimizer如何应用目录视图

当购物者访问&#x200B;**Celport**&#x200B;店面时，Adobe Commerce Optimizer会使用&#x200B;**Celport目录视图**&#x200B;来确定确切适用的产品、价格和规则。 购物者只能看到该镜头允许的内容。

目录中可能仍存在其他产品，例如Aurora轮胎、Bolt发动机或Cruz电池，但是&#x200B;**如果目录视图不允许公开，则** Celport的店面从不公开这些产品。

## 目录视图ID和业务价值

每个目录视图都有一个&#x200B;**唯一ID**。 该ID可将您的店面连接到其目录配置。 您在店面配置中设置一次该配置，然后遵循下游行为 — 正确的产品、正确的价格和正确的规则。

您不必为每个经销商或品牌维护单独的目录并保持同步，而是维护&#x200B;**一个**&#x200B;可组合目录。 目录视图是您塑造与该&#x200B;**单一真实来源**&#x200B;不同的&#x200B;**多个店面体验**&#x200B;的方式。

## 相关内容

* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [促销API快速入门](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
