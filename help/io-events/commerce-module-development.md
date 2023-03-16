---
title: 了解如何在Adobe Commerce中创建模块以使用事件。
description: 了解如何创建商务模块以使用事件。
landing-page-description: 了解如何创建Adobe Commerce模块以使用事件。
short-description: Learn how to create an Adobe Commerce module to use events.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---


# Adobe Commerce模块开发

了解如何注册事件、查找受支持的事件以及如何使用新的XML文件 `io_events.xml` 在自定义模块开发中。 此视频还将向开发人员展示如何查找可使用的已注册事件，以及如何取消订阅任何可能已定义的事件。 其他文档位于 [为Adobe Commerce安装Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 这个视频给谁？

* 初次使用Adobe Commerce和Adobe Developer App Builder的I/O事件的开发人员。

## 视频内容 {#video-content}

* 在Commerce中注册事件以在Adobe Developer App Builder中使用
* 识别可注册的事件
* 了解如何在io_events.xml中注册事件
* 了解如何在商务实例中注册事件 `app/etc/config.php`
* 了解如何取消订阅事件

>[!VIDEO](https://video.tv.adobe.com/v/3415802)

## 有用命令 {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
