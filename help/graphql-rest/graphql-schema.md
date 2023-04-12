---
title: 使用GraphQL的模式语言
description: 了解与GraphQL相关的模式。 阅读架构的描述，以及一些有趣的模式和读取架构的方法。
landing-page-description: 这是对GraphQL的介绍。 了解架构以及如何解释某些元素
short-description: 这是对GraphQL的介绍。 了解架构以及如何解释某些元素
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

# 模式语言

使用的查询和突变依赖于在服务器上实现的特定数据图，GraphQL运行时会使用该数据图来解析查询。 GraphQL规范定义了一种不可知的语言，用于表达数据图的类型和关系。

下面是一个简化的类型架构，它支持您迄今所研究的查询和突变：

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

你可以深入研究 [GraphQL文档](https://graphql.org/learn/schema/){target="_blank"} 要了解类型系统的详细信息，包括此处未显示的某些概念的语法。 但是，上例不言自明。 （另外，请注意语法与查询语法有多相似。） 定义GraphQL架构只是表达给定类型的可用参数和字段以及这些字段的类型。 每个复杂的字段类型本身必须有一个定义，依此类推，通过树，直到您得到简单的标量类型，如 `String`.

的 `input` 声明在所有方面都像 `type` 但定义可用作参数输入的类型。 另请注意 `interface` 声明。 此函数与PHP中的接口大致相同。 其他类型将从此界面继承。

语法 `[CartItemInput!]!` 看起来很棘手，但最后却很直觉。 的 `!` _in_ 括号声明数组中的每个值都必须为非null，而 _外部_ 声明数组值本身必须为非空（例如，空数组）。

>[!NOTE]
>
>有关如何根据架构获取数据和设置其格式的逻辑，以及如何将此类逻辑映射到特定类型，取决于GraphQL运行时实施。 但是，根据对嵌套字段的了解，实施应遵循有意义的概念流程：与根关联的解析操作 `Query` 或 `Mutation` 类型，用于检查请求中指定的每个字段。 对于解析为复杂类型的每个字段，都会对该类型等执行类似的解析，直到所有内容都解析为标量值。

{{$include /help/_includes/graphql-rest-related-links.md}}
