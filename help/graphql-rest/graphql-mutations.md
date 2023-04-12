---
title: 使用GraphQL执行变异
description: 了解有关在Adobe Commerce上使用GraphQL执行变异以及 [!DNL Magento Open Source]. 使用POST调用执行首个变异。
landing-page-description: 了解有关在Adobe Commerce上使用GraphQL执行变异以及 [!DNL Magento Open Source]. 使用POST调用执行首个变异。
short-description: 了解有关在Adobe Commerce上使用GraphQL执行变异以及 [!DNL Magento Open Source]. 使用POST调用执行首个变异。
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

# 突变

任何完整的API规范都不仅需要提供查询数据的功能，还需要提供创建和更新数据的功能。

REST区分更改数据的请求和不使用请求类型或“谓词”(GET与POST或PUT)的请求。
使用GraphQL时，数据修改查询由 `mutation` 与服务器上定义的架构中不同的根类型对应的关键字。

请查看此示例变异，以将产品添加到用户购物车。 (这需要为登录客户的会话或使用 `createEmptyCart` 变异。)

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

您可以想象，上述变异与以下变量字典一起在请求中发送：

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

要注意的是，关于上述示例，除了使用 `mutation` 关键词而不是 `query`，语法与查询相同。 与查询类似，变异包括：

* 任意操作名称(`doAddToCart`)
* 变量列表(例如， `$cartId`)
* 初始字段(`addProductsToCart`)(例如， `cartId`，则设置为的值 `$cartId`)在括号中
* 大括号中字段的子选项

利用字段子选择，可灵活定义要返回的字段（从指定为返回值的类型） `addProductsToCart` - `AddProductsToCartOutput`)。

如前所述，GraphQL架构中定义的字段以查询的根类型开头(通常称为 `Query`)。 同样，还有另一种根类型存在于突变(通常称为 `Mutation`)。 `addProductsToCart` 是该根类型上的字段。

有关上述示例的其他一些说明：

* 的 `!` 字符后缀 `String` 和 `CartItemInput` 表示变量为必需变量。
* 方括号(`[]`) `CartItemInput` 为指定的类型 `$cartItems` 指示该类型的列表，而不是单个值。

{{$include /help/_includes/graphql-rest-related-links.md}}
