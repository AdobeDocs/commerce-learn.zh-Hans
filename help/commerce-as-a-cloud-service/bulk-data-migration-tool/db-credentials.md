---
title: 批量数据迁移工具 — 数据库凭据
description: 了解在运行迁移工具之前，如何使用Magento Cloud CLI或项目ID在.my.cnf文件中配置源数据库连接。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22105
source-git-commit: 0dcb41e9138a36528f10333b0b5a9a9b2a39ed40
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

>[!VIDEO](https://video.tv.adobe.com/v/3496152?learn=on)
