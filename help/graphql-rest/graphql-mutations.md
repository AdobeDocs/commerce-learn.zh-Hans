---
title: 使用GraphQL执行突变
description: 了解如何在Adobe Commerce和[!DNL Magento Open Source]上使用GraphQL执行变异。 使用POST调用执行您的第一个突变。
landing-page-description: 了解如何在Adobe Commerce和[!DNL Magento Open Source]上使用GraphQL执行变异。 使用POST调用执行您的第一个突变。
short-description: 了解如何在Adobe Commerce和[!DNL Magento Open Source]上使用GraphQL执行变异。 使用POST调用执行您的第一个突变。
kt: 13938
doc-type: video
duration: 268
audience: all
last-substantial-update: 2023-10-12T00:00:00.000Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
TQID: 'https://experienceleague.adobe.com/DyzC0YLv2eWrfSAUZb-32cMAePHjurmp1RyynMbsa7Q'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
subfeature_v2:
  - id: e396cff5-f586-484c-89f0-7f1da3308f92
    internal-label: GraphQL
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%
---
# 突变

这是GraphQL和Adobe Commerce系列的第3部分。 突变是指使用GraphQL保存、更新和返回值的能力。


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## 本系列中GraphQL的相关视频和教程

* [第1部分GraphQL — 简介](../graphql-rest/intro-graphql.md)
* [第2部分GraphQL — 查询](../graphql-rest/graphql-queries.md)
* [第4部分GraphQL — 架构](../graphql-rest/graphql-schema.md)

## 示例突变

任何完整的API规范不仅需要提供查询数据的能力，还需要提供创建和更新数据的能力。

REST将区分更改数据的请求和不具有请求类型或“动词”（GET与POST或PUT）的请求。
使用GraphQL时，数据修改查询由对应于其他关键字的`mutation`关键字区分
在服务器上定义的架构中的根类型。

查看此示例突变以将产品添加到用户购物车。 (这需要生成的购物车ID
登录客户的会话或使用`createEmptyCart`突变。)

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

关于上述示例，需要注意的主要事项是，除了使用`mutation`关键字而非`query`之外，
语法与查询相同。 与查询类似，变异包括：

* 任意操作名称(`doAddToCart`)
* 变量列表（例如，`$cartId`）
* 括号中包含参数（例如，`cartId`，设置为值`$cartId`）的初始字段(`addProductsToCart`)
* 大括号中的字段的部分选择

字段子选择允许您灵活定义希望返回的字段(从指定为
完成变异后返回值`addProductsToCart` - `AddProductsToCartOutput`。

如前所述，GraphQL架构中定义的字段从查询的根类型开始（通常称为`Query`）。 同样地，
突变存在另一种根类型（通常称为`Mutation`）。 `addProductsToCart`是一个字段
该根类型。

有关上述示例的其他几点注意事项：

* `!`字符后缀`String`和`CartItemInput`表示该变量是必需的。
* 为`$cartItems`指定的`CartItemInput`类型周围的方括号(`[]`)表示列表
而不是单个值。

{{$include /help/_includes/graphql-rest-related-links.md}}
