---
title: 了解如何为Adobe Commerce 2.4.5安装IO事件
description: 了解如何在Adobe Commerce 2.4.5中安装IO事件所需的模块，以便在Adobe Developer App Builder中使用
landing-page-description: 了解如何使用编辑器安装Adobe Commerce 2.4.5所需的多个模块。
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.5 using composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.5"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---


# Adobe Commerce 2.4.5安装

了解如何使用Composer在Adobe Commerce中安装版本2.4.5的多个新模块。这将设置在Adobe Commerce应用程序中使用的所需模块。 其他文档位于 [为Adobe Commerce安装Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 这个视频给谁？

* 使用I/O事件的Adobe Commerce和Adobe Developer应用程序生成器的新开发人员

## 视频内容 {#video-content}

* 使用编辑器安装所需模块
* 用于本地托管的运行命令
* 要为Adobe Commerce Cloud运行的命令
* Adobe Commerce Cloud yaml需要编辑

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## 有用命令 {#useful-commands}

根据您使用的是自托管环境还是使用Adobe Commerce Cloud，有各种命令会略有不同。

### 内部部署托管 {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
