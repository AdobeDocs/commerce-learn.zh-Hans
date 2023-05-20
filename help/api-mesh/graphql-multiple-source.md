---
title: 建立用於API網格的多重來源GraphQL
description: 瞭解如何在Adobe Commerce上為API Mesh使用多個來源，並 [!DNL Adobe App Builder]. 瞭解一些常見錯誤以及如何解決它們。
landing-page-description: 瞭解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 瞭解如何建立具有多個來源的網格，以及如何解決一些常見錯誤。
short-description: 瞭解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 瞭解如何建立具有多個來源的網格，以及如何解決一些常見錯誤。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 建立具有多個來源的網格

此影片可協助開發人員瞭解如何在Adobe Developer App Builder的API Mesh中建立具有多個來源的網格。 此影片說明如何建立具有多個來源的網格並識別錯誤。 如需詳細資訊和程式碼範例，請造訪 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 這部影片是給誰看的？

* 不熟悉API網狀架構的任何人
* 有意合併多個API和GraphQL來源的開發人員

## 視訊內容

* 使用方式 [轉換](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} 修改預設來源綱要
* 如何疑難排解錯誤，例如名稱衝突、結構描述可用性和其他結構描述語法問題
* 使用修改的組態更新您的網格

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## 建立json設定檔案

API Mesh使用JSON設定檔案來定義您的來源處理常式。 JSON檔案包含 `sources` 包含網格來源的陣列。 以下是具有多個來源的網格範例。

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      },
      {
        "name": "Example",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
