---
title: 'Create a split payment POC: App Builder and AI tools'
description: Learn about a split payment proof of concept with App Builder and Commerce PaaS, including the goals, architecture, and what this first session covers.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, Integrations
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 259
jira: KT-20791
source-git-commit: 47b35088f2d3139d58791a2f7d327159db8f2175
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# Create a split payment POC: App Builder and AI tools

This is the first of a set of tutorialsthat introduce you to using AI-assisted development to help you build a split payment proof of concept. You work with Adobe App Builder and Adobe Commerce in the cloud (PaaS) or on-premises, moving from an overview in this session to the hands-on tutorials that follow. Expect goals, high-level architecture, and a roadmap to what the rest of the series covers.

## 视频

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## Important Details


This tutorial bridges the gap between where most Adobe Commerce teams are today — deep in PHP — and where Adobe wants them to go: event-driven, serverless business logic running outside Commerce in Adobe App Builder. It does this through a real, working feature rather than a toy example.

### What You&#39;ll Actually Build

A split payment system where customers pay using a combination of Cash on Delivery and Store Credit. After the order is placed, an operator (or ERP system) confirms or declines the cash payment through a standalone dashboard — without ever opening Commerce Admin. The entire accept/decline workflow runs in App Builder.
The Architectural Lesson (This Is the Core Teaching)
The tutorial demonstrates a deliberate and repeatable decision framework:

What must stay in PHP: anything that runs synchronously in the Commerce request cycle, or that calls Commerce-internal APIs with no clean external surface
What moves to App Builder: everything else — event processing, operator workflow, external integrations, and operator-facing tools

This isn&#39;t &quot;move everything to App Builder.&quot; It&#39;s a practical, honest starting point for teams who need to begin the transition without a rewrite.

### Why the PHP Code Isn&#39;t Included

The AI prompt approach is actually better than sample code for this use case, because the prompt captures the behavioral contracts and architectural reasoning that sample code alone cannot convey. A developer who runs the prompt and reads the output understands why the code is shaped the way it is, not just what it looks like.

### What Is Included

Complete App Builder application code (consistent across projects — use it directly or as a reference)
A full set of numbered AI prompts designed for Cursor and Claude, covering the Commerce module, the App Builder orchestrator, the operator dashboard, testing, and the path to production
在不需要实际ERP的情况下针对实时Commerce站点测试接受/拒绝流的模拟脚本
体系结构文档解释每一项决策

### 这是给谁的

Adobe Commerce内部部署或Commerce Cloud上的开发人员，他们正开始其第一个真正的App Builder集成。 并非对于完全以Headless API为先的部署 — 专门针对过渡中的团队，这些团队经营着传统的店面，他们希望了解如何在不放弃现有PHP投资的情况下将真实的业务逻辑卸载到App Builder。

### 先决条件标注

Adobe Commerce 2.4.5或更高版本、对具有App Builder项目且启用了I/O事件的Adobe Developer Console组织的访问权限。 为签出UI假定使用干净的Luma主题 — 自定义主题需要在运行它之前调整提示。

### 最后想法

这应被视为关于人工智能辅助发展的入门级讨论。 本教程演示了如何在Commerce开发工作流中使用AI工具，而不仅仅是有关拆分支付的教程。
