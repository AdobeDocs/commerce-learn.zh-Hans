---
title: 安装Adobe I/O Runtime CLI和API Mesh插件
description: 了解如何安装Adobe I/O Runtime命令行界面和API Mesh插件，以便开始使用适用于Adobe Developer App Builder的API Mesh。
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
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
source-wordcount: '182'
ht-degree: 0%
---
# 安装Adobe I/O Runtime CLI和Mesh插件

在开始为Adobe Developer App Builder使用API Mesh之前，您需要安装`aio` CLI和API Mesh插件。
有关安装说明和先决条件，请访问API Mesh [快速入门](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"}页面。

## 此视频面向谁？

* 刚开始使用API Mesh或[!DNL Adobe Commerce]但使用[Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"}和API Mesh经验有限的开发人员。

## 视频内容

* API Mesh简介
* 安装Adobe I/O Runtime CLI（命令行界面）
* 安装API Mesh插件

>[!VIDEO](https://video.tv.adobe.com/v/3414122?learn=on)

## 安装`aio` CLI和API Mesh插件

要安装`aio` CLI，请在安装`node`和`npm`后运行以下命令：

```bash
npm install -g @adobe/aio-cli
```

安装Adobe I/O Runtime CLI后，请使用以下命令安装API Mesh插件：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
