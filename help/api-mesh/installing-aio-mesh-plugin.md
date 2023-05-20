---
title: 安裝Adobe I/O Runtime命令列介面和API網狀外掛程式
description: 瞭解如何安裝Adobe I/O Runtime命令列介面和API Mesh外掛程式
landing-page-description: 瞭解如何使用AdobeApp Builder並安裝Adobe I/O Runtime與API Mesh外掛程式。
short-description: 瞭解如何使用AdobeApp Builder並安裝Adobe I/O Runtime與API Mesh外掛程式。
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# 安裝Adobe I/O Runtime CLI和Mesh外掛程式

開始使用Adobe Developer App Builder的API Mesh之前，您需要安裝 `aio` CLI和API網狀外掛程式。
如需安裝指示和先決條件，請造訪API網格 [快速入門](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} 頁面。

## 這部影片是給誰看的？

* 不熟悉API Mesh的開發人員或 [!DNL Adobe Commerce] 使用經驗有限 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} 和API網格。

## 視訊內容

* API Mesh簡介
* 安裝Adobe I/O Runtime CLI （命令列介面）
* 安裝API網狀外掛程式

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## 安裝 `aio` CLI和API網狀外掛程式

安裝後 `node` 和 `npm`，執行以下命令以安裝 `aio` CLI：

```bash
npm install -g @adobe/aio-cli
```

安裝Adobe I/O Runtime CLI後，請使用下列命令來安裝API Mesh外掛程式：

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
