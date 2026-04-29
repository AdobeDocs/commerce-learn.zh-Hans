---
title: 'Split payment POC: App Builder orchestrator AI prompt'
description: 'Learn how to use this prompt to build the split-payment-orchestrator app: I/O events, payment-orchestrator, web actions, demo dashboard, and aio app deploy.'
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# Split payment POC: App Builder orchestrator AI prompt

Use this page to copy the full prompt that generates the **split-payment-orchestrator** project: the **payment-orchestrator** I/O Event consumer, **payment-accept** and **payment-decline** web actions, the **demo-dashboard**, and the Commerce REST client.

## How to use this prompt

Copy everything from **PROMPT START** to **End of prompt** into Cursor (with Claude) or directly into Claude. Run the prompt from the `split-payment-orchestrator/` directory (the App Builder project root).

## Before you run

* Finish [Split payment POC: prerequisites and environment setup](split-payment-poc-prerequisites-and-setup.md).
* Have your [Split payment POC: environment variables reference](split-payment-poc-env-reference.md) and `.env` file ready in the project.


## The prompt

**PROMPT START**


You are generating a complete Adobe App Builder application for orchestrating split payments in Adobe Commerce. This application receives I/O Events from Commerce, processes split payment decisions, and calls back into Commerce via REST.

**Project:** `split-payment-orchestrator`
**Runtime:** Node.js 18
**Key dependencies:** `@adobe/aio-sdk ^6.0.0`, `got ^11.8.6`, `oauth-1.0a ^2.2.6`

Generate all files listed below. The application must work with Adobe I/O Runtime (`aio app deploy`).


### File Structure to Generate

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

Define four actions in the `split_payment_orchestrator` package:

**`payment-orchestrator`**
* `web: "no"` (I/O Event trigger only — not directly callable via HTTP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Inputs from env: `LOG_LEVEL`, `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET`, `PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` (HTTP web action — callable by dashboard or ERP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Same Commerce credential inputs (no `PAYMENT_THRESHOLD`)

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
7. Return `{ statusCode: 200, body: { ok: true, message: 'processed' } }`

**Important:** Always return `statusCode: 200` — I/O Runtime will retry non-200 responses, which would cause duplicate order processing. Errors are reported in the body.

**`PUBLIC_ERROR`constant:** `"Payment could not be processed. Please try again or contact support."` — used for all external-facing error messages.


### `actions/payment-accept/index.js`

HTTP web action. Calls `POST /V1/split-payment/orders/:orderId/cash-received`.

**Order ID resolution:** Check `params.orderId`, then `params.payload.orderId`, then `params.__ow_body` (parsed JSON). Return 400 if missing.

**流量：**
1. Resolve `orderId`; return 400 if missing
2. Init commerce client; return 500 if fails
3. Call `POST split-payment/orders/${orderId}/cash-received` with empty JSON body
4. If 2xx: return `{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }`
5. If error: log and return `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`


### `actions/payment-decline/index.js`

HTTP web action. Same pattern as `payment-accept` but calls `POST /V1/split-payment/orders/:orderId/cash-decline`.

Return `{ ok: true, orderId, message: 'declined' }` on success.


### `actions/demo-dashboard/index.js`

Self-contained demo operator dashboard. Serves an HTML dashboard for listing pending cash orders and triggering accept/decline actions. This is a single web action that serves both the HTML UI and a JSON API.

**Security:**
* Optional `DEMO_UI_SECRET` check: if set, require `?secret=<value>` query param or `x-demo-secret` header on all requests. Return 401 if missing/wrong.
* Log a warning if `DEMO_UI_SECRET` is not set (dashboard is unprotected)

**Routing (based on HTTP method + path/body):**

The action receives all requests. Determine intent from:
* `params.__ow_method` (GET/POST) and `params.__ow_path` or action params
* `GET` with no action → serve the HTML dashboard
* `GET` with `action=list` param → return JSON list of pending orders
* `POST` with `action=accept` and `orderId` → call `payment-accept` logic (or inline the Commerce REST call)
* `POST` with `action=decline` and `orderId` → call `payment-decline` logic

**Fetching pending orders:**
* Call `GET orders?searchCriteria[pageSize]=50` from Commerce REST
* Filter to orders where `extension_attributes.split_cash_status === 'pending'` AND order is not in a terminal state (`complete`, `closed`, `canceled`, `cancelled`)
* Sort newest-first in memory
* Return up to 20 (configurable via `limit` param)

**HTML Dashboard:**
The dashboard is an HTML string returned with `Content-Type: text/html`. It should:
* List pending cash orders in a table: Order # (increment_id), entity_id, customer name, cash amount, store credit amount, date
* Provide **Accept** and **Decline** buttons for each order that call the action&#39;s own API endpoint
* Show success/error responses inline
* Include a refresh button
* Be styled enough to be usable (minimal CSS is fine; no external CDN dependencies to avoid CORS issues in Runtime)
* Show a warning banner if `DEMO_UI_SECRET` is not set

**Error surfacing in the UI:**
When a Commerce REST call fails, include the HTTP status and a short description of the Commerce error body (sanitize — strip HTML tags, truncate to 500 chars). Do not expose credentials.

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
