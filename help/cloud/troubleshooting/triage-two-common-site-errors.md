---
title: 诊断和修复一些常见的Commerce Cloud错误
description: 解决两个阻止加载站点的常见Adobe Cloud项目错误。
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 297
last-substantial-update: 2024-10-30T00:00:00.000Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
TQID: https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 135
ht-degree: 0%

---

# 诊断并修复服务不可用，并出现错误

了解如何分类并解决在Adobe Commerce Cloud项目上看到的两个常见错误。  了解这些错误发生的方式和原因，以及解决这些错误的建议步骤。

## 此视频面向谁

* 开发人员和IT专业人员
* 系统管理员

## 视频内容

* 诊断并解决存储问题：
* 管理维护模式
* 有效的故障排除提示

>[!VIDEO](https://video.tv.adobe.com/v/3447703?captions=chi_hans&learn=on)


## 视频中使用的命令

查找响应消息中提到的例外日志的最后5行。

```SHELL
 tail -n 5 ~/var/log/exception.log
```

检查硬盘空间。 请注意行dev/mapper/xxxx

```SHELL
df -h
```

让我们查找前15个最大文件

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

显示维护模式的状态

```SHELL
php bin/magento maintenance:status
```

禁用维护模式

```SHELL
php bin/magento maintenance:disable 
```
