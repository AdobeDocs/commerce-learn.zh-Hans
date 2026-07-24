---
title: 批量数据迁移工具 — 单相迁移
description: 了解如何使用Bulk Data Migration Tool运行单相迁移，以便进行练习和在提取期间源可以保持活动状态的环境。
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00Z
jira: KT-22139
source-git-commit: 838387ffddbd8bee3ef3ec22694818eb2de5fe2d
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# 使用批量数据迁移工具运行单相迁移

当源环境在提取期间可以保持活动状态时，运行单相迁移 — 非常适合于练习环境、开发环境或沙盒环境。 如果您需要冻结的源，例如生产直接转换，其中新订单无法在迁移期间到达，请观看本系列中的分阶段迁移视频。

## 此视频面向谁？

* 解决方案架构师
* DevOps工程师
* 后端开发人员

## 视频内容

* 使用`bin console build`构建Docker映像 — 仅在Dockerfile更改时重新运行此映像。
* 要启动CDMS CLI容器管理器，请运行`bin console start`，然后在容器中打开一个外壳程序一次以下载其依赖项。
* 要执行完整的十步管道，请运行`bin console migration`：检查配置、初始化环境、打开PaaS隧道、运行集成测试、向CDMS注册、分析目标架构、生成测试数据、提取源数据、加载到ACCS、验证校验和、清理和汇总。
* 检查迁移摘要报告 — 步骤8（数据完整性验证）记录故障而不停止管道，因此完成的运行不能保证干净的验证。
* 此单相命令是一个完整、独立的管道 — 请勿将其用作具有自己的专用命令的维护模式（分阶段迁移）工作流中的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/3496316?learn=on)
