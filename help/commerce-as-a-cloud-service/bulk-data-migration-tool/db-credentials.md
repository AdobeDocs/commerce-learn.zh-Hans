---
title: 批量数据迁移工具 — 数据库凭据
description: 了解在运行迁移工具之前，如何使用Magento Cloud CLI或项目ID在.my.cnf文件中配置源数据库连接。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
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
source-wordcount: '151'
ht-degree: 0%
---
# 为批量数据迁移工具配置数据库凭据

在运行批量数据迁移工具之前，请在`.my.cnf`文件中设置源数据库连接。 根据您的源环境是内部部署还是Adobe Commerce as a Cloud Service (PaaS)，这些步骤会有所不同。

## 此视频面向谁？

* 解决方案架构师
* DevOps工程师
* 后端开发人员

## 视频内容

* 将`.my.cnf.example`复制到`.my.cnf`并为您的源连接创建一个名为的新分区。
* 如果您的源是Adobe Commerce as a Cloud Service (PaaS)，请在`.my.cnf`中设置项目ID。
* 使用Magento Cloud CLI通道命令获取主机、用户、密码、端口和数据库值。
* 如果您的源是内部部署的，请在运行该工具之前确认主机和端口连接。

>[!VIDEO](https://video.tv.adobe.com/v/3496165?captions=chi_hans&learn=on)
