---
title: 瞭解如何安裝Adobe Commerce 2.4.5的IO事件
description: 瞭解如何在Adobe Commerce 2.4.5中安裝IO事件所需的模組，以用於Adobe Developer App Builder
landing-page-description: 瞭解如何使用撰寫器安裝Adobe Commerce 2.4.5所需的數個模組。
short-description: 瞭解如何使用撰寫器安裝Adobe Commerce 2.4.5所需的數個模組。
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

# Adobe Commerce 2.4.5安裝

瞭解如何使用2.4.5版的Composer，在Adobe Commerce中安裝數個新模組。這會設定在Adobe Commerce應用程式中使用的必要模組。 其他檔案可在下列網址找到： [安裝Adobe Commerce的Adobe I/O事件](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 這部影片是給誰看的？

* 不熟悉Adobe Commerce和Adobe Developer App Builder （使用I/O事件）的開發人員

## 視訊內容 {#video-content}

* 使用Composer安裝必要模組
* 針對內部部署託管執行的命令
* 針對Adobe Commerce Cloud執行的命令
* Adobe Commerce Cloud yaml必要編輯

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## 有用的命令 {#useful-commands}

有些命令稍微有所不同，取決於您是在自行託管的環境中或使用Adobe Commerce Cloud。

### 內部部署託管 {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### 雲端上的Adobe Commerce {#adobe-commerce-cloud}

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
