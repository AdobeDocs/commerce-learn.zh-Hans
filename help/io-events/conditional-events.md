---
title: 了解如何在Adobe Commerce中使用条件事件
description: 了解如何使用要在Adobe Developer App Builder中使用的条件事件。
landing-page-description: 了解如何使用Adobe Commerce条件事件。
short-description: 了解如何使用Adobe Commerce条件事件。
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: e02da0901c01871360bcc556666e310acd7c7224
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Adobe Commerce条件事件

了解Adobe Commerce中可以在Adobe Developer App Builder中使用的条件事件。 在[安装Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}中找到其他文档。

## 此视频面向谁？

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员，需要创建一个Adobe的App Builder项目。

## 视频内容 {#video-content}

* 了解条件事件
* 了解新XML文件io_events.xml的正确用法
* 了解如何配置条件事件
* 定义条件事件中使用的规则
* 了解如何在Commerce实例`app/etc/config.php`中注册事件

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
