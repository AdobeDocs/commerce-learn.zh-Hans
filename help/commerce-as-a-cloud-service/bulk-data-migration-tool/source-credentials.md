---
title: 批量数据迁移工具 — Source凭据
description: 了解如何在运行批量数据迁移工具之前在.env文件中配置源实例URL和身份验证凭据。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22095
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%
---
# 为批量数据迁移工具配置源凭据

在运行批量数据迁移工具之前，请在`.env`文件中设置源实例URL和身份验证凭据。 身份验证步骤略有不同，具体取决于您的源环境是内部部署还是Adobe Commerce as a Cloud Service (PaaS)。

## 此视频面向谁？

* 解决方案架构师
* DevOps工程师
* 后端开发人员

## 视频内容

* 在`.env`文件中设置源实例URL以及REST和GraphQL URL。
* 在Adobe Commerce管理中从&#x200B;**系统** > **扩展** > **集成**&#x200B;检索或创建集成密钥。
* 要生成四个所需的令牌，请激活集成。
* 如果您的源是Adobe Commerce as a Cloud Service (PaaS)，请从account.magento.cloud检索Magento CLI令牌。

>[!VIDEO](https://video.tv.adobe.com/v/3496142?learn=on)
