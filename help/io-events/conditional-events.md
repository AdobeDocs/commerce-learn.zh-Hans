---
title: 了解如何在Adobe Commerce中使用条件事件
description: 了解如何使用条件事件在Adobe Developer App Builder中使用。
landing-page-description: 了解如何使用Adobe Commerce条件事件。
short-description: 了解如何使用Adobe Commerce条件事件。
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

# Adobe Commerce条件事件

了解Adobe Commerce中可在Adobe Developer App Builder中使用的条件事件。 其他文档位于 [为Adobe Commerce安装Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## 这个视频给谁？

* 初次使用Adobe Commerce和Adobe Developer App Builder的开发人员使用I/O事件，需要创建Adobe应用程序生成器项目。

## 视频内容 {#video-content}

* 了解条件事件
* 了解新XML文件io_events.xml的正确用法
* 了解如何配置条件事件
* 定义用于条件事件的规则
* 了解如何在商务实例中注册事件 `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806?quality=12&learn=on)

## 有用命令 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
