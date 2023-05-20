---
title: 在API網格中建立GraphQL單一來源網格
description: 瞭解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 瞭解如何建立具有一個來源的網格。
landing-page-description: 瞭解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 瞭解如何建立具有一個來源的網格。
short-description: 瞭解如何在Adobe Commerce上使用API Mesh和 [!DNL Adobe App Builder]. 瞭解如何建立具有一個來源的網格。
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# 使用單一來源建立網格

此影片可協助開發人員瞭解如何在Adobe Developer App Builder的API Mesh中使用單一來源建立網格。 為了讓此基本範例按預期運作，您需要可公開存取的API或GraphQL端點。 此影片也說明如何建立簡易的 `mesh.json` 要與您的Commerce執行個體一起使用的檔案。 如需詳細資訊和程式碼範例，請造訪 [建立網格](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 這部影片是給誰看的？

* 任何不熟悉API網狀架構的人
* 有意合併多個GraphQL和API來源的開發人員
* 需要瞭解如何篩選網路索引標籤及由GraphQL篩選的任何人

## 視訊內容

* 使用API網格作為反向Proxy
* 從JSON設定檔案建立網格
* 存取新建立的GraphQL端點

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## 建立json設定檔案

API Mesh使用JSON設定檔案來定義您的來源處理常式。 JSON檔案包含 `sources` 包含網格來源的陣列。 以下是單一來源的網格範例。

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
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
