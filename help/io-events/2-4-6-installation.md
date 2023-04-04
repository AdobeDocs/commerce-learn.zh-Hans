---
title: 了解如何为Adobe Commerce 2.4.6安装IO事件
description: 了解如何在Adobe Commerce 2.4.6中安装IO事件所需的模块，以便在Adobe Developer App Builder中使用
landing-page-description: 了解如何安装Adobe Commerce 2.4.6所需的多个模块。
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---


# Adobe Commerce 2.4.6安装

了解如何使用版本2.4.6的编辑器在Adobe Commerce中安装多个新模块。其他文档位于 [为Adobe Commerce安装Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 这个视频给谁？

* 初次使用I/O事件的Adobe Commerce和Adobe Developer应用程序生成器的开发人员。

## 视频内容 {#video-content}

* 用于本地托管的运行命令
* 要为Adobe Commerce Cloud运行的命令
* Adobe Commerce Cloud yaml需要编辑

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## 有用命令 {#useful-commands}

根据您使用的是自托管环境还是使用Adobe Commerce Cloud，有各种命令会略有不同。

### 内部部署托管 {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
