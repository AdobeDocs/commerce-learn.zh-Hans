---
title: 拆分付款POC：Experience Cloud UI扩展人工智能提示
description: 了解如何使用此可选提示在Commerce管理员中嵌入拆分付款：管理员UI SDK、IMS、OAuth、接受和拒绝以及模拟脚本。
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 629bbb6fe26f128e346d85c857111c2f8dbb6d76
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# 拆分付款POC：Experience Cloud UI扩展人工智能提示

这是一个可选步骤，它使用`commerce-checkout-starter-kit`和`commerce-backend-ui-1`模式在&#x200B;**[!UICONTROL Adobe Commerce]**&#x200B;管理外壳(Experience Cloud)中嵌入拆分付款单面板。 App Builder Orchestrator中的独立[演示仪表板](./orchestrator-prompt.md)包含相同的接受和拒绝流程，但没有管理Shell集成。

## 如何使用此提示

将从&#x200B;**PROMPT START**&#x200B;到&#x200B;**End of prompt**&#x200B;的所有内容复制到Cursor或Claude中。 从`commerce-checkout-starter-kit/`目录运行提示。

## 运行之前

* 除了OAuth值之外，此路径还需要&#x200B;**IMS**&#x200B;凭据（请参阅[拆分付款POC：环境变量引用`commerce-checkout-starter-kit`变量](./env-reference.md)）。
* 如果您希望将相同的`payment-accept`和`payment-decline`行为进行比较，请首先完成[拆分付款POC：App Builder orchestrator AI提示](./orchestrator-prompt.md)；UI扩展会使用`COMMERCE_INTEGRATION_*`环境名称重用该逻辑。


## 提示

**提示开始**


您正在为拆分付款PoC生成`commerce-backend-ui-1`管理员UI SDK扩展。 此扩展使用Adobe Commerce Checkout Starter Kit中的`@adobe/aio-app-dev-toolkit`和`@adobe/commerce-backend-ui-1`模式将拆分付款单仪表板嵌入到Commerce Admin Shell中。

**基本项目：** `commerce-checkout-starter-kit/`
**扩展目录：** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### 此扩展提供的功能

1. **管理员订单网格面板** — Commerce管理员外壳程序中的自定义菜单项，其中列出了带有`split_cash_status = 'pending'`的分割付款订单
2. **订单详细信息视图** — 在订单旁边显示拆分付款细分（现金金额、商店贷方金额、状态）
3. **接受/拒绝操作** — 通过OAuth 1.0a调用`payment-accept`和`payment-decline` App Builder操作的按钮


### 要生成的文件结构

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### 后端操作

**`actions/commerce/index.js`** — 经IMS身份验证的Commerce REST
* 使用管理员UI SDK上下文提供的IMS令牌调用Commerce REST
* 使用`split_cash_status`筛选器获取订单列表
* 以JSON格式返回订单列表

**`actions/payment-accept/commerce-client.js`** — OAuth 1.0a客户端
* 与`split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`实现相同
* 使用`COMMERCE_INTEGRATION_*`前缀的环境变量（用于区分IMS凭据）

**`actions/payment-accept/index.js`** — 接受操作
* 与`split-payment-orchestrator/actions/payment-accept/index.js`相同的逻辑
* 通过OAuth 1.0a调用`POST /V1/split-payment/orders/:orderId/cash-received`

**`actions/payment-decline/index.js`** — 拒绝操作
* 呼叫`POST /V1/split-payment/orders/:orderId/cash-decline`

**`actions/registration/index.js`** — 管理员UI SDK注册
* 在Commerce Admin Shell中注册扩展
* 在拆分付款仪表板的“订单”下添加菜单项


### React前端组件

**`SplitPaymentDashboard.jsx`**
* 在频谱样式表中列出挂起的拆分付款单
* 列：订单编号(increment_id)、日期、客户、现金到期日、商店贷项、状态
* 每行的“接受”和“拒绝”按钮
* 通过`web-src/src/utils.js`获取帮助程序调用后端操作
* 显示加载/错误状态；操作完成时刷新

**`SplitPaymentOrderDetail.jsx`**
* 显示单个订单的分割付款明细
* 显示：现金金额、商店贷方金额、当前拆分_cash_status

**`useSplitPaymentOrders.js`** — React挂接
* 从`actions/commerce/index.js`获取拆分付款单
* 返回`{ orders, loading, error, refresh }`


### 模拟脚本

**`scripts/simulate-split-payment.mjs`**

用于直接测试Commerce REST调用的Node.js ESM脚本（无需通过App Builder）。 使用与App Builder操作相同的OAuth 1.0a签名。

命令：
* `node simulate-split-payment.mjs help` — 显示使用情况
* `node simulate-split-payment.mjs list` — 列出具有拆分付款数据的最近订单
* `node simulate-split-payment.mjs show <orderId>` — 显示特定订单(entity_id)的分割付款字段
* `node simulate-split-payment.mjs accept <orderId>` — 调用`cash-received`终结点
* `node simulate-split-payment.mjs decline <orderId>` — 调用`cash-decline`终结点

从`commerce-backend-ui-1/.env.simulation`读取凭据（从`.env.simulation.example`复制）。

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

使用以下内容配置扩展：
* `commerce-backend-ui-1`扩展类型
* 四个后端操作(`commerce`、`payment-accept`、`payment-decline`、`registration`)
* `require-adobe-auth: true`用于除`registration`之外的所有操作
* 来自环境的`COMMERCE_INTEGRATION_*`凭据的输入绑定


### 生成文件后

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### 提示结束


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
