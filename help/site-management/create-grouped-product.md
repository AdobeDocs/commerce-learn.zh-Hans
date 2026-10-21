---
title: 创建分组产品
description: 了解如何使用REST API和Commerce管理员创建分组产品。
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
TQID: 'https://experienceleague.adobe.com/nosJh3ytiC54wmNWaUmSu9qjZCN-ssjolNZD702EpEg'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: cc04bd17-78d5-5120-8c3d-1b8a57e49280
    internal-label: Integration
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: bb2df8be-afdd-4818-b6b5-95ca1dd3bc3a
    internal-label: Admin workspace
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%
---
# 创建分组产品

分组产品由简单独立产品组成，这些产品以组形式显示。 您可以提供单个产品的变体或按季节或主题对它们进行分组。 在创建分组产品之前，请验证要包含在分组中的所有简单产品在Adobe Commerce中均可用，并创建任何不存在的产品。

在本教程中，您将了解如何使用REST API和Adobe Commerce管理员创建分组产品。

使用REST API在管理员中创建组产品：

1. 创建一个空的分组产品。
1. 创建要在分组产品中使用的简单产品。
1. 使用简单产品填充空的分组产品。
1. 创建一个空的分组产品并关联简单产品。

   将简单产品与分组产品关联时，前端将使用有效负荷中的排序顺序属性(`position`)来按所需顺序显示关联的产品。 如果未指定`position`属性，则产品将按添加到分组产品的顺序显示。

从Adobe Commerce管理员创建分组产品时，请先创建简单产品。 准备好创建分组产品后，将简单产品分配给一个批次中的分组产品，以关联这些产品。

## 此视频面向谁？

* 网站管理员
* 电子商务促销商
* 新的Adobe Commerce开发人员，他们想要了解如何使用REST API在Adobe Commerce中创建分组产品。

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
