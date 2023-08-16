---
title: 了解如何安装Adobe Commerce 2.4.5的IO事件
description: 了解如何在Adobe Commerce 2.4.5中安装IO事件所需的模块以用于Adobe Developer App Builder
landing-page-description: 了解如何使用编辑器安装Adobe Commerce 2.4.5所需的几个模块。
short-description: 了解如何使用编辑器安装Adobe Commerce 2.4.5所需的几个模块。
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 0%

---

# Adobe Commerce 2.4.5安装

了解如何使用适用于版本2.4.5的编辑器在Adobe Commerce中安装多个新模块。这将设置要在Adobe Commerce应用程序中使用的所需模块。 其他文档可在 [安装适用于Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 此视频面向谁？

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员

## 视频内容 {#video-content}

* 使用编辑器安装所需模块
* 要为内部部署托管运行的命令
* Adobe Commerce Cloud要运行的命令
* Adobe Commerce Cloud yaml需要编辑

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
