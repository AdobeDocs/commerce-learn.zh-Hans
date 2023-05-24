---
title: 了解如何安装Adobe Commerce 2.4.5的IO事件
description: 了解如何在Adobe Commerce 2.4.5中安装IO事件所需的模块，以便在Adobe Developer App Builder中使用
landing-page-description: 了解如何使用编辑器安装Adobe Commerce 2.4.5所需的几个模块。
short-description: 了解如何使用编辑器安装Adobe Commerce 2.4.5所需的几个模块。
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 0%

---

# Adobe Commerce 2.4.5安装

了解如何使用Composer for version 2.4.5在Adobe Commerce中安装多个新模块。这将设置在Adobe Commerce应用程序中使用的所需模块。 其他文档可在 [安装适用于Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 此视频面向谁？

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员

## 视频内容 {#video-content}

* 使用编辑器安装所需模块
* 要为内部部署托管运行的命令
* 为Adobe Commerce Cloud运行的命令
* Adobe Commerce Cloud yaml必需编辑

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## 有用的命令 {#useful-commands}

有些命令略有不同，具体取决于您是在自托管环境中还是使用Adobe Commerce Cloud。

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

Commerce Cloud `.magento.env.yaml`：

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
