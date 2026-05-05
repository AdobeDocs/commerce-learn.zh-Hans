---
title: 拆分付款POC：App Builder orchestrator AI提示
description: 了解如何使用此提示来构建拆分付款Orchestrator应用程序。 I/O事件、支付 — orchestrator、Web操作、演示仪表板，以及aio应用程序部署。
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# 拆分付款POC：App Builder orchestrator AI提示

使用此页面复制生成&#x200B;**split-payment-orchestrator**&#x200B;项目的完整提示：**payment-orchestrator** I/O事件消费者、**payment-accept**&#x200B;和&#x200B;**payment-decline** Web操作、**demo-dashboard**&#x200B;以及Commerce REST客户端。

## 如何使用此提示

将从&#x200B;**PROMPT START**&#x200B;到&#x200B;**提示**&#x200B;结尾的所有内容复制到光标（使用Claude）或直接复制到Claude。 从`split-payment-orchestrator/`目录（App Builder项目根目录）运行提示。

## 运行之前

* 完成[拆分付款POC：先决条件和环境设置](./prerequisites-and-setup.md)。
* 在项目中准备好您的[拆分付款POC：环境变量引用](./env-reference.md)和`.env`文件。


## 提示

**提示开始**


您正在生成一个完整的Adobe App Builder应用程序，用于在Adobe Commerce中编排拆分支付。 该应用程序接收来自Commerce的I/O事件，处理拆分支付决策，并通过REST回调到Commerce。

**项目：** `split-payment-orchestrator`
**运行时：** Node.js 18
**键依赖项：** `@adobe/aio-sdk ^6.0.0`，`got ^11.8.6`，`oauth-1.0a ^2.2.6`

生成下面列出的所有文件。 应用程序必须与Adobe I/O Runtime (`aio app deploy`)配合使用。


### 要生成的文件结构

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

在`split_payment_orchestrator`包中定义四个操作：

**`payment-orchestrator`**
* `web: "no"` （仅I/O事件触发器 — 不能通过HTTP直接调用）
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 来自环境的输入： `LOG_LEVEL`、`COMMERCE_BASE_URL`、`COMMERCE_CONSUMER_KEY`、`COMMERCE_CONSUMER_SECRET`、`COMMERCE_ACCESS_TOKEN`、`COMMERCE_ACCESS_TOKEN_SECRET`、`PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` （HTTP Web操作 — 可由仪表板或ERP调用）
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 相同的Commerce凭据输入（无`PAYMENT_THRESHOLD`）

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 输入的Commerce凭据相同

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false`←仪表板可公开访问（设置后仅受`DEMO_UI_SECRET`保护）
* 输入：所有Commerce凭据+ `DEMO_UI_SECRET`，`DEMO_UI_BASE_URL`

**事件注册** （在`events.registrations`下）：

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

为Adobe Commerce共享了OAuth 1.0a REST客户端。 实施：

**`createCommerceClient(params, logger)`** — 返回`{ request, baseUrl }`

* 从`params`读取`COMMERCE_BASE_URL`、`COMMERCE_CONSUMER_KEY`、`COMMERCE_CONSUMER_SECRET`、`COMMERCE_ACCESS_TOKEN`、`COMMERCE_ACCESS_TOKEN_SECRET`
* 如果缺少任何凭据，则抛出
* 将`oauth-1.0a`与`HMAC-SHA256`一起使用(Node.js `crypto.createHmac`)
* 使用`got@11` (不是`got@12+` — 项目使用CJS和`prefixUrl = ${baseUrl}/rest/V1/`
* 通过`beforeRequest`挂接添加`Authorization`标头
* `request(method, path, options)` — 标准化路径（剥离前导`/`），返回`{ statusCode, body }`
* `throwHttpErrors: false` — 从不在4xx/5xx上抛出；始终返回状态代码


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — 返回`{ pass: boolean, reason: string }`

逻辑：
1. 读取`params.PAYMENT_THRESHOLD`；解析为浮点数；如果缺少，则默认为`100`、NaN或≤0
2. 如果`orderTotal > threshold`：返回`{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`
3. 如果`Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`：返回`{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`
4. 否则：返回`{ pass: true, reason: '' }`


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — 返回`{ ok: boolean, error? }`

* 通过以下方式调用`POST orders/${orderId}/comments`：

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* 在2xx上返回`{ ok: true }`；否则返回`{ ok: false, error: { code, message } }`
* 在try/catch中打包；返回错误对象，从不抛出


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — 返回`{ ok: boolean, error? }`

* 如果`success`：发布历史记录评论`"Split payment orchestration completed. Order awaiting cash confirmation."`
* 如果`!success`：帖子`"Payment could not be processed. Please try again or contact support."` — 从不将`detail`包含在客户可见的评论中；仅内部记录`detail`
* 返回`{ ok: boolean, error? }` — 从不抛出


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`** — 已弃用的无操作实施

将此文件与JSDoc `@deprecated`通知一起包含在内，说明：
> orchestrator不再通过REST应用商店点数。 使用`BalanceManagementInterface::apply()`在Commerce PHP模块(`PlaceOrderPlugin`)中结帐时应用商店点数，这需要有效的购物车。 到App Builder收到I/O事件时，购物车处于不活动状态。 此文件将保留以供参考或用于自定义流程，其中存储点数在订购后应用。

保持工作实施（与其他模块形状相同），以便开发人员可以研究该模式，但请将其明确标记为未在当前流程中使用。


### `actions/payment-orchestrator/index.js`

I/O事件入口点。 实现`async function main(params)`。

**有效负载提取：**

Adobe Commerce I/O事件可能会以多种形式交付有效负载。 通过按顺序检查这些路径来提取order值对象：
1. `params.__ow_body` （如果需要，可解析JSON字符串）→ `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. 回退到`params`本身

**字段提取自值：**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* 从`value.extension_attributes`拆分金额（同时检查`extension_attributes`和`extensionAttributes`）：
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**流量：**
1. 提取值；如果缺少`orderId`，则记录错误并返回`{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`
2. 调用`evaluateThreshold(...)` — 如果失败，则记录并返回相同的200响应
3. 调用`createCommerceClient(params, logger)` — 如果失败，则返回200错误
4. 如果`storeCredit > 0`，则在签出时记录应用商店点数（无需REST调用）
5. 调用`recordCashPending(...)` — 如果失败，则调用`updateOrderAfterOrchestration(..., success: false)`并返回200错误
6. 呼叫`updateOrderAfterOrchestration(..., success: true)`
7. 返回`{ statusCode: 200, body: { ok: true, message: 'processed' } }`

**重要提示：**&#x200B;始终返回`statusCode: 200` — I/O运行时将重试非200响应，这将导致重复订单处理。 错误报告正文中。

**`PUBLIC_ERROR`常量：** `"Payment could not be processed. Please try again or contact support."` — 用于所有面向外部的错误消息。


### `actions/payment-accept/index.js`

HTTP Web操作。 调用`POST /V1/split-payment/orders/:orderId/cash-received`。

**订单ID解析：**&#x200B;依次检查`params.orderId`、`params.payload.orderId`和`params.__ow_body`（已解析的JSON）。 如果缺少，则返回400。

**流量：**
1. 解析`orderId`；如果缺少，则返回400
2. 初始商务客户端；如果失败，则返回500
3. 调用JSON正文为空的`POST split-payment/orders/${orderId}/cash-received`
4. 如果2xx：返回`{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }`
5. 如果错误：记录并返回`{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`


### `actions/payment-decline/index.js`

HTTP Web操作。 与`payment-accept`模式相同，但调用`POST /V1/split-payment/orders/:orderId/cash-decline`。

成功时返回`{ ok: true, orderId, message: 'declined' }`。


### `actions/demo-dashboard/index.js`

自包含的演示操作员仪表板。 提供HTML仪表板以列出待定现金订单并触发接受/拒绝操作。 这是一个Web操作，可同时提供HTML UI和JSON API。

**安全性：**
* 可选`DEMO_UI_SECRET`检查：如果已设置，则在所有请求上需要`?secret=<value>`查询参数或`x-demo-secret`标头。 如果缺少/错误，则返回401。
* 如果未设置`DEMO_UI_SECRET`，则记录警告（仪表板不受保护）

**路由（基于HTTP方法+路径/正文）：**

操作会接收所有请求。 从以下来源确定意图：
* `params.__ow_method` (GET/POST)和`params.__ow_path`或操作参数
* 不执行任何操作→`GET`提供HTML仪表板
* 包含`action=list`参数的`GET`→返回待处理订单的JSON列表
* 具有`action=accept`和`orderId`的`POST`→调用`payment-accept`逻辑（或内联Commerce REST调用）
* 使用`action=decline`和`orderId`的`POST`→调用`payment-decline`逻辑

**正在提取挂起的订单：**
* 从Commerce REST调用`GET orders?searchCriteria[pageSize]=50`
* 筛选到`extension_attributes.split_cash_status === 'pending'` AND订单未处于终端状态(`complete`、`closed`、`canceled`、`cancelled`)的订单
* 按内存中最新优先的顺序排序
* 最多返回20（可通过`limit`参数配置）

**HTML仪表板：**
仪表板是与`Content-Type: text/html`一起返回的HTML字符串。 它应该：
* 在表中列出待定现金订单：订单编号(increment_id)、entity_id、客户名称、现金金额、商店贷方金额、日期
* 为调用操作自己的API端点的每个订单提供&#x200B;**接受**&#x200B;和&#x200B;**拒绝**&#x200B;按钮
* 显示内联成功/错误响应
* 包括刷新按钮
* 设置足够的样式以便使用（最小CSS没有问题；没有外部CDN依赖项可避免运行时出现CORS问题）
* 如果未设置`DEMO_UI_SECRET`，则显示警告横幅

在UI中显示&#x200B;**错误：**
当Commerce REST调用失败时，请包含HTTP状态和Commerce错误正文的简短描述（清理 — 剥离HTML标记，截断为500个字符）。 不要公开凭据。

**响应帮助程序：**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### 部署命令

生成所有文件后，从`split-payment-orchestrator/`目录：

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

部署后，请记下`aio app deploy`打印的操作URL。 `demo-dashboard` URL是您访问操作员仪表板的位置。


### 提示结束


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
