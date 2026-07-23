---
title: 了解如何安装Adobe Commerce 2.4.6的IO事件
description: 了解如何在Adobe Commerce 2.4.6中安装用于Adobe Developer App Builder的IO事件所需的模块
jira: KT-11887
doc-type: Tutorial
duration: 136
last-substantial-update: 2023-02-22
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
TQID: https://experienceleague.adobe.com/19IVX54xo-RAJAOuXNIABdy11ExmBRAbWugi4eZ0cF0
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 141
ht-degree: 0%

---

# Adobe Commerce 2.4.6安装

了解如何使用适用于版本2.4.6的编辑器在Adobe Commerce中安装多个新模块。 在[安装适用于Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}中找到其他文档。

## 目标受众

* 刚开始使用I/O事件的Adobe Commerce和Adobe Developer App Builder的开发人员。

## 视频内容 {#video-content}

* 要为内部部署托管运行的命令
* 要为Adobe Commerce Cloud运行的命令
* Adobe Commerce Cloud Yaml需要编辑

>[!VIDEO](https://video.tv.adobe.com/v/3415795?learn=on)

## 有用的命令 {#useful-commands}

根据您是在自托管环境中还是使用Adobe Commerce Cloud，有些命令略有不同。

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

Commerce Cloud `.magento.env.yaml`：

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}


