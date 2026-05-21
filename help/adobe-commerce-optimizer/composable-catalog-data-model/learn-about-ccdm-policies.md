---
title: 了解可组合目录数据模型中的CCDM策略
description: 了解Adobe可组合目录数据模型中的策略如何在每个目录视图上通过STATIC rules和TRIGGER标头过滤产品。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 378
last-substantial-update: 2026-05-21T00:00:00Z
jira: KT-21258
source-git-commit: 43fee759ba8ea76dfa91f9ae838a6ad3474d2bcb
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 0%

---

# Adobe可组合目录数据模型中的策略

如果&#x200B;**目录视图**&#x200B;是塑造购物者从统一基础目录看到的内容的镜头，则&#x200B;**政策**&#x200B;是该镜头的组成部分。 本教程将介绍什么是策略，**STATIC**&#x200B;和&#x200B;**TRIGGER**&#x200B;策略如何在&#x200B;**Carvelo Automots**&#x200B;演示方案中协同工作，以及为什么更新策略会立即生效 — 而不重建目录。

## 此视频面向谁？

* 在Adobe Commerce Optimizer上配置目录视图和促销规则的Commerce解决方案架构师和开发人员

## 视频内容

* 将策略用作产品属性的数据访问过滤器
* 多个策略在Celport目录视图中的组合方式（品牌和部件类别）
* 用于永久业务规则的静态策略
* 由API请求标头激活的触发器策略（例如`AC-Policy-Brand`）
* 在不重建目录的情况下更新日常操作中的策略

>[!VIDEO](https://video.tv.adobe.com/v/3491413?learn=on)

## 情景：Carvelo Automotor和Celport

**Carvelo Automoles**&#x200B;是一家在Adobe Commerce演示中使用的虚拟汽车部件公司。 Carvelo通过包括&#x200B;**Celport**&#x200B;在内的经销商跨三个品牌销售部件：**Aurora**、**Bolt**&#x200B;和&#x200B;**Cruz**。

在[Celport目录视图](./learn-about-the-ccdm-feature-catalog-views.md)中，两个策略协同工作：

1. **品牌筛选器** — 仅允许&#x200B;**Bolt**&#x200B;和&#x200B;**Cruz**&#x200B;品牌。 Aurora产品不通过此过滤器。
2. **类别筛选器** — 将可见产品限制为仅&#x200B;**个制动器**&#x200B;和&#x200B;**暂停**。

产品必须满足&#x200B;**每个活动策略**&#x200B;才能显示。 其他所有内容都将被过滤掉。

策略评估产品属性，如&#x200B;**品牌**、**车辆型号**&#x200B;或&#x200B;**部件类别**，并定义允许哪些产品通过该目录视图。

## 什么是策略？

**策略**&#x200B;是&#x200B;**数据访问筛选器**。 它检查产品属性并应用用于确定目录视图可以公开哪些产品的规则。 策略位于共享的可组合目录的顶部 — 它们不会复制目录数据。

## 静态策略

**STATIC**&#x200B;策略为&#x200B;**始终为**。 它强制实施永久业务规则，而不管购物者行为或会话状态如何。

在Celport方案中， STATIC规则包括：

* Celport将仅销售&#x200B;**Bolt**&#x200B;和&#x200B;**Cruz**&#x200B;品牌。
* Celport将仅显示&#x200B;**个制动器**&#x200B;和&#x200B;**暂停**&#x200B;部分。

这些规则永远不会关闭。 静态策略包含&#x200B;**许可协议**、**地域限制**&#x200B;和&#x200B;**品牌权限**。 设置一次，Adobe Commerce Optimizer会在每次请求时自动实施它们。

## 触发策略

值为&#x200B;**TRIGGER**&#x200B;的策略有时称为&#x200B;**独占策略**。 仅当在API调用的标头中指定了触发器&#x200B;**时，目录视图才运行该策略**。

例如，Experience Delivery Services (EDS)店面可能提供包含&#x200B;**所有品牌**、**Aurora**、**Bolt**&#x200B;和&#x200B;**Cruz**&#x200B;的下拉列表：

* 在初始页面查看中，未选择任何内容，因此不会发送额外的品牌标题。
* 当购物者选择&#x200B;**Bolt**&#x200B;时，基础API将`AC-Policy-Brand`标头设置为`Bolt`。
* 当购物者选择&#x200B;**Cruz**&#x200B;时，同一标头设置为`Cruz`。

目录视图仅在标头存在时应用触发器策略，该标头支持交互式筛选，而不更改永久的静态规则。

## STATIC和TRIGGER

**静态**&#x200B;策略处理永久规则。 **TRIGGER**&#x200B;策略处理交互式策略。 在单个目录视图上一起使用时，它们提供了&#x200B;**合规性**&#x200B;和&#x200B;**灵活性** — 固定分类边界加上购物者驱动的细化。

## 更新策略而不重建目录

对于日常操作，策略更改是即时的。 假设Celport的许可协议现在允许&#x200B;**轮胎**&#x200B;以及制动和暂停：

1. 打开相关策略。
1. 将&#x200B;**tires**&#x200B;添加到值列表。
1. 保存。

该更改&#x200B;**立即启用**。 没有目录重建、重新部署或等待时间。 对Celport店面的下一个请求反映了此更新。

策略是&#x200B;**共享目录**&#x200B;上的轻量级过滤器，而不是托管到单独目录副本中的规则。 **更改规则，而非数据。**

## 相关内容

* [为什么存在可组合目录数据模型](./why-ccdm-exists.md)
* [了解目录视图](./learn-about-the-ccdm-feature-catalog-views.md)
* [Merchanding Services的目录视图](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [促销API快速入门](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
