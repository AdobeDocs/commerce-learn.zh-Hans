---
title: 瞭解如何在Adobe Commerce中建立模組以使用事件。
description: 瞭解如何建立Commerce模組以使用事件。
landing-page-description: 瞭解如何建立Adobe Commerce模組以使用事件。
short-description: 瞭解如何建立Adobe Commerce模組以使用事件。
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Adobe Commerce模組開發

瞭解如何註冊事件、尋找支援的事件，以及如何使用新的XML檔案 `io_events.xml` 在自訂模組開發中。 影片也會向開發人員展示如何尋找可以使用的已註冊事件，以及取消訂閱任何可能已定義的事件。 其他檔案可在下列網址找到： [安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 這部影片是給誰看的？

* 不熟悉Adobe Commerce和Adobe Developer App Builder （使用I/O事件）的開發人員。

## 視訊內容 {#video-content}

* 在Commerce中註冊事件以用於Adobe Developer App Builder
* 識別可註冊的事件
* 瞭解如何在io_events.xml中註冊事件
* 瞭解如何在Commerce執行個體中註冊事件 `app/etc/config.php`
* 瞭解如何取消訂閱事件

>[!VIDEO](https://video.tv.adobe.com/v/3415802?quality=12&learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
