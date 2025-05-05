---
title: 诊断和修复一些常见的Commerce Cloud错误
description: 解决两个阻止站点加载的常见AdobeCloud项目错误。
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
source-git-commit: 27c1715dd42853013181d9c729549a5a32bc2af0
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---


# 诊断并修复服务不可用，并出现错误

了解如何分类并解决在Adobe Commerce Cloud项目中看到的两个常见错误。  了解这些错误发生的方式和原因，以及解决这些错误的建议步骤。

## 此视频面向谁

- 开发人员和IT专业人员
- 系统管理员

## 视频内容

- 诊断并解决存储问题：
- 管理维护模式
- 有效的故障排除提示

>[!VIDEO](https://video.tv.adobe.com/v/3447703?learn=on&captions=chi_hans)


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
