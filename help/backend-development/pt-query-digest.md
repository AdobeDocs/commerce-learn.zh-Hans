---
title: 了解Percona Toolkit pt-query-digest的工作方式及其使用原因
description: 从慢速、常规和二进制日志文件分析MySQL查询。 它还可以分析来自“SHOW PROCESSLIST”的查询以及来自tcpdump的MySQL协议数据。
kt: 13846
doc-type: video
duration: 510
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
TQID: https://experienceleague.adobe.com/lh-fBjlhZO6W-K08KNb-KaG-N2slLZVpNOSg6LAp0n8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 113
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

了解为何使用pt-query-digest和一些现实示例来加深推理。

## 此视频面向谁？

* 架构师
* 开发人员
* DevOps

## 视频内容

* 了解pt-query-digest用法
* 了解此Percona Toolkit功能的优点和缺点
* 了解结果并了解应考虑哪些可能的性能步骤

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## 代码引用

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 有用的资源

* [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
