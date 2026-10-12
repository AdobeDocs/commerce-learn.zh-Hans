---
title: 使用项目和工作区
description: 了解如何使用Adobe Developer Console处理API Mesh的项目和工作区，包括添加网格、通过CLI登录和切换组织。
jira: KT-11803
doc-type: Tutorial
duration: 572
last-substantial-update: 2026-08-03T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: ab51f68c-5d28-495b-8472-27b60c4aa8c1
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%
---
# 使用项目和工作区

本教程讨论如何创建项目和工作区以包含网格。 此工作主要在[Adobe Developer控制台](https://developer.adobe.com/console){target="_blank"} UI中进行。

## 此视频面向谁？

* 有权访问Adobe Developer Console帐户并想要创建项目和工作区的开发人员。

## 视频内容

* 在Adobe Developer Console中使用项目和工作区
* 在Adobe Developer Console中将API网格添加到工作区
* 在CLI中登录Adobe Developer控制台
* 在CLI中查看选定的项目和工作区
* 在CLI中更改选定的组织、项目或工作区
* 测试简单的API网格命令

>[!VIDEO](https://video.tv.adobe.com/v/3414123?learn=on)

## 了解Adobe Developer Console

在Adobe Developer Console中，以下层次结构描述了您的API在您的组织中的组织方式： `Organization > Project > Workspace > [API]`。 要了解有关Adobe App Builder、登录到控制台和基本疑难解答的更多信息，请访问[创建您的第一个App Builder应用程序](https://developer.adobe.com/app-builder/docs/get_started/app_builder_get_started/first-app){target="_blank"}。

## Adobe Developer Console中的项目简介

Adobe Developer Console中的所有开发工作都会作为项目的一部分执行。 项目可以包含一个或多个产品以及API、事件、运行时和插件的组合。 有关Adobe Developer控制台中项目的更多详细信息，请访问[项目](https://developer.adobe.com/developer-console/docs/guides/projects/){target="_blank"}。

有关在API Mesh上下文中使用项目和工作区的详细信息，请参阅[修改项目和工作区](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}。

{{$include /help/_includes/api-mesh-related-links.md}}
