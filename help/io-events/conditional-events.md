---
title: 瞭解如何在Adobe Commerce中使用條件式事件
description: 瞭解如何使用要在Adobe Developer App Builder中使用的條件式事件。
landing-page-description: 瞭解如何使用Adobe Commerce條件式事件。
short-description: 瞭解如何使用Adobe Commerce條件式事件。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Adobe Commerce條件事件

瞭解可用於Adobe Developer App Builder的Adobe Commerce條件事件。 其他檔案可在下列網址找到： [安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## 這部影片是給誰看的？

* 剛開始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的開發人員，需要建立AdobeApp Builder專案。

## 視訊內容 {#video-content}

* 瞭解條件事件
* 瞭解新XML檔案io_events.xml的正確用法
* 瞭解如何設定條件式事件
* 定義用於條件事件的規則
* 瞭解如何在Commerce執行個體中註冊事件 `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806?quality=12&learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
