---
title: 使用GraphQL执行查询
description: 了解如何在Adobe Commerce上使用GraphQL执行查询，以及 [!DNL Magento Open Source]. 以下是有关使用GET和POST调用的GraphQL的简介。
landing-page-description: 了解如何在Adobe Commerce上使用GraphQL执行查询，以及 [!DNL Magento Open Source]. 以下是有关使用GET和POST调用的GraphQL的简介。
short-description: 了解如何在Adobe Commerce上使用GraphQL执行查询，以及 [!DNL Magento Open Source]. 以下是有关使用GET和POST调用的GraphQL的简介。
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

# GraphQL查询

让我们通过一个完整的示例来深入研究GraphQL查询语法。 (请记住，您可以自行尝试使用https://venia.magento.com/graphql。)

请观察以下GraphQL查询，该查询将逐条划分：

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

GraphQL服务器对上述查询做出的可信响应可能是：

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

上例依赖于服务器上定义的适用于Adobe Commerce的现成GraphQL架构。 在此请求中，您可以一次查询多种类型的数据。 查询完全表示您需要的字段，并且返回的数据的格式与查询本身类似。

>[!NOTE]
>
>GraphQL客户端会模糊处理正在发送的实际HTTP请求的形式，但这很容易发现。 如果您使用的是基于浏览器的客户端，请观察 [!UICONTROL Network] 选项卡。 您会看到请求包含由“query: `{string}`&quot;，其中 `{string}` 只是整个查询的原始字符串。 如果请求作为GET发送，则查询可能会改为使用查询字符串参数“query”进行编码。 与REST不同，HTTP请求类型无关紧要，只涉及查询的内容。


## 查询所需内容

`country` 和 `categories` 在此示例中，对于两种不同的数据，表示两个不同的“查询”。 与传统API范式（如REST）不同，REST将为每个数据类型定义单独且明确的端点。 通过GraphQL，您可以灵活地使用表达式查询单个端点，该表达式可同时获取多种类型的数据。

同样，查询会准确地指定两者所需的字段 `country` (`id` 和 `full_name_english`)和 `categories` (`items`，该字段本身包含字段的子选项)，并且您收到的数据反映了该字段规范。 这些数据类型可能有更多的字段可用，但您只会返回所请求的内容。


>[!NOTE]
>
>您可能会注意到 `items` 实际上 _阵列_ ，但您仍直接为其选择子字段。 当字段类型为列表时，GraphQL会隐式理解子选项以应用于列表中的每个项目。

## 参数

虽然要返回的字段在每种类型的大括号中指定，但其命名参数和值在类型名称后面的括号中指定。 参数通常是可选的，并且通常会影响查询结果的过滤、格式设置或转换方式。

您正在传递 `id` 参数 `country`，指定要查询的特定国家/地区，以及 `filters` 参数 `categories`.

## 一路向下

你可能会想 `country` 和 `categories` 作为单独的查询或实体，在查询中表示的整个树实际上只包含字段。 的表达式 `products` 在语法上与 `categories`. 两者都是字段，它们的构造没有区别。

任何GraphQL数据图都具有单个“根”类型(通常称为 `Query`)来启动树，且通常被视为实体的类型会分配给此根上的字段。 示例查询实际上是对根类型进行一个通用查询并选择字段 `country` 和 `categories`. 然后，选择这些字段的子字段，等等，可能是多个级别的深度。 只要字段的返回类型是复杂类型（例如，具有其自己字段的类型，而不是标量类型的类型），请继续选择所需的字段。

嵌套字段的这一概念也解释了为什么您可以为 `products` (`pageSize` 和 `currentPage`)的方式与对顶级 `categories` 字段。

![GraphQL字段树](../assets/graphql-field-tree.png)

## 变量

让我们尝试一个不同的查询：

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

首先要注意的是添加了关键词 `query` 在查询的左大括号之前，以及操作名称(`getProducts`)。 此操作名称是任意的；它与服务器架构中的任何内容都不对应。 添加了此语法以支持引入变量。

在上一个查询中，您直接将字段参数的硬编码值作为字符串或整数。 但是，GraphQL规范具有一流的支持，可使用变量将用户输入与主查询分离。

在新查询中，您使用整个查询左大括号前的圆括号来定义 `$search` 变量（变量始终使用美元符号前缀语法）。 提供给的是此变量 `search` 参数 `products`.

当查询包含变量时，GraphQL请求应该与查询本身一起包含一个单独的JSON编码值字典。 对于上述查询，除了查询正文外，您还可以发送以下变量值JSON:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您针对Venia示例网站而不是您自己的Adobe Commerce实例尝试这些查询，则返回的结果可能为空 `related_products`.

在您用于测试的任何GraphQL感知型客户端（例如Altair和GraphiQL）中，UI支持分别输入变量JSON和查询。

正如您看到的GraphQL查询的实际HTTP请求包含“query: `{string}`“”在其正文中，任何包含变量词典的请求都只包含额外的“变量： `{json}`&quot; `{json}` 是包含变量值的JSON字符串。

新查询还使用 _片段_ (`productDetails`)以在多个位置重复使用同一字段选择。 [阅读有关片段的更多信息](https://graphql.org/learn/queries/#fragments){target="_blank"} (在GraphQL文档中)。

{{$include /help/_includes/graphql-rest-related-links.md}}
