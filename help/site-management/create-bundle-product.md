---
title: 创建捆绑产品
description: 了解如何使用REST API和Commerce管理员创建捆绑包产品。
kt: 14589
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5d688e6a-ae8c-4a55-b16c-5d3ae2d1bfd5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# 创建捆绑产品

捆绑产品是一种将多个产品分组到父产品下的方法。 这些子产品可以是一组已定义的产品，也可以提供一些变体，以便为客户提供灵活的配置选项。 捆绑产品类型的设置时间确实要长一些，在配置它们之前，您必须进行一些规划。 但是，提供捆绑产品可让客户更轻松地自定义其产品选择，从而改善购物体验。

例如，您可以在网络商店中提供名为`Learning to surf`的产品捆绑包。 捆绑包是父产品，用作已分配子产品的容器，这些子产品指定可用选项：

- 标准冲浪板
- 典型的冲浪板拉链
- 红色冲浪板翅片

当需要更大的灵活性时，建议允许子产品的多个选项。 这要求更复杂地使用选项和子产品。 要展开上一个示例，最终选项包括：

- 标准冲浪板
- 典型的冲浪板拉链
- 翅片颜色选择：
   - 红色
   - 蓝色
   - 黄色

无论捆绑包是一组简单的静态产品还是多个具有变体的产品，灵活的配置选项都使捆绑包产品类型成为Adobe Commerce商店独一无二且功能强大的促销工具。

在创建捆绑产品之前，请确认捆绑产品中包含的所有简单产品在Adobe Commerce中均可用。 创建任何不存在的项目。

在本教程中，了解如何使用REST API和Adobe Commerce管理员创建捆绑包产品。

使用REST API定义捆绑产品：

1. 创建简单产品以在捆绑包产品中使用。
1. 创建捆绑产品并关联简单产品。
1. 获取捆绑包产品和所有相关选项。
1. 从捆绑产品的一个部分中删除一个选项。
1. 恢复捆绑产品的选项。

从Adobe Commerce管理员创建捆绑产品时，您可以先创建简单产品，也可以使用自动工具通过向导创建简单产品。

## 此视频面向谁？

- 网站管理员
- 电子商务促销商
- 新的Adobe Commerce开发人员，他们想要了解如何使用REST API在Adobe Commerce中创建捆绑产品

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3454513?learn=on&captions=chi_hans)

## 使用REST创建产品

以下命令将创建定义本示例中的捆绑产品所需的所有产品：五个简单产品，以及一个具有三个选项的捆绑产品。

在提交请求之前，请使用环境的值更新示例。

- 更改`"attribute-set": 4`以使用您环境中的属性集ID替换`4`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "10-ft-beginner-surfboard",
    "name": "10 Foot Beginner surfboard",
    "attribute_set_id": 4,
    "price": 100,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "8-ft-surboard-leash",
    "name": "8 Foot surboard leash",
    "attribute_set_id": 4,
    "price": 50,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "red-fins-and-fin-plugs",
    "name": "Red fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "blue-fins-and-fin-plugs",
    "name": "Blue fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "yellow-fins-and-fin-plugs",
    "name": "Yellow fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

## 创建捆绑产品并将简单产品指定为选项

通过发送以下POST请求创建捆绑产品。

在提交请求之前，请使用环境的值更新示例。

- 更改`"attribute_set_id": 4,`并使用您环境中的属性集ID替换`4`。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "beginner-surfboard",
    "name": "Beginner Surfboard",
    "attribute_set_id": 4,
    "status": 1,
    "visibility": 4,
    "type_id": "bundle",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      },
      "bundle_product_options": [
        {
          "option_id": 0,
          "position": 1,
          "sku": "surfboard-essentials",
          "title": "Beginners 10ft Surfboard",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "10-ft-beginner-surfboard",
              "option_id": 1,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 100,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 1,
          "position": 2,
          "sku": "surboard-leash",
          "title": "Surfboard leash",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "8-ft-surboard-leash",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 50,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 3,
          "position": 2,
          "sku": "surfboard-color",
          "title": "Choose a color for the fins and fin plugs",
          "type": "radio",
          "required": true,
          "product_links": [
            {
              "sku": "red-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "blue-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 2,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "yellow-fins-and-fin-plugs",
              "option_id": 3,
              "qty": 1,
              "position": 3,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        }
      ]
    },
    "custom_attributes": [
      {
        "attribute_code": "price_view",
        "value": "0"
      }
    ]
  },
  "saveOptions": true
}
'
```

## 从捆绑产品中删除选项

通过使用cURL发送以下DELETE请求，从捆绑产品中删除子产品而不从目录中删除该产品。

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## 恢复产品选项

在更新捆绑产品选项时，请确保包括要与此产品关联的所有选项。 如果原始选项集包含三个产品并且一个已删除，请在POST请求中包含所有三个选项，以确保产品捆绑包指定所有选项。 如果仅包含您删除的选项，则更新的产品捆绑包将仅包含该选项。

通过查看从为捆绑包产品创建的响应来查找选项ID。 在该响应中，`option_id`是`35`。

```json
...
,
            {
                "option_id": 35,
                "title": "added back color options for fins and fin plugs",
                "required": true,
                "type": "radio",
                "position": 2,
                "sku": "beginner-surfboard",
                "product_links": [
                    {
                        "id": "77",
                        "sku": "red-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 1,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "78",
                        "sku": "blue-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 2,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "79",
                        "sku": "yellow-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 3,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    }
                ]
            }
...
```

更新产品捆绑包，以通过提交以下POST请求来添加您删除的选项。

```bash
curl --location '{{your.url.here}}/rest/default/V1/bundle-products/options/add' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "option": {
    "option_id": 35,
    "title": "Choose a color for the fins and fin plugs",
    "required": true,
    "type": "radio",
    "position": 2,
    "sku": "beginner-surfboard",
    "product_links": [
      {
        "sku": "red-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 1,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "blue-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 2,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "yellow-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 3,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      }
    ],
    "extension_attributes": {}
  }
}'
```

## 其他资源

- [创建捆绑产品教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
- [捆绑产品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html?lang=zh-Hans){target="_blank"}
- [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
