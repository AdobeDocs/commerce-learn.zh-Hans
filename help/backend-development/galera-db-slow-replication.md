---
title: 了解如何在mysql慢查询日志中查找慢查询，以及Galera DB复制设计方法可能是原因的原因
description: Galera DB的设计方法使数据复制到辅助数据库的时间比复制到主数据库的时间长。 了解如何在mysql慢查询日志中找到这些事件，以及在慢查询日志中看到条目的根本原因，以及将来如何防止这些事件。
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
source-git-commit: 598bff1fd2cefdc449d5ae3431401aec1e796313
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# 了解Galera DB复制和相关MySQL慢查询

Galera群集有助于提高性能和可扩展性。 在考虑辅助数据库时，必须了解数据复制的方式与在主数据库上的不同。 主数据库可以执行批量操作。 当针对所有辅助数据库进行复制时，它们一次执行一个操作。 例如，如果在删除操作中有67,000,000个项目，则在辅助数据库中，每个项目一次出现一个。 查看Mysql慢查询日志时，您会发现此操作可能需要很长时间。 由于辅助数据库一次执行一项操作，因此会使得这些操作不同步，并且可以检测性能影响。

如果可能，作为一种解决方案，可以对大型操作进行批处理以帮助辅助数据库与主数据库保持同步。 通过批量执行操作，可以及时执行操作，并将对性能的影响降至最低。

## 此视频面向谁？

- 架构师
- 开发人员
- DevOps

## 视频内容

- 到辅助数据库的常规复制
- 了解流量控制
- 在mysql慢查询日志中查找线程号
- 仅在主系统上执行批量操作。 一次只进行复制
- 对大型承诺进行批处理以帮助复制与主要承诺保持同步

>[!VIDEO](https://video.tv.adobe.com/v/3423537?learn=on&captions=chi_hans)

## 有用的资源

- [Galera群集](https://galeracluster.com/)
