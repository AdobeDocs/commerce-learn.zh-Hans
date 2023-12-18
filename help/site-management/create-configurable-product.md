---
title: 创建可配置产品
description: 了解如何使用REST API和商务管理员创建可配置产品。
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 76716d4c9530963f198a855e101c76b6374c6d75
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 0%

---


# 创建可配置产品

可配置产品是多个简单产品的父产品。 定义可配置产品，要求采购员作出一个或多个选择以选择特定的产品变型。 例如，如果产品是衬衫，则购买者必须选择“尺寸”和“颜色”选项来选择衬衫。

尽管可配置产品使用更多的SKU，并且最初可能需要更长的时间进行设置，但最终仍可以节省您的时间。 如果您计划拓展业务，那么可配置的产品类型对于具有多种选项的产品来说是一个不错的选择。

在创建可配置产品之前，请验证可配置产品中包含的所有简单产品在Adobe Commerce中是否都可用。 创建任何不存在的项目。

在本教程中，了解如何使用REST API和Adobe Commerce管理员创建可配置产品。

使用REST API创建可配置的产品：

1. 获取的属性 [属性集](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html) 为后续API调用使用ID号。
1. 创建简单产品以在可配置产品中使用。
1. 创建空的可配置产品并关联简单产品。
1. 设置可配置产品的产品属性。
1. 使用简单产品填充空的可配置产品。
1. 获取可配置产品和所有属性。
1. 获取为可配置产品分配的子产品。
1. 删除简单产品与可配置产品的关联。

从Adobe Commerce管理员创建可配置产品时，您可以先创建简单产品，也可以使用自动工具创建新的简单产品以供使用。

## 此视频面向谁？

- 网站管理员
- 电子商务促销商
- 新的Adobe Commerce开发人员，他们想要了解如何使用REST API在Adobe Commerce中创建可配置产品

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## 使用cURL获取颜色属性

在本例中，对于属性集10，返回包含所有单个属性的整个属性集。 它可能会很长，数百行也是常见的。 查看响应时，颜色的属性ID可能位于中间。 通过使用grep或其他方法搜索结果，加快对这些值的搜索。 我的响应在第665行附近，包含在JSON响应中的以下代码片段中。

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


要检索属性ID以设置可配置产品，请更新 `attribute-sets/10/attributes` 要替换的以下cURL请求的一部分 `10` 环境中属性集ID的位置。 此请求使用GET方法。

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 使用cURL创建第一个简单产品

### 调整环境ID和产品详细信息

使用API创建第一个简单产品，以使用cURL发送以下POST请求。

在提交请求之前，请使用环境的值更新示例。

- 更改 `"attribute-set": 10` 要替换 `10` 环境中属性集ID的位置。
- 更改 `"value": "13"` 要替换 `13` 用来自您环境的价值。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## 使用cURL创建第二个简单产品

使用API创建第二个简单产品，以使用cURL发送以下POST请求。

在提交请求之前，请使用环境的值更新示例。

- 更改 `"attribute_set_id": 10,` 和替换 `10` 环境中具有的属性集ID。
- 更改 `"value": "14"` 和替换 `14` 用来自您环境的价值。


```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## 使用cURL创建第三个简单产品

使用API创建第三个简单产品，以使用cURL发送以下POST请求。

在提交请求之前，请使用环境的值更新示例。

- 更改 `"attribute_set_id": 10,` 要替换 `10` 环境中属性集ID的位置。
- 更改 `"value": "15"` 和替换 `15` 用来自您环境的价值。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## 使用cURL创建空的可配置产品

通过使用API创建空的可配置产品，以使用cURL发送以下POST请求。

在提交请求之前，请使用环境的值更新示例。

- 更改 `"attribute_set_id": 10,` 和替换 `10` 环境中属性集id的位置。
- 更改 `"value": "93"` 和替换 `93` 用来自您环境的价值。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## 为可配置产品设置可用选项

通过使用API设置可配置产品的可用选项，以使用cURL发送以下POST请求。

在提交请求之前，请更改 `"attribute_id": 93,` 要替换 `93` 使用您环境中的属性ID。

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

如果忘记设置可配置产品（父产品）的选项，则在尝试将子产品与可配置产品关联时会出现错误。 错误消息类似于以下示例：

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## 将子产品链接到可配置

现在，您已创建了三个简单的产品：

- `"Kids Hawaiian Ukulele Red"`，
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

通过使用API发送每个产品的以下POST请求，将这些简单产品添加为可配置产品的子项。 为每种产品单独提交请求。

对于每个请求，请更新 `childSKU` ，其中包含您要添加的子产品的值。 下面的示例将简单产品指定为 `kids-Hawaiian-Ukulele-red` 到具有SKU的可配置产品 `Kids-Hawaiian-Ukulele-red`.


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## 使用cURL获取可配置产品

现在，您已创建具有三个已分配的子SKU的可配置产品。 您可以看到由API分配的产品的链接ID，以使用cURL发送以下GET请求。 此请求返回有关可配置产品的详细信息。

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

下面使用GET方法

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 获取与可配置产品关联的子产品

此请求仅返回与可配置产品关联的子项。 此响应具有子产品的所有属性，包括SKU和价格。

下面使用GET方法

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 从父可配置产品中删除或移除子产品

您可以直接从可配置产品中删除子产品，而无需从目录中删除产品，方法是使用API通过cURL发送以下DELETE请求。

下面使用DELETE方法

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 其他资源

- [创建可配置的产品教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [可配置产品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html){target="_blank"}
- [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
