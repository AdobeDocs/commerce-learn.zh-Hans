---
title: 使用GraphQL執行變異
description: 瞭解如何在Adobe Commerce上使用GraphQL執行變異，以及 [!DNL Magento Open Source]. 使用POST呼叫執行您的第一個變異。
landing-page-description: 瞭解如何在Adobe Commerce上使用GraphQL執行變異，以及 [!DNL Magento Open Source]. 使用POST呼叫執行您的第一個變異。
short-description: 瞭解如何在Adobe Commerce上使用GraphQL執行變異，以及 [!DNL Magento Open Source]. 使用POST呼叫執行您的第一個變異。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 突變

任何完整的API規格不僅需要提供查詢資料的能力，還需要提供建立和更新資料的能力。

REST會區分變更資料的請求與不符合請求型別或「動詞」(GET與POST或PUT)的請求。
使用GraphQL時，資料修改查詢的區別在於 `mutation` 與伺服器上定義之結構描述中的不同根型別對應的關鍵字。

檢視這個將產品新增到使用者購物車的變異範例。 (這需要為登入客戶的工作階段產生的購物車ID，或是使用 `createEmptyCart` 突變)。

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

您可以想像上述變異會連同下列變數字典一起在要求中傳送：

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

最後，您可能會收到如下的回應：

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

關於上述範例，需要注意的主要事項是，除了使用 `mutation` 關鍵字而非 `query`，語法與查詢相同。 和查詢一樣，變異包括：

* 任意操作名稱(`doAddToCart`)
* 變數清單(例如， `$cartId`)
* 初始欄位(`addProductsToCart`)與引數(例如， `cartId`，設定為的值 `$cartId`)括弧中
* 大括弧中的欄位細選

欄位子選項可讓您靈活定義想要傳回的欄位（從指派為傳回值的型別） `addProductsToCart` - `AddProductsToCartOutput`)完成變異後。

如前所述，GraphQL結構描述中定義的欄位從查詢的根型別開始(通常稱為 `Query`)。 同樣地，變異也存在另一種根型別(通常稱為 `Mutation`)。 `addProductsToCart` 是該根型別上的欄位。

關於上述範例的其他幾點備註：

* 此 `!` 字元尾碼 `String` 和 `CartItemInput` 表示變數為必要專案。
* 方括弧(`[]`)周圍 `CartItemInput` 為指定的型別 `$cartItems` 表示該型別的清單，而不是單一值。

{{$include /help/_includes/graphql-rest-related-links.md}}
