---
title: 批量数据迁移工具 — Target凭据
description: 了解如何在运行Bulk Data Migration Tool之前在.env文件中配置目标实例URL、Adobe IMS凭据和CDMS设置。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22107
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
source-wordcount: '173'
ht-degree: 0%
---
# 为批量数据迁移工具配置目标凭据

在运行批量数据迁移工具之前，请在`.env`文件中设置目标实例URL、Adobe IMS凭据和CDMS设置。 确保您的Adobe IMS URL、目标URL和CDMS主机都匹配相同的环境层 — 暂存或生产。

## 此视频面向谁？

* 解决方案架构师
* DevOps工程师
* 后端开发人员

## 视频内容

* 使用experience.adobe.com上实例信息面板中的值，在`.env`文件中设置目标实例REST和GraphQL URL以及目标租户ID。
* 设置Adobe IMS URL以匹配您的环境层（暂存或生产）和区域。
* 在Adobe Developer Console中从&#x200B;**Project** > **OAuth服务器到服务器**&#x200B;检索Adobe IMS客户端ID和客户端密钥。
* 复制目标组织ID并配置CDMS主机、端口和本地服务器设置以匹配您的环境。

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)
