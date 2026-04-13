---
title: 安装Adobe I/O Runtime命令行界面和API Mesh插件
description: 了解如何安装Adobe I/O Runtime命令行界面和API Mesh插件
jira: KT-11801
doc-type: Tutorial
duration: 433
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 003d55eac7e13a02ee633bed5ea9ab98825db151
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# 安装Adobe I/O Runtime CLI和Mesh插件

在开始为Adobe Developer App Builder使用API Mesh之前，您需要安装`aio` CLI和API Mesh插件。
有关安装说明和先决条件，请访问API Mesh [快速入门](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"}页面。

## 此视频面向谁？

* 刚开始使用API Mesh或[!DNL Adobe Commerce]但使用[Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"}和API Mesh经验有限的开发人员。

## 视频内容

* API Mesh简介
* 安装Adobe I/O Runtime CLI（命令行界面）
* 安装API Mesh插件

>[!VIDEO](https://video.tv.adobe.com/v/3414122?learn=on)

## 安装`aio` CLI和API Mesh插件

安装`node`和`npm`后，请运行以下命令以安装`aio` CLI：

```bash
npm install -g @adobe/aio-cli
```

安装Adobe I/O Runtime CLI后，请使用以下命令安装API Mesh插件：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
