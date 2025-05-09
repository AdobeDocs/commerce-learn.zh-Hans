---
title: 配置Adobe Commerce
description: 了解如何配置Adobe Commerce以允许在Adobe Developer App Builder中使用事件。
landing-page-description: 了解如何配置Adobe Commerce以使用Adobe Developer App Builder使用的事件机制。
short-description: 了解如何配置Adobe Commerce以使用Adobe Developer App Builder使用的事件机制。
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Architect, Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 配置Adobe Commerce

了解如何配置Adobe Commerce以公开事件。 在[安装Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}中找到其他文档。

## 此视频面向谁？

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员，需要创建一个Adobe的App Builder项目。

## 视频内容 {#video-content}

* 在Commerce管理员中配置Adobe I/O事件
* 在Commerce管理员中保存私钥
* 在Commerce管理员中保存唯一标识符
* 创建事件提供程序

>[!VIDEO](https://video.tv.adobe.com/v/3419711?quality=12&learn=on&captions=chi_hans)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
