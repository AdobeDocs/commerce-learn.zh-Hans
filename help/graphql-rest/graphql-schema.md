---
title: GraphQL的結構描述語言
description: 瞭解GraphQL的相關結構描述。 閱讀結構描述，以及一些有趣的模式和讀取結構描述的方法。
landing-page-description: 以下是GraphQL的簡介。 瞭解結構以及如何解譯部分元素
short-description: 以下是GraphQL的簡介。 瞭解結構以及如何解譯部分元素
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# 結構描述語言

使用的查詢和變異取決於在伺服器上實作的特定資料圖表，GraphQL執行階段會使用這個圖表並用來解析查詢。 GraphQL規格定義了一種不可知的語言，用於表示資料圖表的型別和關係。

以下是支援您目前所瞭解之查詢和變異的縮寫型別結構描述：

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

您可以深入瞭解 [GraphQL檔案](https://graphql.org/learn/schema/){target="_blank"} 瞭解型別系統的詳細資訊，包括此處未說明的一些概念的語法。 然而，上述範例不言自明。 （此外，請注意語法與查詢語法的相似程度。） 定義GraphQL結構描述只是表示給定型別的可用引數和欄位，以及這些欄位的型別的問題。 每個複雜欄位型別本身都必須有定義，依此類推，透過樹狀結構，直到您取得簡單的純量型別，例如 `String`.

此 `input` 宣告在所有方面與 `type` 但會定義可以做為引數輸入的型別。 另請注意 `interface` 宣告。 此功能與PHP中的介面大致相同。 其他型別則繼承自此介面。

語法 `[CartItemInput!]!` 看起來很棘手，但最後還是相當直覺的。 此 `!` _裡面_ 方括弧會宣告陣列中的每個值都必須非空值，而 _外部_ 宣告陣列值本身必須為非空值（例如，空陣列）。

>[!NOTE]
>
>如何根據結構描述擷取及格式化資料，以及如何將這類邏輯對應至特定型別的邏輯，取決於GraphQL執行階段實施。 不過，根據對巢狀欄位的瞭解，實施應遵循合理的概念流程：與根關聯的解決操作 `Query` 或 `Mutation` 執行型別，檢查請求中指定的每個欄位。 對於每個解析為複雜型別的欄位，會對該型別執行類似的解析，以此類推，直到所有內容都解析為純量值為止。

{{$include /help/_includes/graphql-rest-related-links.md}}
