---
title: 安装Adobe I/O Runtime命令行界面和API Mesh插件
description: 了解如何安装Adobe I/O Runtime命令行界面和API Mesh插件
landing-page-description: 了解如何使用Adobe应用程序生成器，并使用API Mesh插件安装Adobe I/O Runtime。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: f365e4cbf3f9bd8a0364acb9d28f8d9479809011
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# 安装Adobe I/O Runtime CLI和Mesh插件

在开始为Adobe Developer App Builder使用API Mesh之前，您需要先安装 `aio` CLI和API Mesh插件。
有关安装说明和先决条件，请访问API网格 [入门指南](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} 页面。

## 这个视频给谁？

* 不熟悉API Mesh或 [!DNL Adobe Commerce] 使用 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} 和API网格。

## 视频内容

* API Mesh简介
* 安装Adobe I/O Runtime CLI（命令行界面）
* 安装API Mesh插件

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## 安装 `aio` CLI和API Mesh插件

安装后 `node` 和 `npm`，运行以下命令以安装 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

安装Adobe I/O Runtime CLI后，使用以下命令安装API Mesh插件：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
