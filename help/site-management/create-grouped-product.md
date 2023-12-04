---
title: 创建分组产品
description: 了解如何使用REST API和商务管理员创建分组产品。
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: eec9a85198f963404f5ba82fc2fc76315a82f964
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# 创建分组产品

分组产品由简单独立产品组成，这些产品以组形式显示。 您可以提供单个产品的变体或按季节或主题对它们进行分组。 在创建分组产品之前，请验证要包含在分组中的所有简单产品在Adobe Commerce中均可用，并创建任何不存在的产品。 在本教程中，您将了解如何使用REST API和Adobe Commerce管理员创建分组产品。

使用REST API在管理员中创建组产品：

1. 创建一个空的分组产品。
1. 创建要在分组产品中使用的简单产品。
1. 使用简单产品填充空的分组产品。
1. 创建一个空的分组产品并关联简单产品。

在Adobe Commerce管理员中创建分组产品时，建议先创建简单产品。 准备好创建分组产品后，将简单产品分配给一个批次中的分组产品，以关联这些产品。

有效负载中的排序顺序属性是必需的，并且前端用于按所需顺序显示关联产品。

## 此视频面向谁？

- 网站管理员
- 电子商务促销商
- 新的Adobe Commerce开发人员，他们想要了解如何使用REST API在Adobe Commerce中创建分组产品。

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## 分组产品的设置

在此示例中，有三个简单产品（首先创建）和一个分组产品。 两个简单产品与分组产品相关联，然后将第三个简单产品添加到分组产品中。

## 使用cURL创建第一个简单产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## 使用cURL创建第二个简单产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## 使用cURL创建第三个简单产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## 使用cURL创建空的分组产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## 将第一个和第二个简单产品添加到分组产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## 将第三个简单产品添加到现有的分组产品

包括相应的职位编号(除以下 `1` 或 `2`)，它们用于最初与分组产品关联的前两个产品。 在本例中，位置为 `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## 从分组产品中删除简单产品

至 [删除简单产品](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) 在分组产品中，使用： `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

了解要用作的内容 `{type}`，使用xdebug捕获请求并评估$linkTypes： `related`， `crosssell`， `uupsell`、和 `associated`.
![分组的产品链接类型 — 替换文本](/help/assets/site-management/catalog/grouped-types.png "在xdebug会话期间捕获的分组产品链接类型")

将简单产品链接到分组产品时，有效负荷包含几个类似于以下内容的部分：

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

在有效负载中， `link_type` 值 `associated` 提供 `{type}` DELETE请求中所需的值。 请求URL将类似于 `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

请参阅cURL请求以删除包含 `product-sku-three` 包含的分组产品中的SKU `my-new-grouped-product` SKU：

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## 使用cURL获取分组产品

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 其他资源

- [创建和管理分组的产品](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [已分组的产品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
- [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
