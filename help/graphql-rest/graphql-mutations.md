---
title: 使用GraphQL执行突变
description: 了解有关在Adobe Commerce上使用GraphQL执行突变的简介 [!DNL Magento Open Source]. 使用POST调用执行您的第一个突变。
landing-page-description: 了解有关在Adobe Commerce上使用GraphQL执行突变的简介 [!DNL Magento Open Source]. 使用POST调用执行您的第一个突变。
short-description: 了解有关在Adobe Commerce上使用GraphQL执行突变的简介 [!DNL Magento Open Source]. 使用POST调用执行您的第一个突变。
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 突变

任何完整的API规范不仅需要提供查询数据的能力，还需要提供创建和更新数据的能力。

REST将区分更改数据的请求和不具有请求类型或“动词”(GET与POST或PUT)的请求。
使用GraphQL时，数据修改查询的区别在于 `mutation` 与服务器上定义的模式中的不同根类型对应的关键字。

查看此示例突变以将产品添加到用户购物车。 （这需要为登录客户的会话或使用生成的购物车ID） `createEmptyCart` 突变。)

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

您可以设想在请求中发送上述突变以及以下变量词典：

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

最后，您可能会收到如下响应：

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

关于上述示例，需要注意的主要事项是， `mutation` 关键字而不是 `query`，语法与查询相同。 与查询类似，变异包括：

* 任意操作名称(`doAddToCart`)
* 变量列表(例如， `$cartId`)
* 初始字段(`addProductsToCart`)和参数(例如， `cartId`，设置为的值 `$cartId`)在括号中
* 大括号中的字段的部分选择

字段子选择允许您灵活定义希望返回的字段(从指定为 `addProductsToCart` - `AddProductsToCartOutput`)。

如前所述，GraphQL架构中定义的字段从查询的根类型开始(通常称为 `Query`)。 同样，另一种根类型存在突变(通常称为 `Mutation`)。 `addProductsToCart` 是该根类型上的字段。

有关上述示例的其他几点注意事项：

* 此 `!` 字符后缀 `String` 和 `CartItemInput` 指示变量是必需的。
* 方括号(`[]`) `CartItemInput` 类型指定给 `$cartItems` 指示该类型的列表，而不是单个值。

{{$include /help/_includes/graphql-rest-related-links.md}}
