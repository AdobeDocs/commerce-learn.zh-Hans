---
title: Create a grouped product
description: Learn how to create a grouped product using the REST API and the Commerce Admin.
kt: 14585
doc-type: video
duration: 979
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
TQID: https://experienceleague.adobe.com/nosJh3ytiC54wmNWaUmSu9qjZCN-ssjolNZD702EpEg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: c18ed297-2187-4aec-affb-9d9654eca6fc
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 551
ht-degree: 0%

---

# Create a grouped product

A grouped product consists of simple standalone products that are presented as a group. You can offer variations of a single product or group them by season or theme. Before creating a grouped product, verify that all the simple products to include in the group are available in Adobe Commerce, and create any that do not exist.

In this tutorial, you learn how to create a grouped product using the REST API and the Adobe Commerce Admin.

Use the REST API to create a group product in the Admin:

1. Create an empty grouped product.
1. Create simple products for use in the grouped product.
1. Populate the empty grouped product with simple products.
1. Create an empty grouped product and associate the simple products.

   When you associate simple products to the grouped product, the sort order attribute (`position`) in the payload  is used by the frontend to display the associated products in a desired order. If the `position` attribute is not specified, the products are displayed in the order that they were added to the grouped product.

When creating grouped products from the Adobe Commerce Admin, create the simple products first. When you are ready to create the grouped product, associate the simple products by assigning them to the grouped product in one batch.

## 此视频面向谁？

* 网站管理员
* 电子商务促销商
* New Adobe Commerce developers who want to learn how to create grouped products in Adobe Commerce using the REST API.

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## Setup for the grouped product

In this example, there are three simple products (created first) and a grouped product. Two simple products are associated with the grouped product, and then the third simple product is added to the grouped product.

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

包括相应的职位编号（`1`或`2`除外），这些职位编号用于最初与分组产品关联的前两个产品。 对于此示例，位置为`4`。

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

若要从分组产品中[删除简单产品](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/)，请使用： `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`。

要发现用作`{type}`的内容，请使用xdebug捕获请求并评估$linkTypes： `related`、`crosssell`、`uupsell`和`associated`。
![分组的产品链接类型 — 替换文本](/help/assets/site-management/catalog/grouped-types.png "在xdebug会话期间捕获的分组的产品链接类型")

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

在有效负载中，`link_type`值`associated`提供DELETE请求中所需的`{type}`值。 请求URL将类似于`/V1/products/my-new-grouped-product/links/associated/product-sku-three`。

查看cURL请求，以从具有`my-new-grouped-product` SKU的分组产品中删除具有`product-sku-three` SKU的简单产品：

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

* [创建和管理分组的产品](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
* [已分组的产品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
* [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
