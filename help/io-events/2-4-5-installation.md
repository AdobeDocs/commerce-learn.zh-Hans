---
title: 了解如何安装Adobe Commerce 2.4.5的IO事件
description: 了解如何在Adobe Commerce 2.4.5中安装IO事件所需的模块以用于Adobe Developer App Builder
jira: KT-11886
doc-type: Tutorial
duration: 179
last-substantial-update: 2023-02-22
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
TQID: https://experienceleague.adobe.com/vb-q-JXeM4KvkxzfDui1MaTOSBFXDVBwmpTeolUtKGw
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 164
ht-degree: 0%

---

# Adobe Commerce 2.4.5安装

了解如何使用适用于版本2.4.5的编辑器在Adobe Commerce中安装多个新模块。 这将设置要在Adobe Commerce应用程序中使用的所需模块。 在[安装适用于Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}中找到其他文档。

## 此视频面向谁？

* 不熟悉Adobe Commerce和Adobe Developer App Builder且使用I/O事件的开发人员

## 视频内容 {#video-content}

* 使用编辑器安装所需模块
* 要为内部部署托管运行的命令
* 要为Adobe Commerce Cloud运行的命令
* Adobe Commerce Cloud Yaml需要编辑

>[!VIDEO](https://video.tv.adobe.com/v/3419825?captions=chi_hans&learn=on)

## 可用命令 {#useful-commands}

根据您是在自托管环境中还是使用Adobe Commerce Cloud，有些命令略有不同。

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


