---
title: GraphQL的架构语言
description: 了解GraphQL中涉及的架构。 阅读架构的说明，以及一些有趣的模式和读取架构的方法。
landing-page-description: 以下是GraphQL的简介。 了解架构以及如何解释某些元素
short-description: 以下是GraphQL的简介。 了解架构以及如何解释某些元素
kt: 13939
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---

# 架构语言

这是GraphQL和Adobe Commerce系列的第4部分。 使用的查询和突变依赖于在服务器上实现的特定数据图，GraphQL运行时将使用这些数据图解析查询。 GraphQL规范定义了一种不可知语言，用于表示数据图的类型和关系。

>[!VIDEO](https://video.tv.adobe.com/v/3446620?learn=on&captions=chi_hans)

## 本系列中GraphQL的相关视频和教程

* [第1部分GraphQL — 简介](../graphql-rest/intro-graphql.md)
* [第2部分GraphQL — 查询](../graphql-rest/graphql-queries.md)
* [第3部分GraphQL — 突变](../graphql-rest/graphql-mutations.md)

## 模式示例

下面是一个缩写型架构，它支持您目前所查看的查询和突变：

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

您可以深入了解[GraphQL文档](https://graphql.org/learn/schema/){target="_blank"}以了解类型系统的详细信息，包括此处未表示的一些概念的语法。 然而，上述例子不言自明。 （此外，请注意语法与查询语法的相似性。） 定义GraphQL架构只是表示给定类型的可用参数和字段以及这些字段的类型的问题。 每个复杂字段类型本身必须有一个定义，以此类推，通过树，直到您获得简单的标量类型（如`String`）。

`input`声明在所有方面都与`type`类似，但定义了一个可以用作参数输入的类型。 另请注意`interface`声明。 此功能与PHP中的接口大致相同。 其他类型继承自此接口。

语法`[CartItemInput!]!`看起来很棘手，但最后还是相当直观的。 `!` _内部_&#x200B;括号声明数组中的每个值都必须为非null，而&#x200B;_外部_&#x200B;括号声明数组值本身必须为非null（例如，空数组）。

>[!NOTE]
>
>有关如何根据架构获取和格式化数据以及如何将此逻辑映射到特定类型的逻辑，可取决于GraphQL运行时实施。 但是，实施应遵循一个概念流程，根据对嵌套字段的理解来制定相应的流程：执行与根`Query`或`Mutation`类型关联的解析操作，以检查请求中指定的每个字段。 对于每个解析为复杂类型的字段，将对该类型进行类似的解析，依此类推，直到所有内容都已解析为标量值。

{{$include /help/_includes/graphql-rest-related-links.md}}
