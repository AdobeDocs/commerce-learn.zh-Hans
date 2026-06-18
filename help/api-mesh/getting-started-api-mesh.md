---
title: API Mesh入门
description: 了解如何在Adobe Commerce和Adobe App Builder上使用API Mesh，包括安装App Builder、处理项目和创建反向代理。
jira: KT-11802
doc-type: Tutorial
duration: 422
last-substantial-update: 2023-08-27T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# API Mesh入门

如果您不熟悉Adobe Developer App Builder的API Mesh，Adobe建议先从本介绍性教程开始，然后再转到其他视频和教程。

## 什么是API Mesh

API Mesh将多个数据源整合在一起，得到单个响应以供应用程序使用。

[查看完整的API网格文档](https://developer.adobe.com/graphql-mesh-gateway/mesh/){target="_blank"}

## 此视频面向谁？

* 刚开始使用API Mesh或[!DNL Adobe Commerce]但使用[Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"}和API Mesh经验有限的任何开发人员。

## 视频内容

* API Mesh概述
* 补充文档链接
* 在结账时执行实时清单检查的用例
* 将开发工作和资源使用从您的商务应用程序中移开

>[!VIDEO](https://video.tv.adobe.com/v/3421884?captions=chi_hans&learn=on)

## 示例用例

您的Commerce应用程序具有REST API和GraphQL端点。 例如，使用REST API应用特殊定价或GraphQL端点处理库存状态。 使用API网格，您可以定义两个端点，检索信息，并将其作为一次响应返回到请求应用程序。

## 什么是反向代理

作为使用Adobe App Builder和API Mesh的开发人员，无需了解反向代理的定义。 但是，如果您对与Adobe App Builder相关的整体功能感兴趣，请使用以下资源：

* [什么是反向代理](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}


{{$include /help/_includes/api-mesh-related-links.md}}
