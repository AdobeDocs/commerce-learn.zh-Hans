---
title: 创建虚拟产品
description: 了解如何使用REST API和商务管理员创建虚拟产品。
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 225faceffefc31a6205f689933210510dba235d1
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# 创建虚拟产品

了解如何使用REST API和Adobe Commerce管理员创建虚拟产品。

## 此视频面向谁？

- 网站管理员
- 电子商务促销商
- 新的Adobe Commerce开发人员，他们想要了解如何使用REST API在Adobe Commerce中创建产品。

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3425723?learn=on)

## 使用curl创建虚拟产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "postman-rest-virtual-product-test",
    "name": "POSTMAN virtual product test",
    "attribute_set_id": 4,
    "price": 44,
    "type_id": "virtual"
  }
}
'
```

## 使用curl获取产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Admin-created-virtual-product' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=2cd97649bcf4ee5b45b23f8eb940e3a0; private_content_version=564dde2976849891583a9a649073f01e'
```

## 其他资源

- [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
