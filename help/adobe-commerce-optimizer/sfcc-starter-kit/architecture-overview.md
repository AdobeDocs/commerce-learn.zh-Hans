---
title: Salesforce Commerce Cloud Connector应用程序的架构概述
description: 了解Salesforce Commerce Cloud与Adobe Commerce Optimizer的架构。
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Architect, Developer
level: Beginner
doc-type: Technical Video
duration: 243
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: fa615aab7b8eff3b13908797cd263ec4cdc65eb6
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# 了解Salesforce Commerce Cloud入门套件架构

了解Commerce Optimizer Connector Starter Kit的架构和功能，该工具包集成了Salesforce Commerce Cloud (SFCC)、Adobe App Builder。 Adobe Commerce Optimizer使用入门套件来简化Edge Delivery店面的目录同步。 它解释了SFCC中的自定义磁带盒如何通过增量导出文件检测目录更改并通过自定义API公开它们。 App Builder运行时操作（包括同步和异步）会使用这些更改来执行完全同步和增量同步、元数据更新以及特定于产品的同步。 该系统还包括验证工具，以确保店面准确性，并使用App Builder的状态管理来跟踪同步状态并防止冲突。

## 此视频面向谁？

* Commerce解决方案架构师
* 技术营销工程师
* 电子商务平台管理员

## 视频内容

* 自定义SFCC墨盒和API通过增量导出检测目录更改，从而实现与Adobe App Builder的有效数据同步。
* App Builder运行时操作可管理完全同步和增量同步、验证和状态跟踪，以确保对Commerce Optimizer的更新准确且没有冲突。

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)
