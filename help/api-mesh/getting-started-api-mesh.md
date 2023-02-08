---
title: API Mesh入门
description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解有关安装Adobe Developer、处理项目、创建图形反向代理等的信息。
landing-page-description: 了解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 了解如何安装AdobeIO、处理项目、创建图形反向代理等。
kt: 11802
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# API Mesh入门

如果您是初次使用API Mesh，则Adobe建议您先学习此介绍性教程，然后再深入学习随附的视频和教程。

## 什么是API Mesh

API Mesh允许使用多个数据源来提供供商务应用程序使用的单个响应。

[查看完整的API Mesh文档](https://developer.adobe.com/graphql-mesh-gateway/gateway/overview/)

## 用例示例

您的商务应用程序的后端系统具有一个REST API，您可以使用该API获取特价。 另一个具有GraphQL端点的后端系统会处理库存状态。 使用API Mesh，您可以定义两个端点，检索信息，并将其作为一个响应返回到请求应用程序。

## 什么是反向代理

作为使用Adobe应用程序生成器和GraphQL Mesh的开发人员，您无需了解反向代理是什么。 但是，您可能对整体功能感兴趣，因为它与Adobe应用程序生成器相关。 互联网上有很多好的资源。
有关反向代理基本功能的更多信息，请参阅以下几个外部资源：

* [什么是反向代理](https://www.imperva.com/learn/performance/reverse-proxy/)
* [什么是反向代理，为什么这很重要](https://blog.hubspot.com/website/reverse-proxy)

{{$include /help/_includes/api-mesh-related-links.md}}
