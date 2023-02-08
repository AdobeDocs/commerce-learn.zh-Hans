---
title: 安装Adobe Developer IO命令行界面和API Mesh插件
description: 了解如何安装Adobe Developer IO命令行界面和API Mesh插件
landing-page-description: 了解如何使用Adobe应用程序生成器，并通过API Mesh插件安装Adobe Developer IO。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---


# 安装Adobe Developer IO和Mesh插件

在开始之前，需要设置以下几项内容。 首先，设置Adobe Developer IO命令行界面。 接下来，确保在每个环境中配置了API Mesh插件。
有关设置本地环境以运行Node、nvm和安装Adobe Developer IO的说明，请务必访问 [GraphQL Mesh快速入门](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## 这个视频给谁？

* 初次Adobe应用程序生成器或 [!DNL Magento Open Source] 具有有限的Adobe Developer IO和API Mesh使用经验。

## 视频内容

* API Mesh简介
* 安装Adobe Developer IO命令行界面
* 将API Mesh插件添加到AIO命令行

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## 使用NPM和AIO的示例命令

安装Adobe Developer命令行界面非常简单。 安装Node后，运行此命令 `npm install -g @adobe/aio-cli`
安装Adobe Developer cli后，即可安装Mesh插件。 要执行此操作，请运行此命令 `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}
