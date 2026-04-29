---
title: 拆分付款POC：先决条件和环境设置
description: 了解如何在拆分支付构建提示之前设置Commerce、COD和商店积分管理员、OAuth集成、I/O事件、App Builder和aio CLI。
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# 拆分付款POC：先决条件和环境设置

在运行任何生成提示之前，请完成本教程中的每个步骤。 缺少一个步骤是教程中期流量中断的最常见原因。

## &#x200B;1. Adobe Commerce要求

* Adobe Commerce **2.4.5或更高版本**（内部部署或Commerce Cloud）
* 对Commerce项目的Git访问权限（您在`app/code/`下添加模块）
* **[!UICONTROL Commerce Admin]**&#x200B;的访问权限

### 仅限Commerce Cloud：启用I/O事件

添加模块之前，请将以下内容添加到`.magento.env.yaml`并进行部署：

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **警告：**&#x200B;必须成功完成此部署才能解决I/O事件模块依赖关系。


## &#x200B;2. Commerce管理配置

先执行这些步骤，然后再执行其他操作。 签出JavaScript取决于完全匹配的字符串。

### 2a. 使用确切标题启用“货到付款”

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]**： **是**
* **[!UICONTROL Title]**： **`Cash`** （此字符串正是JavaScript匹配的签出字符串）

> **注意：**&#x200B;如果您的商店使用其他货到付款(COD)实现或标题，请在Commerce模块中调整`payment-method-helper.js`。

### 2b. 启用商店点数

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]**： **是**

### 2c. 向测试客户添加商店积分

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[您的测试客户]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

在商店积分中添加至少&#x200B;**$50**。 您将使用总金额在100美元以下的订单进行测试。

### 2d 创建Commerce集成

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**： `Split Payment App Builder`（或您喜欢的任何名称）
* 在&#x200B;**[!UICONTROL API]**&#x200B;选项卡上，至少授予：
   * `Magento_Sales::actions` （`cash-received`终结点所必需的）
   * `Magento_Sales::cancel` （`cash-decline`终结点所必需的）
   * 报价或购物车管理（完整或相关的子集）
   * **[!UICONTROL Customer balance]** （完整或相关的子集）

选择&#x200B;**[!UICONTROL Save]**，然后选择&#x200B;**[!UICONTROL Activate]**。

**复制所有四个值；它们只显示一次：**

| 值 | 环境变量 |
| --- | --- |
| 使用者密钥 | `COMMERCE_CONSUMER_KEY` |
| 使用者密码 | `COMMERCE_CONSUMER_SECRET` |
| 访问令牌 | `COMMERCE_ACCESS_TOKEN` |
| 访问令牌密码 | `COMMERCE_ACCESS_TOKEN_SECRET` |

安全地存储这些值。 您在每个App Builder `.env`文件中都需要这些文件。


## &#x200B;3. Adobe Developer Console和App Builder

* 访问Adobe Developer Console组织
* **App Builder项目**（新项目或您重复使用的项目）
* 已配置工作区；提示假定为&#x200B;**[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]**&#x200B;作为服务添加到工作区
* **Commerce**&#x200B;已连接为工作区的事件提供程序

概念验证中使用的事件代码为： `com.adobe.commerce.observer.sales_order_place_before`

如果您尚未将Commerce作为事件提供程序进行连接，请参阅Commerce扩展性指南中的[配置Commerce以向Adobe I/O发出事件](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"}。


## &#x200B;4. 本地开发环境

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## &#x200B;5. 需要了解的两个项目目录

本教程使用两个单独的目录根。 把它们分开。

**Commerce项目根**（您的Magento Git存储库）：

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**App Builder项目根**（源包中的`split-payment-orchestrator`文件夹，或您创建的新项目）：

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_id与increment_id比较

> **在REST调用中始终使用`entity_id`（数字数据库ID），而不是`increment_id`（例如`000000042`）。**

从&#x200B;**[!UICONTROL Commerce Admin]**&#x200B;订单URL中查找`entity_id`：

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

或者，从模拟脚本中：

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. 100美元的门槛

拆分付款UI和阈值保护目标订单&#x200B;**$100.00或更少**。 流的行为如下所示：

* **在$100或以下：**&#x200B;将显示拆分付款UI；客户可以设置现金加商店信用拆分
* **超过$100：** `CheckoutPlugin`在付款步骤引发错误

要进行测试，请构建其小计、运费和税为&#x200B;**小于或等于$100**&#x200B;的购物车（例如，低于$90的商品，因此运费和税费仍低于上限）。

阈值存储在以下位置：

* Commerce配置： `split_payment/general/threshold` （在`etc/config.xml`中默认为`100`）
* App Builder环境： `PAYMENT_THRESHOLD=100` （必须匹配Commerce）


## &#x200B;8. Fastly（仅限Commerce Cloud）

App Builder操作会通过REST (`/rest/V1/split-payment/orders/...`)调用Commerce。 如果您的Commerce Cloud项目使用Fastly进行IP，则必须App Builder运行时出口IP地址。

**如何检查：**&#x200B;首先运行模拟脚本（直接使用OAuth签名进行curl）。 如果这有效，但App Builder操作返回`403`，则Fastly可能会阻止该请求。

使用Adobe当前有关App Builder出口IP范围的文档，并根据需要将它们添加到您的Fastly配置中。


## 验证核对清单

在开始生成提示之前，请确认以下内容：

* [ ] Commerce版本为2.4.5或更高版本
* [ ] I/O事件已启用(Commerce Cloud)并已部署
* [ ]启用了“货到付款”，其标题完全设置为`Cash`
* [ ]商店点数已启用
* [ ]测试客户至少有50美元的商店点数
* [ ]创建并激活Commerce集成，并保存所有四个OAuth值
* [ ] App Builder项目配置了I/O事件服务和Commerce事件提供程序
* [ ] `aio login`已完成，且已通过`aio app use`选择正确的工作区
* [ 已安装] Node.js 18或更高版本，并已安装`aio` CLI
* [ ] `.env`文件是根据[拆分付款POC：环境变量引用](split-payment-poc-env-reference.md)准备的（如果您使用源包，则还包括源包）


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
