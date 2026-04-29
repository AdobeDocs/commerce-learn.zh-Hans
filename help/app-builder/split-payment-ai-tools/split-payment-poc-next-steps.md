---
title: 分割支付POC：概念验证后的后续步骤
description: 'Learn how to move the split payment POC toward production: Experience Cloud UI, ERP hooks, API Mesh, PHP scope, App Builder workflows, and deploy checklist.'
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# 分割支付POC：概念验证后的后续步骤

The demo dashboard and simulation script you built in this tutorial are intentionally rough. They exist to prove the pattern. This page describes a practical path from proof of concept to production-style App Builder development.


## Step 1 — Replace the Demo Dashboard with an Experience Cloud UI Extension

The `demo-dashboard` web action serves HTML from a string inside a Node.js function. It works, but it is not the production pattern.

The proper replacement is the `commerce-backend-ui-1` extension in the `commerce-checkout-starter-kit` — a React application embedded in the Commerce Admin Shell via the Adobe Admin UI SDK. See [Split payment POC: Experience Cloud UI extension AI prompt](split-payment-poc-experience-cloud-ui-prompt.md) for the generation prompt.

**What changes:**
* Operators log in through Commerce Admin Shell (IMS authentication instead of a shared secret)
* The order list uses the IMS token context — no need for a demo secret
* Accept/decline actions are scoped to the operator&#39;s IMS identity
* The UI is embedded in Commerce Admin at the URL Commerce administrators already know

**What stays the same:**
* `payment-accept` and `payment-decline` App Builder actions are unchanged
* Commerce REST endpoints are unchanged
* The PHP module is unchanged


## Step 2 — Connect a Real ERP

The accept/decline flow in this PoC is driven by a human clicking a button. In production, this is driven by your ERP, CRM, or payment processor.

**The integration pattern:**
1. Your ERP system captures cash and calls `POST /payment-accept` (the App Builder web action URL) with `{ orderId: <entity_id> }`
2. App Builder验证调用（持有者令牌或API密钥 — 将身份验证中间件添加到`payment-accept`）
3. App Builder调用Commerce REST `cash-received`
4. Commerce开具发票并发送订单

无需更改PHP。 REST表面已经存在。 App Builder操作只需要安全调用方，而不是仪表板按钮。

ERP调用方的&#x200B;**身份验证选项：**
* Adobe I/O Runtime支持`require-adobe-auth: true`作为IMS令牌（如果ERP可以获取IMS令牌）
* 对于非Adobe系统：在`payment-accept`操作中添加简单的API密钥检查（根据操作环境中存储的密钥检查标头）


## 步骤3 — 将API网格添加为代理层

目前，App Builder使用OAuth 1.0a凭据直接调用Commerce REST。 对于大型部署，Adobe API Mesh在App Builder和Commerce之间提供了一个托管网关层。

**福利：**
* 集中式凭据管理 — API Mesh包含Commerce凭据；App Builder调用API Mesh
* 请求转换 — 在不更改App Builder操作的情况下将App Builder有效负载映射到Commerce REST形状
* 速率限制和缓存 — 保护Commerce免受高流量事件流量的影响

**模式：**
* 将`createCommerceClient(params, logger)`调用替换为对API网格端点的调用
* API Mesh处理对Commerce的OAuth签名


## 步骤4 — 减少PHP占用的空间

当前PHP模块处理必须保持处于进程中的五个事项（请参阅[拆分付款POC：架构和设计决策](split-payment-poc-architecture-and-decisions.md)）。 随着Adobe Commerce的API表面日渐成熟，其中某些功能可能会变得可移动：

**将来可能可以移动：**
* 商店积分REST API正在不断发展 — 未来的版本可能支持在订单后应用积分或将其应用到非活动的购物车
* 随着更多Commerce操作变得非同步安全，阈值保护可以移动到预排序的API Mesh解析器

**不可移动（架构约束）：**
* 除非Commerce通过API优先扩展点公开干净的挂接，否则`placeOrder()`之前的报价操作将始终需要进程内代码
* REST端点(`/V1/split-payment/*`)特定于此功能；它们驻留在Commerce中，因为它们调用Commerce内部服务


## 步骤5 — 向App Builder添加更多工作流

当前的PoC执行一项操作：侦听订单下达，然后启用“接受/拒绝”。 自然扩展：

接受前&#x200B;**欺诈得分：**
在`payment-orchestrator`中，在记录待处理现金后，先调用欺诈评分API，然后再将编排结果视为最终结果。 如果分数超过阈值，则自动拒绝而不是等待操作员操作。

**通知电子邮件：**
当`payment-accept`成功时，会触发一封电子邮件（通过Adobe Campaign、SendGrid或任何HTTPS API），通知客户其现金付款已确认。

**忠诚度积分奖励：**
确认现金后，调用忠诚度API以奖励积分。 这是纯粹的App Builder — 不需要PHP。

**超时处理：**
添加一个计划的App Builder操作（使用`app.config.yaml`中的`cron`），该操作扫描超过X天的`split_cash_status = 'pending'`订单并自动拒绝它们。


## 步骤6 — 部署到生产

已为`Stage`工作区配置PoC。 移至生产环境：

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**生产准备清单：**
* [ 已设置] `DEMO_UI_SECRET`（或已使用Experience Cloud UI替换演示仪表板）
* [ 生产环境中的] `LOG_LEVEL=warn`或`error` （不是`debug`）
* [ ] `PAYMENT_THRESHOLD`与Commerce生产配置匹配
* [ `.env`中的]个Commerce集成凭据用于专用生产集成（非暂存）
* [ ] Fastly IP已使用App Builder生产出口IP (Commerce Cloud)更新
* [ ] I/O Event registration confirmed in production workspace


## Architecture Evolution Diagram

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## Key References

* [Adobe App Builder documentation](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Adobe I/O Events for Commerce](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Commerce Checkout Starter Kit](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [Adobe Admin UI SDK](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Adobe API Mesh](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime (OpenWhisk)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
