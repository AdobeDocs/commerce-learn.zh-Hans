---
title: 将表添加到数据库
description: '"[!DNL Commerce] 有一种特殊机制，可让您创建数据库表、修改现有表，甚至向其中添加一些数据。”'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: acee5ba84ea32e14a743cd269f77ced821548ad6
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# 将表添加到数据库

[!DNL Commerce] 有一种特殊机制，允许您创建数据库表、修改现有表，甚至向其中添加一些数据 — 例如安装数据，安装模块时必须添加这些数据。 此机制允许这些更改在不同安装之间转移。

开发人员不会在重新安装系统时反复执行手动SQL操作，而是创建一个包含数据的安装（或升级）脚本。 每次安装模块时，脚本都会运行。

## 这个视频给谁？

- 开发人员

## 视频内容

- 创建模块
- 创建InstallSchema脚本
- 创建InstallData Script
- 添加新模块并验证是否已创建包含数据的表
- 创建UpgradeSchema脚本
- 创建UpgradeData脚本
- 运行升级脚本并验证表是否已更改

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 有用资源

- [迁移命令](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/migration-commands.html)
