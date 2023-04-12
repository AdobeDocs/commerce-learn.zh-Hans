---
title: 配置Adobe Commerce
description: 了解如何配置Adobe Commerce以允许在Adobe Developer App Builder中使用事件。
landing-page-description: 了解如何配置Adobe Commerce以使用事件机制供Adobe Developer App Builder使用。
short-description: 了解如何配置Adobe Commerce以使用事件机制供Adobe Developer App Builder使用。
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# 配置Adobe Commerce

了解如何配置Adobe Commerce以显示事件。 其他文档位于 [为Adobe Commerce安装Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 这个视频给谁？

* 初次使用Adobe Commerce和Adobe Developer App Builder的开发人员使用I/O事件，需要创建Adobe应用程序生成器项目。

## 视频内容 {#video-content}

* 在商务管理员中配置Adobe I/O事件
* 在商务管理员中保存私钥
* 在商务管理员中保存唯一标识符
* 创建事件提供程序

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## 有用命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
