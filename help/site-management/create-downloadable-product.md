---
title: Create a downloadable product
description: Learn how to create a downloadable product using the REST API and the Adobe Commerce Admin.
kt: 14464
doc-type: video
duration: 946
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
TQID: https://experienceleague.adobe.com/YHtAD-NRQmIG58myhZk9X7-jJjwlk8S4NX9jYnZwnQc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 631
ht-degree: 0%

---

# Create a downloadable product

Learn how to create a downloadable product using the REST API and the Adobe Commerce Admin.

## 此视频面向谁？

* 网站管理员
* 电子商务促销商
* New Adobe Commerce developers who want to learn how to create products in Adobe Commerce using the REST API

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## Allowed downloadable domains

You must specify which domains are permitted to allow downloads. Domains are added to the project&#39;s `env.php` file via the command line. The `env.php` file details the domains allowed to contain downloadable content. An error occurs if a downloadable product is created using the REST API _before_  the `php.env` file is updated:

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

To set the domain, connect to the server: `bin/magento downloadable:domains:add www.example.com`

Once that is complete, the `env.php` is modified inside the _downloadable_ domains_ array.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

Now that the domain is added to the `env.php`, you can create a downloadable product in the Adobe Commerce Admin or by using the REST API.

See [Configuration Reference](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=zh-Hans#downloadable_domains) to learn more.

>[!IMPORTANT]
>On some versions of Adobe Commerce, you might get the following error when a product is edited in the Adobe Commerce Admin. The product is created using the REST API but the linked download has a `null` price.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

To fix this error, use the update link API: `POST V1/products/{sku}/downloadable-links.`

See the [Update a product download link using cURL](#update-downloadable-links) section for more info.

## Create a downloadable product using cURL (download from remote server)

This example shows how to create a downloadable product using cURL when the file to download is not on the same server. 如果文件存储在S3存储段或其他数字资产管理器中，则此用例是典型的。

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## 使用cURL创建可下载的产品（从Commerce应用程序服务器下载）

此示例演示了当文件与Adobe Commerce应用程序存储在同一服务器上时，如何使用cURL从Adobe Commerce管理员创建可下载的产品。

在此使用案例中，当管理目录的管理员选择`upload file`时，文件将传输到`pub/media/downloadable/files/links/`目录。  自动化会根据以下模式创建文件并将其移动到其各自位置：

* 每个上载的文件都存储在文件夹中，该文件夹基于文件名的前两个字符。
* 启动上传后，Commerce应用程序会创建或使用现有文件夹传输文件。
* 下载文件时，路径的`link_file`部分使用附加到`pub/media/downloadable/files/links/`目录的路径部分。

例如，如果上传的文件名为`download-example.zip`：

* 文件上载到路径`pub/media/downloadable/files/links/d/o/`。
如果子目录`/d`和`/d/o`不存在，则创建它们。

* 文件的最终路径为`/pub/media/downloadable/files/links/d/o/download-example.zip`。

* 此示例的`link_url`值为`d/o/download-example.zip`

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## 使用curl获取产品

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## 使用Postman更新产品 {#update-downloadable-links}

使用端点 `rest/all/V1/products/{sku}/downloadable-links`
`SKU`是在创建产品时生成的产品ID。 例如，在下面的代码示例中，这是数字39，但请确保将其更新以使用您网站中的ID。 这将更新可下载产品的链接。

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## 使用CURL更新产品下载链接

使用cURL更新产品下载链接时，该URL包含要更新的产品的SKU。  在以下代码示例中，SKU是`abcd12345`。 在提交命令时，更改值以匹配要更新的产品的SKU。

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## 其他资源

* [可下载的产品类型](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=zh-Hans){target="_blank"}
* [Downloadable Domains Configuration Guide](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=zh-Hans#downloadable_domains){target="_blank"}
* [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
