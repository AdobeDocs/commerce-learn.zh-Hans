---
title: 拆分支付POC — App Builder和AI工具
description: 了解使用App Builder和Commerce PaaS的分段支付概念验证，包括目标、架构以及本次第一场会议涵盖的内容。
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 237
jira: KT-20791
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 0%

---

# 创建拆分支付POC：App Builder和AI工具

这是一系列教程中的第一个，这些教程向您介绍使用AI辅助开发来帮助您构建分割支付概念验证。 您可以在云中(PaaS)或内部部署中使用Adobe App Builder和Adobe Commerce，从此会话的概述转移到后面的实践教程。 期望目标、高级架构以及系列其他涵盖内容的路线图。

## 视频

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## 重要详细信息


本教程弥合了大多数Adobe Commerce团队的现状（在PHP中非常深刻）与Adobe希望他们走向何方（在Adobe App Builder中的Commerce之外运行事件驱动的无服务器业务逻辑）之间的差距。 它通过真实的工作功能而不是玩具来做到这一点。

### 您实际将构建的内容

客户使用货到付款和商店贷记的组合进行支付的分割支付系统。 下订单后，操作员（或ERP系统）通过独立仪表板确认或拒绝现金支付，而无需打开Commerce Admin。 整个接受/拒绝工作流将在App Builder中运行。

#### 建筑课（核心教学）

本教程演示了一个精心设计且可重复的决策框架：

* **在PHP中必须保留的内容：**&#x200B;在Commerce请求周期中同步运行的任何内容，或者调用没有干净外部表面的Commerce内部API的任何内容
* **哪些内容移至App Builder：**&#x200B;其他内容 — 事件处理、操作员工作流、外部集成和面向操作员的工具

这不是“将所有内容移至App Builder”。 对于需要在不重写的情况下开始转变的团队来说，这是一个实用、诚实的起点。

### 为何不包含PHP代码

对于此用例，AI提示方法实际上比示例代码更好，因为提示捕获了仅示例代码无法传达的行为约定和架构推理。 运行提示并读取输出的开发人员了解代码为何按原样塑造，而不仅仅是按原样塑造。

### 包含的内容

* 完成App Builder应用程序代码（跨项目一致 — 直接使用它或将其用作引用）
* 为Cursor和Claude设计的一整套带编号的人工智能提示，包括Commerce模块、App Builder Orchestrator、操作员仪表板、测试和生产路径
* 在不需要实际ERP的情况下针对实时Commerce站点测试接受/拒绝流的模拟脚本
* 体系结构文档解释每一项决策

### 这是给谁的

Adobe Commerce内部部署或Commerce Cloud上的开发人员，他们正开始其第一个真正的App Builder集成。 并非对于完全以Headless API为先的部署 — 专门针对过渡中的团队，这些团队经营着传统的店面，他们希望了解如何在不放弃现有PHP投资的情况下将真实的业务逻辑卸载到App Builder。

### 先决条件标注

Adobe Commerce 2.4.5或更高版本、对具有App Builder项目且启用了I/O事件的Adobe Developer Console组织的访问权限。 为签出UI假定使用干净的Luma主题 — 自定义主题需要在运行它之前调整提示。

### 最后想法

这应被视为关于人工智能辅助发展的入门级讨论。 本教程演示了如何在Commerce开发工作流中使用AI工具，而不仅仅是有关拆分支付的教程。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
