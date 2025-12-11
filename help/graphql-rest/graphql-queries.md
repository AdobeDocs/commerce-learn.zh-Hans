---
title: 使用GraphQL执行查询
description: 了解如何在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL执行查询。 以下是关于使用GET和POST调用的GraphQL的简介。
landing-page-description: 了解如何在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL执行查询。 以下是关于使用GET和POST调用的GraphQL的简介。
short-description: 了解如何在Adobe Commerce和 [!DNL Magento Open Source]上使用GraphQL执行查询。 以下是关于使用GET和POST调用的GraphQL的简介。
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 0%

---

# GraphQL查询

这是GraphQL和Adobe Commerce系列的第2部分。 在本教程和视频中，了解GraphQL查询以及如何针对Adobe Commerce执行这些查询。

>[!VIDEO](https://video.tv.adobe.com/v/3424120?learn=on)

## 本系列中GraphQL的相关视频和教程

* [第1部分GraphQL — 简介](../graphql-rest/intro-graphql.md)
* [第3部分GraphQL — 突变](../graphql-rest/graphql-mutations.md)
* [第4部分GraphQL — 架构](../graphql-rest/graphql-schema.md)

## GraphQL语法示例

让我们通过一个完整的示例来深入了解GraphQL查询语法。 (请记住，您可以对https://venia.magento.com/graphql尝试此操作。)

请观察以下GraphQL查询，该查询按件细分：

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

上述示例依赖于在服务器上定义的Adobe Commerce的现成GraphQL架构。 在此请求中，您
同时查询多种类型的数据。 查询准确地表示所需的字段，并且返回的数据将进行格式化
与查询本身类似。

>[!NOTE]
>
>GraphQL客户端会对发送的实际HTTP请求的形式进行模糊处理，但这很容易发现。 如果您使用基于浏览器的客户端，请在发送查询时观察[!UICONTROL Network]选项卡。 您看到请求包含由“查询： `{string}`”组成的原始正文，其中`{string}`只是整个查询的原始字符串。 如果请求作为GET发送，则查询可能会改用查询字符串参数“query”进行编码。 与REST不同，HTTP请求类型并不重要，只有关注查询的内容。


## 查询您想要的

本例中的`country`和`categories`表示两种不同的“查询”，适用于两种不同的数据。 与REST等传统API范例不同，REST将为每种数据类型定义单独和明确的端点。 GraphQL使您可以灵活地使用一次可获取多种类型数据的表达式来查询单个端点。

同样，查询准确地指定了`country` （`id`和`full_name_english`）和`categories`所需的字段（`items`，它本身具有字段的子选择），并且您接收的数据反映了该字段规范。 可能还有更多字段可用于这些数据类型，但您只能返回您请求的内容。


>[!NOTE]
>
>您可能会注意到`items`的返回值实际上是一个值为&#x200B;_数组_&#x200B;的值，但您还是直接为其选择子字段。 当字段的类型为列表时，GraphQL隐式了解要应用于列表中每个项目的子选择。

## 参数

虽然要返回的字段在每种类型的大括号中指定，但它们的命名参数和值在类型名称后面的括号中指定。 参数通常是可选的，并且经常会影响查询结果的筛选、格式化或以其他方式转换的方式。

您正在将`id`参数传递给`country`，指定要查询的特定国家/地区以及`filters`的`categories`参数。

## 字段一直向下对齐

虽然您可能倾向于将`country`和`categories`视为单独的查询或实体，但查询中表达的整个树实际上只包含字段。 `products`的表达式在语法上与`categories`的表达式没有区别。 两者都是领域，它们的构造没有区别。

任何GraphQL数据图都有一个“根”类型（通常称为`Query`）来启动树，并且通常被认为是实体的类型会被分配给此根上的字段。 示例查询实际上为根类型创建了一个泛型查询，并选择字段`country`和`categories`。 然后，它选择这些字段的子字段，依此类推，可能深入多个级别。 只要字段的返回类型是复杂类型（例如，具有自己的字段而不是标量类型的字段），请继续选择所需的字段。

嵌套字段的这个概念也是为什么可以像对顶级`products`字段一样为`pageSize` （`currentPage`和`categories`）传递参数的原因。

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

首先要注意的是，在查询的开头大括号前面添加了关键字`query`，以及操作名称(`getProducts`)。 此操作名称是任意的；它不对应于服务器架构中的任何内容。 添加此语法是为了支持引入变量。

在上一个查询中，您以字符串或整数的形式直接对字段的参数值进行硬编码。 但是，GraphQL规范提供一流的支持，可以使用变量将用户输入与主查询分开。

在新查询中，您在整个查询的开头大括号前使用圆括号定义`$search`变量（变量始终使用美元符号前缀语法）。 是此变量提供给`search`的`products`参数。

当查询包含变量时，GraphQL请求应在查询本身旁包含一个单独的JSON编码值字典。 对于上述查询，除了查询正文之外，您还可以发送以下JSON格式的变量值：

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>如果您针对Venia示例站点而不是您自己的Adobe Commerce实例尝试这些查询，则对于`related_products`，返回的结果可能为空。

在您用于测试的任何GraphQL感知型客户端（例如Altair和GraphiQL）中，UI支持与查询分开输入变量JSON。

正如您看到的GraphQL查询的实际HTTP请求正文中包含“query： `{string}`”一样，任何包含变量词典的请求都只是在同一正文中包含一个额外的“variables： `{json}`”，其中`{json}`是带有变量值的JSON字符串。

新查询还使用&#x200B;_片段_ (`productDetails`)在多个位置重复使用相同的字段选择。 [在GraphQL文档中阅读有关片段的更多信息](https://graphql.org/learn/queries/#fragments){target="_blank"}。

{{$include /help/_includes/graphql-rest-related-links.md}}
