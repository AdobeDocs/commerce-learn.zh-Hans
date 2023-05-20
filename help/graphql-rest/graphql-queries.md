---
title: 使用GraphQL執行查詢
description: 瞭解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 以下為使用GET和POST呼叫的GraphQL簡介。
landing-page-description: 瞭解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 这是使用 GET 和 POST 调用的 GraphQL 简介。
short-description: 瞭解如何在Adobe Commerce上使用GraphQL執行查詢，以及 [!DNL Magento Open Source]. 以下為使用GET和POST呼叫的GraphQL簡介。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# GraphQL查詢

以完整的範例來直接探討GraphQL查詢語法。 (請記住，您可以針對https://venia.magento.com/graphql自行嘗試此做法。)

請注意下列GraphQL查詢，此查詢會依件細分：

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

来自上述查询的 GraphQL 服务器的 plausible 响应可能是：

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

以上示例依赖于在服务器上定义的 Adobe Systems 商务的现成 GraphQL 模式。 在此请求中，您
一次查询多个类型的数据。 查询将准确表达您需要的字段，并将返回的数据设置为
与查询本身类似。

>[!NOTE]
>
>GraphQL 客户会对所发送的实际 HTTP 请求的形式进行模糊处理，但这很容易发现。 如果您使用的是基于浏览器的客户端，请在发送查询时观察 [!UICONTROL Network] 选项卡。 您會看到請求包含原始內文，其中包括「query： `{string}`&quot;，其中 `{string}` 只是整個查詢的原始字串。 如果請求是以GET形式傳送，則查詢可能會改以查詢字串引數&quot;query&quot;編碼。 與REST不同，HTTP要求型別並不重要，只關乎查詢的內容。


## 查询所需的内容

`country``categories`对于两种不同的数据，该示例表示两个不同的 &quot;查询&quot;。与传统的 API 模式点赞 REST 不同，这将为每个数据类型定义单独和明确的端点。 GraphQL 可让您灵活地查询单个端点与可以一次获取多种类型数据的表达式。

同样，查询指定确切需要的 `country` 字段（ `id` and `full_name_english` ）和 `categories` （ `items` ，后者本身包含字段的一部分），而您收到的数据则是字段规范的镜像。 这些数据类型可能有很多可用字段，但您只返回您请求的内容。


>[!NOTE]
>
>您可能会注意到，的返回值 `items` 实际上是一个 _值数组_ ，但您却要直接为其选择子字段。 當欄位的型別為清單時，GraphQL會隱含地瞭解要套用至清單中每個專案的子選取範圍。

## 引數

雖然您要傳回的欄位是在每個型別的大括弧內指定，但已命名的引數以及它們的值是在型別名稱后的括弧內指定。 引數通常是選用的，通常會影響查詢結果的篩選、格式化或以其他方式轉換的方式。

您将向 `country` 传递 `id` 参数，指定要查询的特定国家/地区和 `filters` 参数 `categories` 。

## 字段一直向下

雖然您可能會考慮 `country` 和 `categories` 作為單獨的查詢或實體，查詢中表達的整個樹實際上只包含欄位。 的運算式 `products` 在語法上與以下專案並無不同 `categories`. 兩者都是欄位，其結構之間沒有差異。

任何GraphQL資料圖表都有單一「根」型別(通常稱為 `Query`)，以啟動樹狀結構，且通常視為實體的型別會指派給此根目錄上的欄位。 範例查詢實際上是對根型別進行一般查詢並選取欄位 `country` 和 `categories`. 然後它會選取這些欄位的子欄位，依此類推，可能深入數個層級。 只要欄位的傳回型別是複雜型別（例如，具有自己欄位的型別，而不是純量型別），請繼續選取您想要的欄位。

此巢狀欄位的概念也是您傳遞引數的原因 `products` (`pageSize` 和 `currentPage`)，其方式與您在頂層相同 `categories` 欄位。

![GraphQL 字段树](../assets/graphql-field-tree.png)

## 变量

請嘗試不同的查詢：

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

首先要注意的是，已新增關鍵字 `query` 在查詢的開頭大括弧之前，加上作業名稱(`getProducts`)。 此作業名稱是任意的；它未對應到伺服器結構描述中的任何內容。 新增此語法以支援變數的引入。

在上一個查詢中，您直接以字串或整數的形式硬式編碼欄位引數值。 但是，GraphQL 规范具有从主查询使用变量分隔用户输入的第一类支持。

在新查询中，您在整个查询的左大括号之前使用括号来定义 `$search` 变量（变量始终使用美元符号前缀语法）。 这是为提供给 `search` 参数的 `products` 变量。

当查询包含变量时，GraphQL 请求应包含单独的 JSON 编码的值字典，与查询本身一起使用。 对于上面的查询，除了查询体之外，您可能会发送以下变量值 JSON：

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您要针对 Venia 示例网站而不是您自己的 Adobe Systems 商务实例尝试这些查询，则返回的结果可能为空 `related_products` 。

在用于测试的任何 GraphQL 识别客户端（如 Altair 和 GraphiQL）中，UI 都支持从查询中单独输入变量 JSON。

正如您看到的，GraphQL查詢的實際HTTP請求包含「query： `{string}`」在其內文中，任何包含變數字典的請求僅包括額外的「變數： `{json}`」在相同內文中，其中 `{json}` 是包含變數值的JSON字串。

新查詢也使用 _片段_ (`productDetails`)，以便在多個位置重複使用相同的欄位選取範圍。 [深入瞭解片段](https://graphql.org/learn/queries/#fragments){target="_blank"} (位於GraphQL檔案中)。

{{$include /help/_includes/graphql-rest-related-links.md}}
