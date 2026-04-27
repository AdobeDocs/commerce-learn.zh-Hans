---
title: 了解mysql查询缓存的方式
description: 有时mysql查询会被备份，等待锁定。 本教程介绍什么是查询缓存，以及在遇到问题时提供的一些设置建议。
kt: 13690
doc-type: video
duration: 444
activity: use
last-substantial-update: 2023-7-27
feature: Backend Development, Cache, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 8d3b0ec2-e80c-4457-b924-69e8b8cedf03
TQID: https://experienceleague.adobe.com/W91-fJGZtgfpp03ZtYmSh97oNqmCFpF8AvkTYDaOB-g
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 165
ht-degree: 0%

---

# 了解mysql查询缓存

了解什么是MySQL查询缓存，以及它的工作方式的一些基本了解。 了解如何通过发现mysql慢查询日志中的大量出现“正在等待查询缓存锁定”来检测mysql查询缓存问题。

## 此视频面向谁？

* 架构师
* 开发人员
* DevOps

## 视频内容

* 了解查询缓存
* 如何通过发现“等待查询缓存锁定”来检测查询缓存设置是否可能存在问题
* 了解如何保存和使用SQL查找匹配的查询缓存
* 有关配置设置的一些提示

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## 有用的资源

* [一般MySQL准则](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql.html?lang=zh-Hans){target="_blank"}
* [galera复制和查询缓慢](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication.html?lang=zh-Hans){target="_blank"}
