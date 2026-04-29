---
title: 拆分支付POC：测试和验证指南
description: 了解如何验证拆分付款POC：Commerce安装、REST、结账、阈值、模拟接受和拒绝、演示仪表板和App Builder日志。
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# 拆分支付POC：测试和验证指南

本指南将指导您按照应测试的顺序验证每个组件是否正常工作。 从下开始（Commerce模块），然后再从上开始(App Builder)。


## 步骤1 — 验证Commerce模块的安装

运行`bin/magento setup:upgrade`和`bin/magento setup:di:compile`后：

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

预期输出：在`sales_order`中显示以`split_`开头的四列。


## 步骤2 — 验证Commerce管理配置

在Commerce Admin中：
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — 确认已启用&#x200B;**[!UICONTROL Cash On Delivery]**，标题为`Cash`
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** — 确认已启用
3. 确认您的测试客户拥有商店积分： **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[客户]* > **[!UICONTROL Store Credit]**


## 步骤3 — 验证REST端点是否可访问

使用curl确认端点响应（它们会拒绝未授权请求，但401会确认它们已正确路由）：

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## 步骤4 — 测试签出UI

1. 以测试客户（拥有商店积分）的身份登录到店面
2. 将产品添加到购物车（发货后加税后合计不到$100）
3. 继续以结账
4. 在付款步骤中，选择&#x200B;**现金**（或货到付款）
5. 验证是否显示拆分付款字段：
   * 显示的商店贷方余额
   * 使用订单总额预填充的现金金额字段
   * 显示$0.00的“此订单的商店积分”字段（因为现金=全部订单总计）
6. 减少现金金额（例如，为$50的订单输入$10）
7. Verify the store credit portion updates to $40.00
8. Verify the message appears: `"The remaining $40.00 will automatically be applied from your store credit."`

**Test validation:**
* Enter a cash amount greater than the order total → error message
* Enter a cash amount that requires more store credit than available → error message
* Enter cash = 0 → error (or store credit covers entire order)


## Step 5 — Test Threshold Guard

1. Add products totaling more than $100 (subtotal + shipping + tax > $100)
2. Proceed to checkout, select **Cash**
3. Attempt to place the order
4. Verify the error message appears: `"Payment could not be processed. Please try again or contact support."`
5. Verify the cart is preserved (customer can still adjust cart and try again)


## Step 6 — Place a Test Split Payment Order

1. Build a cart under $100 (logged in customer with store credit)
2. Select Cash at checkout
3. Enter a cash amount less than the order total (e.g., $10 of a $45 order)
4. Confirm the store credit message appears
5. Click **Place Order**

After order placement, verify in Commerce Admin:
* Order is in `pending_payment` status
* Order has two history comments:
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` (from App Builder `payment-orchestrator`)
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` (from App Builder)
* The split payment amounts are visible in the order payment block

> **If no App Builder comments appear:** Check App Builder action logs with `aio app logs`. The event may not have fired or the action may have an error.


## Step 7 — Test Accept via Simulation Script

The simulation script is the fastest way to test the accept/decline flow without the full operator UI.

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

After accept, verify in Commerce Admin order view:
* Order status is `processing`
* History comment: `"Cash payment of $X.XX received."`
* Cash invoice created (visible in Invoices tab)
* Shipment created (visible in Shipments tab, if applicable)
* History comment: `"Split payment: cash portion invoiced #XXXXXXXX."`
* History comment: `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## Step 8 — Test Decline via Simulation Script

Place another test order (same setup as Step 6), then:

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

After decline, verify in Commerce Admin:
* Order status is `canceled`
* History comment: `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## Step 9 — Test the Demo Dashboard

After deploying the `split-payment-orchestrator`, `aio app deploy` prints the action URLs.

Open the `demo-dashboard` URL in a browser:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

If `DEMO_UI_SECRET` is set:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

With a pending order:
1. The dashboard should show the order in the pending list
2. Click **Accept** → order should move to `processing` in Commerce
3. Place another order; click **Decline** → order should be `canceled` in Commerce


## Step 10 — Test App Builder Action Logs

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

For the `payment-orchestrator`, look for:

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

For `payment-accept` or `payment-decline`:

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## Common Issues and Fixes

### &quot;The signature is invalid&quot; from Commerce OAuth

**Cause:** Clock skew between App Builder runtime and Commerce, or an OAuth signing bug.

**Fix:**
* Confirm `COMMERCE_BASE_URL` has no trailing slash
* Confirm the four OAuth credentials are for an _activated_ integration
* Test with the simulation script first — if the script works but App Builder doesn&#39;t, it&#39;s likely an env variable not loaded (check `aio app deploy` output for missing env vars)

### Split payment fields not appearing in checkout

**Cause:** LayoutProcessorPlugin injection paths don&#39;t match your checkout layout.

**Fix:**
* Check the browser console for RequireJS errors loading `Client_SplitPayment/js/view/payment/split-payment`
* Check `bin/magento setup:static-content:deploy` completed successfully
* Flush cache: `bin/magento cache:flush`
* If your theme has a custom checkout, the `LayoutProcessorPlugin` path to inject the component may need adjustment

### Store credit not applying / &quot;Payment could not be processed&quot; at place order

**Cause:** Usually one of the edge case plugins not working correctly.

**Check:**
1. Is `SplitPaymentSession` returning the correct amounts? Add temporary debug logging in `PlaceOrderPlugin`
2. Is `FixSplitPaymentGrandTotalPlugin` running and affecting the quote total before `BalanceManagementInterface::apply()` is called? The `beginBalanceApply()` flag should suppress it.
3. Is the customer balance module enabled in Commerce?

### App Builder action receives event but orderId is missing

**Cause:** The `io_events.xml` field list doesn&#39;t include `entity_id`, or the event payload shape changed.

**修复：**
* 确认`io_events.xml`在字段列表中包含`entity_id`
* 在操作中，暂时记录`JSON.stringify(params)`以查看完整的有效负载形状
* 检查`extractValue()`函数是否找到了正确的嵌套级别

### 订单未显示在演示仪表板中

**原因：** Commerce REST `orders`搜索条件未返回订单，或`split_cash_status`字段不在REST响应中。

**修复：**
* 确认`OrderRepositoryPlugin`是否正确加载扩展属性
* 直接测试： `GET /rest/V1/orders?searchCriteria[pageSize]=5`并检查响应中是否显示`extension_attributes.split_cash_status`
* 检查`extension_attributes.xml`是否在`OrderInterface`上正确声明`split_cash_status`属性


## 验证核对清单

* [ ] `split_*`列显示在`sales_order`表中
* [ 在没有身份验证的情况下调用] REST端点时返回401（不是404）
* [ 选择“现金”时，]拆分付款UI在结账时呈现
* [ ]验证消息有效（付款过多、信用不足）
* [ ]阈值保护阻止订单> $100
* [ ]已下订单具有`pending_payment`状态和App Builder备注
* [ ] `simulate-split-payment.mjs list`显示具有拆分金额的测试订单
* [ ] `simulate-split-payment.mjs accept <id>`将订单移动到`processing`，其中包含发票和装运
* [ ] `simulate-split-payment.mjs decline <id>`取消订单
* [ ]演示仪表板列出待处理订单并接受/拒绝来自UI的工作


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
