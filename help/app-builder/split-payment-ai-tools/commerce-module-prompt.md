---
title: 拆分付款POC — Commerce模块AI提示
description: 了解如何使用此提示生成Client_SplitPayment。 REST、插件、签出JavaScript、I/O事件以及启用、编译和部署命令。
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# 拆分付款POC：Commerce模块AI提示

使用此页面复制生成`Client_SplitPayment`进程内模块的完整提示： REST、会话处理、**[!UICONTROL Checkout]**&#x200B;和为拆分付款概念验证显示的&#x200B;**[!UICONTROL Admin]**。 操作员工作流停留在App Builder中。

## 如何使用此提示

将从&#x200B;**PROMPT START**&#x200B;到&#x200B;**提示**&#x200B;结尾的所有内容复制到光标（使用Claude）或直接复制到Claude。 从Commerce项目的根目录或AI可以创建文件的目录运行它。

## 在运行前进行自定义

* 将`Client`替换为您的真实供应商名称。
* 如果需要其他模块名称，请更改`SplitPayment`。
* 如果站点使用自定义主题，则需要更改布局XML和RequireJS路径。
* 如果您的&#x200B;**[!UICONTROL Cash on delivery]**&#x200B;方法使用的代码与`cashondelivery`不同，请更新`payment-method-helper.js`。


## 提示

**提示开始**

您正在为分割支付功能生成一个完整的生产就绪型Adobe Commerce 2.4.5及更高版本处理中模块。 此模块是细小的PHP适配器，可在Commerce生命周期的正确时刻公开正确的REST曲面并附加正确的数据。 所有运算符工作流逻辑都存在于Adobe App Builder中（而不是本模块中）。

**模块标识：**
* 供应商： `Client`
* 模块： `SplitPayment`
* 全名： `Client_SplitPayment`
* 命名空间： `Client\SplitPayment`
* 位置： `app/code/Client/SplitPayment/`
* 依赖项： `Magento_Checkout`，`Magento_CustomerBalance`，`Magento_Sales`，`Magento_Quote`，`Magento_WebApi`，`Magento_AdobeCommerceEventsClient`

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

扩展`Magento_OfflinePayments/js/view/payment/offline-payments`。 使用`payment-method-helper.js`检测现金支付方式代码。 在其`additional`区域中注册`split-payment`组件。

#### 23. `payment-method-helper.js`

实用程序返回`getCashMethodCode()` — 检查`cashondelivery`的`window.checkoutConfig.paymentMethods`；如果需要，回退到`checkmo`。

#### &#x200B;24. `cashondelivery.html` 模板

标准COD模板，但包括`<!-- ko foreach: getRegion('additional') -->`区域，以便呈现拆分付款子组件。

#### &#x200B;25. `split-payment.html` 模板

拆分付款字段的挖空JS模板：
* 显示可用的商店贷方余额
* 现金金额输入（数字，步骤0.01）
* 存储信用部分显示（只读）
* 自动应用商店积分消息（在拆分有效并且商店积分> 0时显示）
* 验证错误消息

#### 26. `requirejs-config.js`

映射：
* 组件→的`Client_SplitPayment/js/view/payment/split-payment`
* `Client_SplitPayment/js/view/payment/cashondelivery-method`→COD覆盖
* `Client_SplitPayment/js/model/payment-method-helper`→帮助程序

#### 27. `etc/config.xml`

默认系统配置值：

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### 重要实施说明

**存储信用申请必须使用`BalanceManagementInterface`，而不是直接模型操作。** `BalanceManagementInterface::apply()`自动处理会话、验证和购物车重新计算。

**`PlaceOrderPlugin`必须使用`aroundPlaceOrder` （不是`beforePlaceOrder`）。** 必须在购物车仍处于活动状态时应用商店点数，并且必须在调用`$proceed()`之前进行保证。

**`beginBalanceApply` / `endBalanceApply`的会话标志模式是关键的。** 如果没有它，`FixSplitPaymentGrandTotalPlugin`将在`collectTotals()`期间在余额操作中运行，并将总计设置为现金余额，从而导致`BalanceManagementInterface::apply()`失败或限制贷项。

**从不向客户公开内部错误详细信息。** 所有显示到REST响应的`catch`块都必须引发`LocalizedException('Payment could not be processed. Please try again or contact support.')`。

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
