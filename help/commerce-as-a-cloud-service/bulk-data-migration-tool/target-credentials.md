---
title: 批量数据迁移工具 — Target凭据
description: 了解如何在运行Bulk Data Migration Tool之前在.env文件中配置目标实例URL、Adobe IMS凭据和CDMS设置。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
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

>[!VIDEO](https://video.tv.adobe.com/v/3496175?captions=chi_hans&learn=on)
