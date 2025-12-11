---
title: GraphQL简介
description: 了解如何在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL。 对Adobe Commerce和 [!DNL Magento Open Source]使用GraphQL GET和POST调用。
short-description: 了解如何为Adobe Commerce和 [!DNL Magento Open Source]使用GraphQL GET和POST调用。
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# GraphQL简介

这是GraphQL和Adobe Commerce系列的第1部分。 GraphQL已迅速成为功能强大的客户端应用程序与后端交互的行业标准。 随着该平台在Headless实施领域的不断扩展，它对于Adobe Commerce开发人员来说越来越重要。

如果您不熟悉GraphQL，本节将引导您了解基本概念和用法。

>[!VIDEO](https://video.tv.adobe.com/v/3443952?captions=chi_hans&learn=on)

## 本系列中GraphQL的相关视频和教程

* [第2部分GraphQL — 查询](../graphql-rest/graphql-queries.md)
* [第3部分GraphQL — 突变](../graphql-rest/graphql-mutations.md)
* [第4部分GraphQL — 架构](../graphql-rest/graphql-schema.md)

## 什么是GraphQL？

GraphQL是一种针对唯一API查询语言和运行时的规范，提供数据以响应该查询语言。

诸如REST之类的传统Web API非常适用于数据来回传递的不同系统，但对于诸如渐进式Web应用程序之类的现代应用程序链接体验而言，其性能却未能达到峰值。 在这样的应用程序中，_same_&#x200B;应用程序的前端层和后端层通过Web API进行通信。 REST等方案的程序化方法通常无法提供适当的灵活性，在这种情况下需要快速获取多种类型的数据。

GraphQL允许客户端显式地描述&#x200B;_确切的_&#x200B;所需数据。 单个请求可以查询多种类型，而不需要多个网络请求来获取多种数据类型。 而且，通过仅包含所请求的类型和字段（以直观地反映查询的格式），响应保持精简状态。

实施GraphQL规范的运行时可以用任何语言构建。 Adobe Commerce和[!DNL Magento Open Source]使用
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP实现并在其上构建自己的层。

[查看完整的GraphQL文档](https://graphql.org/learn){target="_blank"}

## 使用GraphQL客户端

您需要GUI GraphQL客户端来测试代码示例和教程。 有几个选项：

* [Altair](https://altairgraphql.dev/){target="_blank"}是专为GraphQL构建的优秀且功能齐全的客户端。 Adobe在演练视频中使用Altair。
* 如果不想安装桌面应用程序，则还有可在
  [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}、Firefox或[Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"}浏览器。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"}是GraphQL Foundation中的GraphQL IDE的实现。 这不是一个可安装的工具，而是一个可用于自行构建界面的软件包。
* 如果您已熟悉[Postman](https://www.postman.com/){target="_blank"}，它可为GraphQL查询提供良好的支持，但并不像专用的GraphQL客户端那样功能完善。

在GraphQL客户端中，您应该将请求提交到Adobe Commerce或`/graphql`实例上的URL路径[!DNL Magento Open Source]。 如果您希望将现有实例用于测试，则可以使用Venia主题的演示(PWA Studio的示例实现)： `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
