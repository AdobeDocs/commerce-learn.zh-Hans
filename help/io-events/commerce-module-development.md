---
title: 了解如何在Adobe Commerce中创建模块以使用事件。
description: 了解如何创建Commerce模块以使用事件。
landing-page-description: 了解如何创建Adobe Commerce模块以使用事件。
short-description: 了解如何创建Adobe Commerce模块以使用事件。
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Adobe Commerce模块开发

了解如何注册事件、查找支持的事件以及如何使用新的XML文件 `io_events.xml` 在自定义模块开发中。 此视频还将向开发人员展示如何查找可以使用的已注册事件，以及取消订阅可能已经定义的任何事件。 其他文档可在 [安装适用于Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 此视频面向谁？

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员。

## 视频内容 {#video-content}

* 在Commerce中注册事件以在Adobe Developer App Builder中使用
* 识别可以注册的事件
* 了解如何在io_events.xml中注册事件
* 了解如何在商务实例中注册事件 `app/etc/config.php`
* 了解如何取消订阅事件

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
