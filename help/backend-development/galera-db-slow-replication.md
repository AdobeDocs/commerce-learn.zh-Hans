---
title: 在MySQL慢查询日志中诊断Galera数据库复制
description: 了解Galera DB的复制设计如何减慢辅助数据库同步，如何在MySQL慢查询日志中识别这些事件，以及如何最大程度地降低影响。
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 262
ht-degree: 0%

---

# 了解Galera DB复制和相关MySQL慢查询

Galera群集有助于提高性能和可扩展性。 在考虑副本数据库时，请务必了解数据复制的方式与主数据库上的不同。 主数据库可以执行批量操作。 当针对所有副本数据库进行复制时，它们一次执行一个操作。 例如，如果在删除操作中有67,000,000个项目，则在副本数据库中，每个项目一次发生一次。 查看MySQL慢查询日志时，您会发现此操作可能需要很长时间。 副本数据库按顺序执行操作这一事实是不能同步的原因，并且可以检测性能影响。

为帮助副本数据库与主数据库保持同步，请尽可能对大型操作进行批处理操作。 通过批量执行操作，可以及时执行操作，并将对性能的影响降至最低。

## 目标受众

* 架构师
* 开发人员
* DevOps

## 视频内容

* 到副本数据库的常规复制
* 了解流量控制
* 在mysql慢查询日志中查找线程号
* 仅在主系统上执行批量操作。 一次只进行复制
* 为帮助复制与主要复制保持同步，请批量处理您的大型提交。

>[!VIDEO](https://video.tv.adobe.com/v/3423537?captions=chi_hans&learn=on)

## 有用的资源

* [Galera群集](https://mariadb.com/products/enterprise/galera-cluster/)
