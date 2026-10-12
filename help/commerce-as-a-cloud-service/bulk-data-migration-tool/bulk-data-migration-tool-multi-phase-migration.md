---
title: 批量数据迁移工具 — 多阶段迁移
description: 了解在生产直接转换期间源必须保持冻结时，如何使用维护模式通过批量数据迁移工具运行多阶段迁移。
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00.000Z
jira: KT-22157
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%
---

# 使用批量数据迁移工具运行多阶段迁移

当在提取期间必须冻结源环境时运行多阶段迁移 — 非常适合在迁移期间无法收到新订单的生产直接转换。 它使用维护模式，有五个阶段必须按顺序运行。 如果您的源可以保持实时状态，请观看此系列中的单相迁移视频。

## 此视频面向谁？

* 解决方案架构师
* DevOps工程师
* 后端开发人员

## 视频内容

* 在启动之前，有一个关键区别：针对迁移工具本身运行`bin/console`命令；在您的源Commerce服务器上运行`bin/magento maintenance`命令。 此工具不会为您启用或禁用维护模式 — 这是一个手动步骤。
* 第一阶段在源仍处于活动状态时运行 — `bin console migration:before-maintenance`检查配置、初始化环境、连接到CDMS、注册迁移、运行功能测试以及创建合成测试数据。 在此阶段完成之前不要启用维护模式。
* 第三阶段是从冻结环境中提取 — `bin/console migration:during-maintenance`根据需要重新打开PaaS隧道，从源提取，清除暂存视图，加载到ACCS目标，运行验证，以及清除目标上的测试数据。

>[!VIDEO](https://video.tv.adobe.com/v/3496413?learn=on)
