---
title: 创建可下载的产品
description: 了解如何使用REST API和Adobe Commerce管理员创建可下载的产品。
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# 创建可下载的产品

了解如何使用REST API和Adobe Commerce管理员创建可下载的产品。

## 此视频面向谁？

- 网站管理员
- 电子商务促销商
- 新的Adobe Commerce开发人员，他们想要了解如何使用REST API在Adobe Commerce中创建产品

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## 允许下载的域

您必须指定允许下载的域。 域通过命令行添加到项目的`env.php`文件。 `env.php`文件详细列出了允许包含可下载内容的域。 如果在&#x200B;_之前使用REST API_&#x200B;创建了可下载的产品，则更新了`php.env`文件时出错：

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

若要设置域，请连接到服务器： `bin/magento downloadable:domains:add www.example.com`

完成此操作后，`env.php`将在&#x200B;_downloadable_domains_&#x200B;数组内进行修改。

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

现在该域已添加到`env.php`，您可以在Adobe Commerce管理员中或使用REST API创建可下载的产品。

有关详细信息，请参阅[配置引用](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=zh-Hans#downloadable_domains)。

>[!IMPORTANT]
>在某些版本的Adobe Commerce上，在Adobe Commerce管理员中编辑产品时，您可能会收到以下错误。 产品是使用REST API创建的，但链接下载的价格为`null`。

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`。

要修复此错误，请使用更新链接API： `POST V1/products/{sku}/downloadable-links.`

有关详细信息，请参阅[使用cURL更新产品下载链接](#update-downloadable-links)。

## 使用cURL创建可下载的产品（从远程服务器下载）

此示例说明当要下载的文件不在同一服务器上时，如何使用cURL创建可下载的产品。 如果文件存储在S3存储段或其他数字资产管理器中，则此用例是典型的。

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

## 使用cURL创建可下载的产品(从Commerce应用程序服务器下载)

此示例演示了当文件与Adobe Commerce应用程序存储在同一服务器上时，如何使用cURL从Adobe Commerce管理员创建可下载的产品。

在此使用案例中，当管理目录的管理员选择`upload file`时，文件将传输到`pub/media/downloadable/files/links/`目录。  自动化会根据以下模式创建文件并将其移动到其各自位置：

- 每个上载的文件都存储在文件夹中，该文件夹基于文件名的前两个字符。
- 启动上传后，Commerce应用程序会创建或使用现有文件夹传输文件。
- 下载文件时，路径的`link_file`部分使用附加到`pub/media/downloadable/files/links/`目录的路径部分。

例如，如果上传的文件名为`download-example.zip`：

- 文件上载到路径`pub/media/downloadable/files/links/d/o/`。
如果子目录`/d`和`/d/o`不存在，则创建它们。

- 文件的最终路径为`/pub/media/downloadable/files/links/d/o/download-example.zip`。

- 此示例的`link_url`值为`d/o/download-example.zip`

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

使用终结点`rest/all/V1/products/{sku}/downloadable-links`
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

- [可下载的产品类型](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=zh-Hans){target="_blank"}
- [可下载的域配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=zh-Hans#downloadable_domains){target="_blank"}
- [Adobe Developer REST教程](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
