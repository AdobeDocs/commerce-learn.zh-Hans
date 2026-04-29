---
title: 分割支付POC：架构和设计决策
description: 了解拆分付款POC如何将签出映射到Commerce，以及如何将I/O驱动的步骤映射到App Builder，以及扩展属性、REST和五个插件边缘案例。
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 9add0b4bfa1eba33ec359adaa766b64711df25ba
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# 分割支付POC：架构和设计决策

本页介绍分割支付概念验证背后的体系结构选择。 阅读本系列构建提示之前，请先阅读该指南，以便您了解每个组件的结构以及如何调整您项目中的模式。

## 核心原则

概念验证并非关于最优雅的分割支付实施。 此视频介绍&#x200B;**如何开始将Commerce逻辑移动到App Builder而无需一次重大重写**。

在整个中应用的规则是：

> **如果某些内容必须在Commerce请求周期中同步运行，或者必须调用没有干净外部表面的Commerce内部API，则它将保留在PHP中。 其他所有内容均移至App Builder。**

## Commerce (PHP)中的内容及其原因

### &#x200B;1. 存储信用申请： `PlaceOrderPlugin`

使用`Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`将商店点数应用到购物车。 此方法仅适用于&#x200B;**活动**&#x200B;购物车。 下订单后，购物车即会停用。 App Builder会在下订单&#x200B;*后收到I/O事件*，因此无法从App Builder申请商店积分。

**课程：**&#x200B;必须在Commerce中运行任何必须更改购物车状态才能下达订单的内容。 没有解决方法。

### &#x200B;2. 同步阈值保护： `CheckoutPlugin`

在客户选择&#x200B;**[!UICONTROL Place Order]**&#x200B;之前，$100订单阈值检查必须在付款步骤阻止客户。 必须在Commerce请求周期中同步响应。 App Builder是事件驱动型和非同步的，因此当下无法立即返回错误。

App Builder *还*&#x200B;验证阈值（作为审核），但客户体验取决于先运行的Commerce检查。

### &#x200B;3. 自定义REST端点： `webapi.xml`和`SplitPaymentManagement`

以下端点必须：

* 调用`SplitInvoiceService`（使用Commerce内部发票服务的发票）
* 调用`ShipOrder::execute()`（Commerce的内部装运服务）
* 使用Commerce的订单状态机更新订单状态和状态

`/V1/split-payment/orders/:id/cash-received`和`/V1/split-payment/orders/:id/cash-decline`

该行为没有干净的公共REST层，因此Commerce会公开端点。 App Builder呼叫他们。

### &#x200B;4. 拆分报价和订单金额：观察者和插件

Commerce需要订单上的拆分数量(`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`)，这既适用于App Builder读取的REST响应，也适用于订单视图&#x200B;**[!UICONTROL Admin]**。 这些金额附有扩展属性，并且在`sales_model_service_quote_submit_before`上从报价复制到观察者的订单中。

## App Builder的居住环境及原因

### &#x200B;1. 事件驱动的订单处理： `payment-orchestrator`

在`sales_order_place_before`触发后，App Builder会接收该事件。 它重新验证阈值（作为审核），在订单上记录&#x200B;*待付现金*&#x200B;注释，并更新订单状态。 所有这些不需要新的PHP，只需REST返回Commerce。

### &#x200B;2. 现金承兑：`payment-accept`

当ERP（或仪表板中的操作员）确认已收到现金时，`payment-accept`致电`POST /V1/split-payment/orders/:id/cash-received`。 发票、发运和订单状态在Commerce中处理。 触发事件的是App Builder。

### &#x200B;3. 现金拒绝：`payment-decline`

`payment-decline`调用`POST /V1/split-payment/orders/:id/cash-decline`，Commerce将取消订单。 与现金承兑模式相同。

### &#x200B;4. 操作员仪表板： `demo-dashboard`

通过App Builder Web操作提供的自包含HTML功能板。 它会从Commerce REST获取等待现金的订单，并提供调用上述App Builder操作的&#x200B;**[!UICONTROL Accept]** / **[!UICONTROL Decline]**&#x200B;操作。 Commerce **[!UICONTROL Admin]**&#x200B;不是必需的。

## 阈值：故意强制两次

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerce拥有面向用户的防护；App Builder拥有置入后审核。** 这是有意为之。

## 商店点数：为什么它停留在PHP中

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

Orchestrator中的`store-credit.js`文件记录了该信息。 它是一个无操作存根，其中包含注释，可解释为何不使用该存根。

## 扩展属性：粘合

拆分金额根据扩展属性在系统中移动：

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## 数据模型

此模块添加的&#x200B;**`sales_order`个平面列**

| 列 | 类型 | 用途 |
| --- | --- | --- |
| `split_store_credit_amount` | 浮点数 | 已应用的商店积分 |
| `split_cash_amount` | 浮点数 | 应付现金金额 |
| `split_cash_status` | varchar | `pending`、`received`或`declined` |
| `split_sc_invoice_id` | int | 商店贷方发票的实体ID |
| `split_cash_invoice_id` | int | 现金发票的实体ID |

**扩展属性** （在`CartInterface`、`OrderInterface`和`OrderPaymentInterface`上）

* `split_store_credit_amount` （浮动）
* `split_cash_amount` （浮动）
* `split_cash_status` （字符串）

## I/O事件有效负载字段

`observer.sales_order_place_before`在`io_events.xml`中配置为在事件中包含以下内容：

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder使用`entity_id`作为订单ID，`split_store_credit_amount`和`split_cash_amount`进行阈值验证。

## 概念验证涵盖的五个边缘案例

### 1. `CapCustomerBalanceCollectPlugin`

Commerce的本机&#x200B;**[!UICONTROL Customer balance]**&#x200B;总计收集器可能会过度应用（它可以查看全部可用余额，而不是会话声明的分割量）。 此插件将数量限制为会话中声明的值。

### 2. `FixSplitPaymentGrandTotalPlugin`

应用商店积分后，报价单&#x200B;**[!UICONTROL Grand Total]**&#x200B;可以丢弃为仅限现金的金额。 签出JavaScript必须在该更改的&#x200B;*之前计算拆分验证*&#x200B;的订单总数。 该插件在总计收集后运行并更正显示，而JavaScript不只信任`grand_total`并从小计区段中重建值。

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

在收回发票合计时，可以应用两次商店贷项。 此插件更正了发票上的`customer_balance_amount`。

### 4. `SplitPaymentZeroTotalPlugin`

在应用商店点数后，购物车&#x200B;**[!UICONTROL Grand Total]**&#x200B;可以是$0（全商店点数订单）。 在这种情况下，Commerce的&#x200B;**[!UICONTROL Zero subtotal checkout]**&#x200B;检查可以阻止COD。 当会话现金量大于0时，此插件允许COD。

### &#x200B;5. `BalanceManagementInterface::apply()`之前的引号回收

`apply()`根据当前&#x200B;**[!UICONTROL Grand Total]**&#x200B;检查金额。 如果总计已经是仅现金部分，`apply()`可能会失败或达到上限。 `PlaceOrderPlugin`使用会话标志(`beginBalanceApply` / `endBalanceApply`)在应用余额时临时暂停总计修复。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
