---
title: 配置Adobe Commerce
description: 了解如何配置Adobe Commerce以允许在Adobe Developer App Builder中使用事件。
jira: KT-11889
doc-type: Tutorial
duration: 268
last-substantial-update: 2023-02-21
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 114
ht-degree: 0%

---

# 配置Adobe Commerce

了解如何配置Adobe Commerce以公开事件。 在[安装适用于Adobe Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}中找到其他文档。

## 此视频面向谁？

* 刚开始接触Adobe Commerce和Adobe Developer App Builder的开发人员，他们使用的I/O事件需要创建Adobe App Builder项目。

## 视频内容 {#video-content}

* 在Commerce管理员中配置Adobe I/O Events
* 在Commerce管理员中保存私钥
* 在Commerce管理员中保存唯一标识符
* 创建事件提供程序

>[!VIDEO](https://video.tv.adobe.com/v/3415799?learn=on)

## 有用的命令 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}


