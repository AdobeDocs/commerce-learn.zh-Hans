---
title: 向数据库添加表
description: ”[!DNL Commerce] 有一种特殊的机制使您能够创建数据库表、修改现有表，甚至向表中添加一些数据。”
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: e8d2631b31319701beb327f42fdf1372d9dad9b7
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 向数据库添加表

>[!IMPORTANT]
>
>不再建议这样做，请参阅https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] 具有一种特殊的机制，允许您创建数据库表、修改现有表，甚至可以向表中添加一些数据，如安装数据，安装模块时必须添加这些数据。 此机制允许在不同的安装之间传输这些更改。

开发人员创建包含数据的安装（或升级）脚本，而不是在重新安装系统时重复执行手动SQL操作。 每次安装模块时，该脚本都会运行。

## 此视频面向谁？

- 开发人员

## 视频内容

- 创建模块
- 创建InstallSchema脚本
- 创建InstallData脚本
- 添加新模块并验证是否创建了包含数据的表
- 创建UpgradeSchema脚本
- 创建UpgradeData脚本
- 运行升级脚本并验证表是否已更改

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 有用的资源

- [将安装/升级脚本迁移到声明性架构](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
