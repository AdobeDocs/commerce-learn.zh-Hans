---
title: GraphQL简介
description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 对Adobe Commerce和 [!DNL Magento Open Source].
landing-page-description: 了解如何在Adobe Commerce上使用GraphQL和 [!DNL Magento Open Source]. 对Adobe Commerce和 [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: ef3dd7aaa409d9c1bc30d3d9c225966d8c1ace9e
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 0%

---

# GraphQL简介

GraphQL已迅速成为功能强大的客户端应用程序与后端进行交互的行业标准。 对于Adobe Commerce开发人员而言，这个主题越来越相关，因为该平台在无头实施领域不断扩展其功能。

如果您是GraphQL的新用户，此部分将介绍基本概念和用法。

## 什么是GraphQL?

GraphQL是唯一API查询语言和运行时的规范，用于响应该查询语言提供数据。

传统Web API（如REST）对于来回传递数据的不同系统来说非常有用，但对于诸如Progressive Web Application之类的现代应用程序链接体验而言，其性能低于峰值。 在此类应用程序中， _相同_ 应用程序通过web API进行通信。 REST等方案的程序化方法往往没有在需要快速获取多种数据的情况下提供适当的灵活性。

GraphQL允许客户以表达式描述 _完全_ 所需数据。 单个请求不需要多个网络请求来获取多种数据类型，而是可以查询多种类型。 而且，通过仅包含所请求的类型和字段（以直观地镜像查询的格式），响应会保持精益。

可以使用任何语言构建实施GraphQL规范的运行时。 Adobe Commerce和 [!DNL Magento Open Source] 使用
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP实施并在其上构建自己的层。

[查看完整的GraphQL文档](https://graphql.org/learn){target="_blank"}

## 使用GraphQL客户端

您需要GUI GraphQL客户端来测试代码示例和教程。 有以下几个选项：

* [阿尔泰尔](https://altairgraphql.dev/){target="_blank"} 是专为GraphQL构建的卓越且功能齐全的客户端。 Adobe在演示视频中使用Altair。
* 如果您不想安装桌面应用程序，则还有一些Altair扩展会直接在
   [铬黄](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} 浏览器。
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} 是GraphQL IDE在GraphQL Foundation中的实施。 这不是一个可安装的工具，而是一个可用于自行构建界面的包。
* 如果您已经熟悉 [Postman](https://www.postman.com/){target="_blank"}，但它对GraphQL查询有不错的支持，但其功能不如专门的GraphQL客户那样全面。

在GraphQL客户端中，您应将请求提交到URL路径 `/graphql` 在您的Adobe Commerce或 [!DNL Magento Open Source] 实例。 如果您希望将现有实例用于测试，则可以使用Venia主题演示(PWA Studio的示例实施): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
