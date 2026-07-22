---
title: 批量数据迁移工具 — Source凭据
description: 了解如何在运行批量数据迁移工具之前在.env文件中配置源实例URL和身份验证凭据。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22095
source-git-commit: a785518a36cda9d2bfb82951c26f2e197ee3d43e
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
* 如果您的源是Magento as a Cloud Service (PaaS)，请从account.magento.cloud检索Adobe Commerce CLI令牌。

>[!VIDEO](https://video.tv.adobe.com/v/3496142?learn=on)
