---
title: 分割支付POC：概念验证后的后续步骤
description: 了解如何将拆分付款POC移至生产环境。 Experience Cloud UI、ERP挂接、API Mesh、PHP范围、App Builder工作流和部署核对清单。
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# 分割支付POC：概念验证后的后续步骤

您在本教程中构建的演示仪表板和模拟脚本会刻意粗糙。 它们存在是为了证明这种模式。 本页介绍从概念验证到生产样式App Builder开发的实用途径。


## 第1步 — 将演示功能板替换为Experience Cloud UI扩展

`demo-dashboard` Web操作从Node.js函数内的字符串提供HTML。 它管用，但不是生产模式。

正确的替代方案是`commerce-checkout-starter-kit`中的`commerce-backend-ui-1`扩展 — 通过Adobe管理UI SDK嵌入到Commerce管理Shell中的React应用程序。 有关生成提示，请参阅[拆分付款POC：Experience Cloud UI扩展人工智能提示](./experience-cloud-ui-prompt.md)。

**更改内容：**
* 操作员通过Commerce Admin Shell登录（IMS身份验证而不是共享密钥）
* 订单列表使用IMS令牌上下文 — 无需演示密码
* 接受/拒绝操作的作用范围取决于操作员的IMS身份
* 该UI嵌入到Commerce管理员中，位于Commerce管理员已知的URL上

**保持不变的内容：**
* `payment-accept`和`payment-decline` App Builder操作未更改
* Commerce REST端点未更改
* PHP模块保持不变


## 步骤2 — 连接真正的ERP

此PoC中的接受/拒绝流由用户单击按钮来驱动。 在生产环境中，这由您的ERP 、 CRM或支付处理器驱动。

**集成模式：**
1. 您的ERP系统通过`{ orderId: <entity_id> }`捕获现金和呼叫`POST /payment-accept` （App Builder Web操作URL）
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

当前PHP模块处理必须保持处于进程中的五个事项（请参阅[拆分付款POC：架构和设计决策](./architecture-and-decisions.md)）。 随着Adobe Commerce的API表面日渐成熟，其中某些功能可能会变得可移动：

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
* [ 已在生产工作区中确认]个I/O事件注册


## 架构演变图

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


## 关键引用

* [Adobe App Builder文档](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [适用于Commerce的Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Commerce Checkout入门工具包](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [Adobe管理UI SDK](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Adobe API网格](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime(OpenWhisk)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
