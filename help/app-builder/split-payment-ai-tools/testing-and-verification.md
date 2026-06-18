---
title: 拆分支付POC — 测试和验证指南
description: 了解如何验证拆分付款POC。 Commerce安装、REST、签出、阈值、模拟接受和拒绝、演示仪表板和App Builder日志。
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
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
7. 验证商店贷方部分是否更新为$40.00
8. 验证消息是否显示： `"The remaining $40.00 will automatically be applied from your store credit."`

**测试验证：**
* 输入大于订单总额→错误消息的现金金额
* 输入需要比可用的→错误消息更多的商店贷项的现金金额
* 输入现金= 0→错误（或商店信用覆盖整个订单）


## 步骤5 — 测试阈值保护

1. 添加合计超过$100的产品（小计+运费+税> $100）
2. 继续结帐，选择&#x200B;**现金**
3. 尝试下单
4. 验证是否显示错误消息： `"Payment could not be processed. Please try again or contact support."`
5. 验证购物车是否已保留（客户仍然可以调整购物车并重试）


## 步骤6 — 下达测试分解支付订单

1. 构建低于$100的购物车（使用商店点数登录的客户）
2. 选择结账时收款
3. 输入小于订单总计的现金金额（例如，$45订单中的$10）
4. 确认显示商店点数消息
5. 单击&#x200B;**下单**

下订单后，在Commerce管理员中验证：
* 订单处于`pending_payment`状态
* 订单具有两个历史记录注释：
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` （来自App Builder `payment-orchestrator`）
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` （来自App Builder）
* 拆分付款金额在订单付款块中可见

> **如果未出现App Builder注释：**&#x200B;请检查App Builder操作日志，其中包含`aio app logs`。 该事件可能没有触发，或者该操作可能有错误。


## 步骤7 — 通过模拟脚本测试接受

模拟脚本是在没有完整操作员UI的情况下测试接受/拒绝流的最快方式。

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

接受后，在Commerce管理员订单视图中验证：
* 订单状态为`processing`
* 历史记录注释： `"Cash payment of $X.XX received."`
* 已创建现金发票（显示在“发票”标签中）
* 已创建发运（如果适用，显示在“发运”标签中）
* 历史记录注释： `"Split payment: cash portion invoiced #XXXXXXXX."`
* 历史记录注释： `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## 步骤8 — 通过模拟脚本进行测试拒绝

再次下达测试订单（与步骤6的设置相同），然后：

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

拒绝后，在Commerce管理员中进行验证：
* 订单状态为`canceled`
* 历史记录注释： `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## 步骤9 — 测试演示仪表板

部署`split-payment-orchestrator`后，`aio app deploy`将打印操作URL。

在浏览器中打开`demo-dashboard` URL：

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

如果设置了`DEMO_UI_SECRET`：

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

待处理订单：
1. 仪表板应显示挂起列表中的顺序
2. 单击&#x200B;**接受**→订单应移至Commerce中的`processing`
3. 下另一张订单；单击&#x200B;**拒绝**，Commerce中的订单应为`canceled`


## 步骤10 — 测试App Builder操作日志

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

对于`payment-orchestrator`，查找：

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

对于`payment-accept`或`payment-decline`：

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## 常见问题和修复

### Commerce OAuth中的“签名无效”

**原因：** App Builder运行时和Commerce之间的时钟偏差，或OAuth签名错误。

**修复：**
* 确认`COMMERCE_BASE_URL`没有尾随斜杠
* 确认四个OAuth凭据用于&#x200B;_激活的_&#x200B;集成
* 首先使用模拟脚本进行测试 — 如果脚本可正常使用，但App Builder无法正常使用，则很可能未加载环境变量（请检查`aio app deploy`输出以了解缺少的环境变量）

### 拆分付款字段未显示在结账中

**原因：** LayoutProcessorPlugin注入路径与您的签出布局不匹配。

**修复：**
* 检查浏览器控制台中是否有加载`Client_SplitPayment/js/view/payment/split-payment`的RequireJS错误
* 检查`bin/magento setup:static-content:deploy`已成功完成
* 刷新缓存： `bin/magento cache:flush`
* 如果您的主题具有自定义签出，则可能需要调整用于插入组件的`LayoutProcessorPlugin`路径

### 商店积分未申请/“无法处理付款”下单

**原因：**&#x200B;通常边缘案例插件之一无法正常工作。

**检查：**
1. `SplitPaymentSession`是否返回正确的金额？ 在`PlaceOrderPlugin`中添加临时调试日志记录
2. 是否在调用`BalanceManagementInterface::apply()`之前运行`FixSplitPaymentGrandTotalPlugin`并影响报价总额？ `beginBalanceApply()`标记应禁止使用。
3. 是否在Commerce中启用了客户平衡模块？

### App Builder操作会接收事件，但缺少orderId

**原因：** `io_events.xml`字段列表不包含`entity_id`，或事件有效负载形状已更改。

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
