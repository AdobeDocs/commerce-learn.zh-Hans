---
title: 了解如何在mysql慢查询日志中查找慢查询，以及Galera DB复制设计方法可能是原因的原因
description: Galera DB的设计方法使数据复制到辅助数据库的时间比复制到主数据库的时间长。 了解如何在mysql慢查询日志中找到这些事件，以及在慢查询日志中看到条目的根本原因，以及将来如何防止它们。
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: ae85993d98fb8620b763a212e5a2d7e53596870f
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# 了解Galera DB复制和相关MySQL慢查询

Galera群集有助于提高性能和可扩展性。 在考虑辅助数据库时，请务必了解数据复制的方式与在主数据库上的不同。 主数据库可以执行批量操作。 当对所有辅助数据库进行复制时，它们一次执行一个操作。 例如，如果在删除操作中有67,000,000个项目，则在辅助数据库中，每个项目一次发生一次。 查看Mysql慢查询日志时，您发现此操作可能需要很长时间。 由于辅助数据库每次执行一项操作，因此是不同步的原因之一，并且可以检测到性能影响。

如果可能，作为一种解决方案，可以对大型操作进行批处理以帮助辅助数据库与主数据库保持同步。 通过批量执行操作，可以及时执行操作，并将对性能的影响降至最低。

## 此视频面向谁？

- 架构师
- 开发人员
- DevOps

## 视频内容

- 到辅助数据库的常规复制
- 了解流量控制
- 在mysql慢查询日志中查找线程号
- 批量执行仅在主节点上执行。 一次执行1个复制
- 对大型提交进行批处理以帮助复制跟上主要的

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## 有用的资源

- [Galera集群](https://galeracluster.com/)
