---
title: 'Split payment POC: Commerce module AI prompt'
description: 'Learn how to use this prompt to generate Client_SplitPayment: REST, plugins, checkout JavaScript, I/O events, and enable, compile, and deploy commands.'
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# Split payment POC: Commerce module AI prompt

Use this page to copy the full prompt that generates the `Client_SplitPayment` in-process module: REST, session handling, **[!UICONTROL Checkout]**, and **[!UICONTROL Admin]** display for the split payment proof of concept. Operator workflow stays in App Builder.

## How to use this prompt

Copy everything from **PROMPT START** to **End of prompt** into Cursor (with Claude) or directly into Claude. Run it from the root of your Commerce project or a directory where the AI can create files.

## Customize before you run

* Replace `Client` with your real vendor name.
* Change `SplitPayment` if you want a different module name.
* If the site uses a custom theme, layout XML and RequireJS paths may need changes.
* If your **[!UICONTROL Cash on delivery]** method uses a different code than `cashondelivery`, update `payment-method-helper.js`.


## The prompt

**PROMPT START**

You are generating a complete, production-ready Adobe Commerce 2.4.5+ in-process module for a split payment feature. This module is the thin PHP adapter that exposes the right REST surface and attaches the right data at the right moments in the Commerce lifecycle. All operator workflow logic lives in Adobe App Builder (not in this module).

**Module identity:**
* Vendor: `Client`
* Module: `SplitPayment`
* Full name: `Client_SplitPayment`
* Namespace: `Client\SplitPayment`
* Location: `app/code/Client/SplitPayment/`
* Dependencies: `Magento_Checkout`, `Magento_CustomerBalance`, `Magento_Sales`, `Magento_Quote`, `Magento_WebApi`, `Magento_AdobeCommerceEventsClient`

生成以下文件结构列出的每个文件。 不要忽略任何文件。 在所有PHP文件中使用`declare(strict_types=1)`。


### 要生成的文件结构

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### 行为规范

#### &#x200B;1. 数据库架构(`etc/db_schema.xml`)

将这些列添加到`sales_order` （资源： `sales`）：

| 列 | 类型 | 可为空 | 注释 |
|---|---|---|---|
| `split_store_credit_amount` | decimal(12,4) | 是 | 商店贷方部分 |
| `split_cash_amount` | decimal(12,4) | 是 | 到期现金部分 |
| `split_cash_status` | varchar(32) | 是 | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | 无符号int | 是 | 商店贷方发票实体ID |
| `split_cash_invoice_id` | 无符号int | 是 | 现金发票实体ID |

也为这些列生成`db_schema_whitelist.json`。

#### &#x200B;2. 扩展属性(`etc/extension_attributes.xml`)

将`split_store_credit_amount` （浮动）、`split_cash_amount` （浮动）、`split_cash_status` （字符串）添加到：
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. REST端点(`etc/webapi.xml`)

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

所有三个映射到`Client\SplitPayment\Api\SplitPaymentManagementInterface`。

#### &#x200B;4. I/O事件(`etc/io_events.xml`)

订阅`observer.sales_order_place_before`并包含字段：
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — 会话包装器

在结账会话中存储声明的拆分金额。 必须公开：
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — 返回`['store_credit' => float, 'cash' => float]`
* `clear(): void`
* `beginBalanceApply(): void` — 在应用商店点数期间设置禁止总修复插件的标志
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` — REST控制器

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* 验证购物车属于当前会话（通过比较数字和遮罩的报价ID）
* 在`SplitPaymentSession`中存储金额
* 成功时返回`true`；失败时抛出`LocalizedException`并返回常规消息

**`markCashReceived(int $orderId): bool`**
* 按`entity_id`加载订单
* 验证`split_cash_status === 'pending'`
* 将状态设置为`received`，将状态设置为`processing`
* 添加历史记录注释： `"Cash payment of $X.XX received."`
* 呼叫`SplitInvoiceService::createCashPortionInvoice($orderId)`
* 使用现金发票增量ID添加注释
* 调用`createShipmentIfPossible($orderId)` — 如果存在可发运项目，则使用`ShipOrder::execute()`创建发运
* 返回`true`；在发生任何错误时引发通用`LocalizedException`

**`markCashDeclined(int $orderId): bool`**
* 加载顺序
* 验证`split_cash_status === 'pending'`
* 验证`$order->canCancel()`
* 将状态设置为`declined`，添加注释： `"Cash payment declined (simulated fraud check)."`
* 保存订单
* 呼叫`OrderManagement::cancel($orderId)`
* 返回`true`；失败时引发通用`LocalizedException`

#### &#x200B;7. `PlaceOrderPlugin` — `QuoteManagement`上的aroundPlaceOrder

这是最关键的插件。 运行`aroundPlaceOrder`：

1. 加载报价；如果不处于活动状态，请立即调用`$proceed()`
2. 读取`SplitPaymentSession::getAmounts()`
3. 如果引用中包含`customer_balance_amount_used > 0`，请调用`BalanceManagementInterface::remove($cartId)` （句柄`LocalizedException` — 记录并作为通用消息重发）
4. 调用`recollectQuoteForBalanceOperation()` — 加载引用，调用`collectTotals()`，保存（在`beginBalanceApply()` / `endBalanceApply()`内以禁止显示总修复插件）
5. 如果`$storeCredit > 0`，则调用`BalanceManagementInterface::apply($cartId, $storeCredit)`（句柄失败）
6. 重新加载报价；设置扩展属性： `split_store_credit_amount`、`split_cash_amount`、`split_cash_status = 'pending'`
7. 将`payment->additional_information['client_split_payment']`另存为`['store_credit' => x, 'cash' => y]`
8. 保存报价
9. 呼叫`$proceed()`，捕获订单ID
10. 呼叫`SplitPaymentSession::clear()`
11. 退货单编号

#### &#x200B;8. `CopySplitPaymentToOrder` — `sales_model_service_quote_submit_before`上的观察者

从会话中读取拆分金额→付款additional_information→报价扩展属性（按该优先级顺序）。 将`split_store_credit_amount`、`split_cash_amount`、`split_cash_status = 'pending'`写入订单对象。

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — `sales_order_place_after`上的观察者

下单后，如果订单具有商店贷方金额(`split_store_credit_amount > 0`)，请调用`SplitInvoiceService::createStoreCreditPortionInvoice($orderId)`以立即为商店贷方部分开票。

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
仅为商店贷方部分创建发票。 将发票上的`customer_balance_amount`设置为商店贷方金额。 登记并保存发票。 返回发票实体ID或在失败时返回null（日志；不重新引发）。

**`createCashPortionInvoice(int $orderId): ?int`**
为现金部分（SC发票未涵盖的剩余订单项目/金额）创建发票。 注册并保存。 失败时返回实体ID或null。

#### &#x200B;11. `CheckoutPlugin` — 在`PaymentInformationManagement`

`Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`上的插件。 继续之前：
* 从签出会话加载报价
* 获取配置的阈值： `Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`，默认`100`
* 如果`$quote->getGrandTotal() > $threshold`，则抛出`LocalizedException('Payment could not be processed. Please try again or contact support.')`

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — 总计`Customerbalance`

本机客户余额总计收集运行后，上限`customer_balance_amount_used`到`SplitPaymentSession::getAmounts()['store_credit']`。 这样可防止Commerce在客户声明商店信用部分较少时过度核销全部客户余额。

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — 在`Quote\Address\Total\Grand`

收集总计后：如果存在拆分付款会话且`isBalanceApplyInProgress()`为false，则将报价总计设置为会话现金金额。 这将使结账UI仅显示现金的到期情况。

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — 在`Sales\Model\Order\Invoice`

在收集发票合计后，如果发票的关联订单具有`split_sc_invoice_id`，请更正发票上的`customer_balance_amount`以防止双重核销商店贷项。

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — 在`Payment\Model\Checks\ZeroTotal`

在`SplitPaymentSession::getAmounts()['cash'] > 0`时允许COD付款，即使报价总计为$0（因为商店积分涵盖了整个订单）。

#### &#x200B;16. `LayoutProcessorPlugin` — 在`Checkout\Block\Checkout\LayoutProcessor`

处理布局后：
* 将`Client_SplitPayment/js/view/payment/split-payment`组件插入`components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`处`cashondelivery`付款方法组件的`additional`子项
* 从付款步骤中删除本机商店信用用户界面组件(`customerBalance`， `useStoreCredit`) — 拆分付款组件拥有商店信用显示/应用程序

#### &#x200B;17. `OrderRepositoryPlugin` — 在`OrderRepositoryInterface`

在`get()`和`getList()`之后，从平面`sales_order`列(`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`)中水合订单的扩展属性。

#### &#x200B;18. `AdminSplitPaymentTitlePlugin` — 在`Payment\Block\Info`

在`getTitle()`返回后，如果付款方式为`cashondelivery`且订单具有拆分付款，则将`" (Split: Cash $X.XX + Store Credit $Y.YY)"`附加到标题。

#### &#x200B;19. `OrderPaymentPlugin` — 在`Sales\Block\Adminhtml\Order\Payment`

在`_beforeToHtml`中或通过afterToHtml，将拆分付款详细信息（现金金额、商店贷方金额、状态）附加到Commerce管理员订单视图中的付款块HTML。

#### &#x200B;20. 店面商店信用余额控制器

`Controller/Checkout/StoreCreditBalance.php` — 路由： `GET /splitpayment/checkout/storecreditbalance`

返回JSON： `{"balance": float, "logged_in": bool}`。 如果客户未登录，则返回`{"balance": 0, "logged_in": false}`。 从`Magento\CustomerBalance\Model\Balance`读取余额。

在`etc/frontend/routes.xml`中向`frontName="splitpayment"`注册前端路由。

#### &#x200B;21. 签出KnockoutJS组件 — `split-payment.js`

`uiComponent`满足以下条件：
* 检测何时选择`cashondelivery`付款方式(`quote.paymentMethod.subscribe`)
* 通过`GET /splitpayment/checkout/storecreditbalance`加载客户的商店贷方余额
* 使用完整订单总额（从`total_segments`开始计算，不包括`grand_total`和`customerbalance`）预填现金金额字段 — 从不直接使用`grand_total`，因为可以将其设置为现金余额)
* 当客户更改现金金额输入时：计算所需的存储贷项=订单总额−现金；验证；过帐至`POST /V1/split-payment/set`
* 显示以下验证消息：现金>订单总计，商店积分不足，未登录
* 在输入有效拆分时显示成功消息： `"The remaining $X.XX will automatically be applied from your store credit."`
* 在选择其他付款方法时重置（发布`{storeCreditAmount: 0, cashAmount: 0}`以清除会话）

#### 22. `cashondelivery-method.js`

扩展`Magento_OfflinePayments/js/view/payment/offline-payments`。 使用`payment-method-helper.js`检测现金支付方式代码。 Registers `split-payment` component in its `additional` region.

#### 23. `payment-method-helper.js`

Utility returning `getCashMethodCode()` — checks `window.checkoutConfig.paymentMethods` for `cashondelivery`; falls back to `checkmo` if needed.

#### 24. `cashondelivery.html` Template

Standard COD template but includes `<!-- ko foreach: getRegion('additional') -->` region so the split payment child component can render.

#### 25. `split-payment.html` Template

KnockoutJS template for the split payment fields:
* Available store credit balance display
* Cash amount input (number, step 0.01)
* Store credit portion display (read-only)
* Auto-apply store credit message (shown when split is valid and store credit > 0)
* Validation error message

#### 26. `requirejs-config.js`

Maps:
* `Client_SplitPayment/js/view/payment/split-payment` → the component
* `Client_SplitPayment/js/view/payment/cashondelivery-method` → the COD override
* `Client_SplitPayment/js/model/payment-method-helper` → the helper

#### 27. `etc/config.xml`

Default system config values:

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### Critical Implementation Notes

**Store credit application must use `BalanceManagementInterface`, not direct model manipulation.** `BalanceManagementInterface::apply()` handles the session, validation, and cart recalculation atomically.

**`PlaceOrderPlugin`must use `aroundPlaceOrder` (not `beforePlaceOrder`).** The store credit must be applied while the cart is still active, and that must be guaranteed before `$proceed()` is called.

**The session flag pattern for `beginBalanceApply` / `endBalanceApply` is critical.** Without it, `FixSplitPaymentGrandTotalPlugin` runs during `collectTotals()` inside the balance operation and sets grand total to the cash remainder, causing `BalanceManagementInterface::apply()` to fail or cap the credit.

**Never expose internal error details to the customer.** All `catch` blocks that surface to REST responses must throw `LocalizedException('Payment could not be processed. Please try again or contact support.')`.

**`entity_id`是数字数据库ID。** 来自App Builder的REST调用始终使用`entity_id`，而不是`increment_id`。

**`SplitInvoiceService`应该捕获并记录错误，而不是传播它们。** 发票创建失败不应取消已下的订单 — 请记录该失败情况并让管理员手动处理。


### 生成文件后

在Commerce项目根目录中运行以下命令：

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 提示结束


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
