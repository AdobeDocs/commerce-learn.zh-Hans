---
title: 拆分支付POC分步实施指南
description: 了解如何在设置完成后按顺序实施分割付款POC，包括模块、环境、Orchestrator、验证和可选UI。
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# 拆分支付POC分步实施指南

此页面是您的实施核对清单。 它假定您已观看[概述](./overview.md)和[完整演示](./full-demo.md)，并且Commerce站点和App Builder项目均已准备就绪。 各个教程页面包含下面每个主题的完整详细信息 — 使用此页面可了解要做什么以及按照什么顺序。

## 开始之前

运行任何提示符或命令之前，请确认以下所有操作。 [先决条件和设置](./prerequisites-and-setup.md)页面详细介绍了每个项目。

**Commerce端**

* [ ] Adobe Commerce 2.4.5或更高版本
* [ ] I/O事件已启用并部署（仅限Commerce Cloud — 需要`.magento.env.yaml`中的`ENABLE_EVENTING: true`）
* [ ] **货到付款**&#x200B;已在Admin中启用，其标题完全设置为`Cash`
* [ 已在Admin中启用]商店积分
* [ ]测试客户至少有$50美元的商店点数
* [ 已创建、激活]个Commerce集成，并安全地保存了所有四个OAuth值

**App Builder端**

* [ Adobe Developer Console中存在]个App Builder项目
* [ ]个Adobe I/O Events服务已添加到工作区
* [ ]个Commerce已连接为事件提供程序
* [ ] `aio login`完成；使用`aio app use`选择了正确的工作区
* [ 已安装] Node.js 18或更高版本；已安装`aio` CLI (`npm install -g @adobe/aio-cli`)

如果以上任何内容缺失，请在此处停止，然后先完成它。

## 第1阶段 — 构建Commerce模块

**这样做的原因：**&#x200B;生成用于处理签出UI、存储信用申请、REST端点和I/O事件订阅的`Client_SplitPayment` PHP模块。 这是精简的Commerce内适配器 — 所有操作员工作流程都保留在App Builder中。

### 步骤1.1 — 自定义项目的提示

打开[Commerce module AI提示](./commerce-module-prompt.md)页面，并在复制提示之前注意以下事项：

* 在提示中将`Client`替换为您的真实供应商名称
* 如果需要其他模块名称，请替换`SplitPayment`
* 如果您的主题不是Luma，`LayoutProcessorPlugin`注入路径和RequireJS映射可能需要调整
* 如果您的“货到付款”方法使用`cashondelivery`以外的代码，请更新`payment-method-helper.js`

### 步骤1.2 — 运行提示

将完整的提示从&#x200B;**PROMPT START**&#x200B;复制到&#x200B;**提示结束**，并将其粘贴到Cursor （使用Claude）中或直接粘贴到Claude中。 从您的Commerce项目的根目录中运行它，以便AI可以在`app/code/`下创建文件。

### 步骤1.3 — 启用并安装模块

从Commerce项目根目录运行以下命令：

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 步骤1.4 — 验证模块是否已正确安装

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

您应该看到五列：`split_store_credit_amount`、`split_cash_amount`、`split_cash_status`、`split_sc_invoice_id`和`split_cash_invoice_id`。

然后在浏览器中确认，当具有商店点数的登录客户选择&#x200B;**现金**&#x200B;作为付款方式时，拆分付款字段会在结账时显示。

## 阶段2 — 配置环境变量

**此操作如下：**&#x200B;准备App Builder Orchestrator、模拟脚本和（可选）Experience Cloud UI扩展使用的凭据文件。 每个`.env`文件中都会使用来自您的Commerce集成的相同四个OAuth值。

有关每个组件的变量的完整列表，请参阅[环境变量引用](./env-reference.md)。

### 步骤2.1 — 创建Orchestrator `.env`

在您的`split-payment-orchestrator/`目录中：

```bash
cp .env.example .env
```

填写每个值：

| 变量 | 从何处获得 |
|---|---|
| `COMMERCE_BASE_URL` | 您的商店URL — 无尾随斜杠 |
| `COMMERCE_CONSUMER_KEY` | Commerce管理员>系统>集成 |
| `COMMERCE_CONSUMER_SECRET` | 相同 — 仅在激活时显示 |
| `COMMERCE_ACCESS_TOKEN` | 相同 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 相同 |
| `PAYMENT_THRESHOLD` | 必须匹配Commerce中的`split_payment/general/threshold`（默认值： `100`） |
| `DEMO_UI_SECRET` | 可选 — 如果设置，则为仪表板URL中的`?secret=`必需 |

### 步骤2.2 — 创建模拟脚本`.env`

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

填写`COMMERCE_BASE_URL`和四个OAuth值（与上述凭据相同）。

## 阶段3 — 构建App Builder Orchestrator

**此操作是什么：**&#x200B;生成`split-payment-orchestrator` App Builder项目：`payment-orchestrator` I/O事件使用者、`payment-accept`和`payment-decline` Web操作以及`demo-dashboard`操作员UI。

### 步骤3.1 — 运行Orchestrator提示符

打开[App Builder orchestrator AI提示符](./orchestrator-prompt.md)页。 复制完整提示并从`split-payment-orchestrator/`目录运行它。

### 步骤3.2 — 安装依赖项并部署

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

部署后，`aio app deploy`打印操作URL。 记下`demo-dashboard` URL — 您在阶段4中需要它。

### 步骤3.3 — 确认事件注册

在Adobe Developer Console中，打开您的App Builder项目工作区。 在&#x200B;**事件**&#x200B;下，确认`Split payment — sales order place before`注册存在且已绑定到`payment-orchestrator`操作。

## 阶段4 — 测试整个流程

按顺序完成验证步骤。 [测试和验证指南](./testing-and-verification.md)具有以下每个步骤的完整curl命令、模拟脚本使用和故障排除参考。

### 步骤4.1 — 验证REST端点是否可路由

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### 步骤4.2 — 测试签出UI

1. 以测试客户（拥有商店点数的客户）的身份登录到店面
2. 将产品添加到购物车，在配送和征税后购物车总价值在100美元以下
3. 在付款步骤中，选择&#x200B;**现金**
4. 确认显示的分摊付款字段：显示余额、预付现金、存储贷记为$0
5. 减少现金金额，并确认商店信用部分正确更新

### 步骤4.3 — 测试阈值保护

添加合计超过$100的产品，继续结帐，选择“现金”，然后尝试下订单。 必须显示错误`"Payment could not be processed. Please try again or contact support."`。

### 步骤4.4 — 发出测试拆分支付订单

创建低于$100的购物车，输入现金/商店信用拆分，然后下订单。 在Commerce Admin中，确认：

* 订单状态为`pending_payment`
* 订单上会显示来自App Builder的两条评论：
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

如果未出现App Builder注释，请检查包含`aio app logs`的日志后再继续。

### 步骤4.5 — 通过模拟脚本测试接受

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

在Commerce Admin中，确认：

* 订单状态已更改为`processing`
* 历史记录注释： `"Cash payment of $X.XX received."`
* 在“发票”标签上显示的现金发票
* 在“发运”标签中显示的发运（如果适用）

### 步骤4.6 — 通过模拟脚本拒绝测试

再下达一次测试订单，然后：

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

确认订单状态为`canceled`，`split_cash_status`为`declined`。

### 步骤4.7 — 测试演示仪表板

打开步骤3.2中的功能板URL。 系统中有挂起的订单：

1. 确认订单显示在待处理列表中
2. 单击&#x200B;**接受**&#x200B;并确认在Commerce中将订单移至`processing`
3. 下新订单，返回到仪表板，然后单击&#x200B;**拒绝** — 确认取消

## 阶段5（可选） — 添加Experience Cloud UI扩展

此阶段将操作员仪表板嵌入到Commerce Admin shell中，而不是使用独立的`demo-dashboard`。 如果独立仪表板满足您的需求，请跳过此阶段。

### 步骤5.1 — 查看先决条件

除了OAuth集成值之外，Experience Cloud UI扩展还需要IMS凭据。 请阅读[Experience Cloud UI扩展AI提示](./experience-cloud-ui-prompt.md)页，然后再运行该提示，并注意`OAUTH_CLIENT_ID`和相关IMS变量。

### 步骤5.2 — 运行UI扩展提示

将完整的提示从&#x200B;**PROMPT START**&#x200B;复制到&#x200B;**End of prompt**，然后从`commerce-checkout-starter-kit/`目录运行它。

### 步骤5.3 — 部署

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## 快速疑难解答参考

| 症状 | 首先要检查的事项 |
|---|---|
| 结账时未显示拆分付款字段 | 浏览器控制台中出现RequireJS错误；同时运行`bin/magento cache:flush` |
| 下单`"Payment could not be processed"` | `PlaceOrderPlugin`或`BalanceManagementInterface` — 在`PlaceOrderPlugin`中添加临时调试日志记录 |
| 订单上没有App Builder备注 | 运行`aio app logs`以查看是否触发了事件以及返回了什么操作 |
| App Builder中Commerce REST的`403` | Fastly IP列入允许列表 — 首先使用模拟脚本进行测试；如果可行，则问题在于出口IP阻塞 |
| 来自Commerce的`"The signature is invalid"` | 确认`COMMERCE_BASE_URL`没有尾随斜杠；确认集成已激活 |
| 订单未显示在演示仪表板中 | 检查`extension_attributes.split_cash_status`是否出现在直接`GET /rest/V1/orders`响应中 |

有关完整的疑难解答详细信息，请参阅[测试和验证指南](./testing-and-verification.md#common-issues-and-fixes)。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
