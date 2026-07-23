---
title: 创建Adobe Commerce模块以使用I/O事件
description: 了解如何创建Commerce模块以使用事件。
jira: KT-11891
doc-type: Tutorial
duration: 314
last-substantial-update: 2023-02-21
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
TQID: https://experienceleague.adobe.com/bRnOh6fnsyTY-21f81vIXV4-eeitLXQWsAbjg2rx-Is
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: aea3c1c4ad2b5fbb28ebc64965f09d3a73831344
workflow-type: tm+mt
source-wordcount: 140
ht-degree: 0%

---

# Adobe Commerce模块开发

了解如何在自定义模块开发中注册事件、查找支持的事件和使用新的XML文件`io_events.xml`。 此视频还向开发人员展示如何查找要使用的已注册事件，以及如何删除已定义的事件。 在[安装适用于Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}中找到其他文档。

## 此视频面向谁？

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员。

## 视频内容 {#video-content}

* 为Adobe Developer App Builder注册Commerce事件
* 识别可以注册的事件
* 了解如何在io_events.xml中注册事件
* 了解如何在Commerce实例`app/etc/config.php`中注册事件
* 了解如何取消订阅事件

>[!VIDEO](https://video.tv.adobe.com/v/3415802?learn=on)

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

