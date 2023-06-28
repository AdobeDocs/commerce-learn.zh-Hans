---
title: GraphQL简介
description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 对Adobe Commerce和使用GraphQLGET和POST调用 [!DNL Magento Open Source].
landing-page-description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 对Adobe Commerce和使用GraphQLGET和POST调用 [!DNL Magento Open Source].
short-description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 对Adobe Commerce和使用GraphQLGET和POST调用 [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# GraphQL简介

GraphQL已迅速成为功能强大的客户端应用程序与后端通信的行业标准。 随着该平台在Headless实施领域的不断扩展其功能，它对Adobe Commerce开发人员来说越来越重要。

如果您不熟悉GraphQL，本节将引导您了解基本概念和用法。

## 什么是GraphQL？

GraphQL是一种针对唯一API查询语言和运行时的规范，提供数据以响应该查询语言。

像REST这样的传统Web API非常适用于数据来回传递的不同系统，但对于Progressive Web Application等现代应用程序链接体验而言，它们提供的性能却低于峰值。 在诸如此类的应用程序中， _相同_ 应用程序通过Web API进行通信。 REST等方案的规范化方法通常不能提供适当的灵活性，因为在这种情况下需要快速获取多种数据。

GraphQL允许客户用表达的方式描述 _完全匹配_ 所需的数据。 单个请求可以查询多种类型，而不需要多个网络请求来获取多种数据类型。 而且，通过仅包含所请求的类型和字段（以直观地镜像查询的格式），响应保持精简状态。

实施GraphQL规范的运行时可以用任何语言构建。 Adobe Commerce和 [!DNL Magento Open Source] 使用
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP实施并在其上构建自己的层。

[查看完整的GraphQL文档](https://graphql.org/learn){target="_blank"}

## 使用GraphQL客户端

您需要GUI GraphQL客户端来测试代码示例和教程。 有几个选项：

* [阿尔泰](https://altairgraphql.dev/){target="_blank"} 是专门为GraphQL构建的优秀且功能齐全的客户端。 Adobe在演练视频中使用Altair。
* 如果您不想安装桌面应用程序，则还可以在
  [铬黄](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} 浏览器。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} 是来自GraphQL Foundation的GraphQL IDE的实现。 这不是一个可安装的工具，而是一个您可以自己用来构建界面的软件包。
* 如果您已经熟悉 [Postman](https://www.postman.com/){target="_blank"}，它可很好地支持GraphQL查询，但功能不如专用的GraphQL客户端。

在GraphQL客户端中，您应该向URL路径提交请求 `/graphql` 在您的Adobe Commerce上或 [!DNL Magento Open Source] 实例。 如果您希望将现有实例用于测试，则可以使用Venia主题的演示(PWA Studio的示例实现)： `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
