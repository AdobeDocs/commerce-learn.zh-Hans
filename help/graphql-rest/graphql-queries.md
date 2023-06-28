---
title: 使用GraphQL执行查询
description: 了解如何在Adobe Commerce中使用GraphQL执行查询，并且 [!DNL Magento Open Source]. 以下介绍如何使用GET和POST调用的GraphQL。
landing-page-description: 了解如何在Adobe Commerce中使用GraphQL执行查询，并且 [!DNL Magento Open Source]. 以下介绍如何使用GET和POST调用的GraphQL。
short-description: 了解如何在Adobe Commerce中使用GraphQL执行查询，并且 [!DNL Magento Open Source]. 以下介绍如何使用GET和POST调用的GraphQL。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# GraphQL查询

让我们通过一个完整的示例来深入了解GraphQL查询语法。 (请记住，您可以自己对https://venia.magento.com/graphql尝试此操作。)

请观察以下GraphQL查询，该查询逐条细分：

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

GraphQL服务器对上述查询的合理响应可能是：

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

上述示例依赖于在服务器上定义的Adobe Commerce的现成GraphQL架构。 在此请求中，您一次查询多种类型的数据。 查询准确地表示所需的字段，返回数据的格式与查询本身类似。

>[!NOTE]
>
>GraphQL客户端会对实际发送的HTTP请求的形式进行模糊处理，但这很容易发现。 如果您使用的是基于浏览器的客户端，请遵守 [!UICONTROL Network] 选项卡。 您会看到请求包含一个由“query： `{string}`“”，其中 `{string}` 只是整个查询的原始字符串。 如果请求是以GET形式发送，则查询可能会改为在查询字符串参数“query”中进行编码。 与REST不同，HTTP请求类型并不重要，只关乎查询的内容。


## 查询您需要的内容

`country` 和 `categories` 在此示例中，针对两种不同类型的数据，表示两个不同的“查询”。 与REST等传统API范例不同，后者将为每种数据类型定义单独和明确的端点。 GraphQL使您可以灵活地使用表达式查询单个端点，该表达式可以一次获取多种类型的数据。

同样，查询也恰好指定了这两个字段所需的字段 `country` (`id` 和 `full_name_english`)和 `categories` (`items`，它本身具有字段的子选项)，并且您接收的数据会镜像该字段规范。 可能还有更多字段可用于这些数据类型，但您只能返回您请求的内容。


>[!NOTE]
>
>您可能会注意到 `items` 实际上是 _数组_ ，但您仍会为其直接选择子字段。 当字段的类型为列表时，GraphQL隐式了解要应用于列表中每个项目的子选择。

## 参数

虽然要返回的字段是在每个类型的大括号内指定的，但命名参数及其值是在类型名称后的括号内指定的。 参数通常是可选的，并且通常会影响查询结果的筛选、格式化或以其他方式转换的方式。

您正在传递 `id` 参数 `country`，指定要查询的特定国家/地区，以及 `filters` 参数 `categories`.

## 字段一直向下对齐

虽然你可能会想到 `country` 和 `categories` 作为单独的查询或实体，查询中表示的整个树实际上只包含字段。 表达式 `products` 在语法上与 `categories`. 两者都是字段，其结构之间没有区别。

任何GraphQL数据图都具有单个“根”类型(通常称为 `Query`)，以启动树，通常被认为是实体的类型会分配给此根目录上的字段。 示例查询实际上是为根类型创建一个通用查询并选择字段 `country` 和 `categories`. 然后，它选择这些字段的子字段，依此类推，可能深入多个级别。 只要字段的返回类型是复杂类型（例如，具有自己字段而不是标量类型的类型），请继续选择所需的字段。

嵌套字段的概念也是您传递参数的原因 `products` (`pageSize` 和 `currentPage`)，其方式与您在顶层执行的操作相同 `categories` 字段。

![GraphQL字段树](../assets/graphql-field-tree.png)

## 变量

让我们尝试其他查询：

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

首先要注意的是，添加了关键字 `query` 在查询的开头大括号之前，以及操作名称(`getProducts`)。 该操作名是任意的；它不对应于服务器架构中的任何内容。 添加此语法是为了支持引入变量。

在上一个查询中，您直接对字段的参数值进行硬编码，以字符串或整数的形式。 但是，GraphQL规范对于使用变量将用户输入与主查询分隔开提供一流支持。

在新查询中，使用括号位于整个查询的开头大括号前以定义 `$search` 变量（变量始终使用美元符号前缀语法）。 是此变量提供给 `search` 参数 `products`.

当查询包含变量时，GraphQL请求应随查询本身一起包含一个单独的JSON编码值字典。 对于上述查询，除了查询正文之外，您还可以发送以下JSON形式的变量值：

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您是针对Venia示例网站(而不是您自己的Adobe Commerce实例)尝试这些查询，则返回的结果可能为空 `related_products`.

在您用于测试的任何GraphQL感知客户端（例如Altair和GraphiQL）中，UI支持与查询分开输入变量JSON。

正如您看到的，GraphQL查询的实际HTTP请求包含“query： `{string}`”在其正文中，任何包含变量词典的请求仅包含一个额外的“变量： `{json}`”在同一个体内，其中 `{json}` 是包含变量值的JSON字符串。

新查询还使用 _片段_ (`productDetails`)，以便在多个位置重复使用相同的字段选择。 [阅读有关片段的更多信息](https://graphql.org/learn/queries/#fragments){target="_blank"} 在GraphQL文档中。

{{$include /help/_includes/graphql-rest-related-links.md}}
