---
title: Adobe Commerce Developer Agent App Builder练习
description: 通过这个动手实践的Commerce练习，了解如何使用Adobe Commerce Developer Agent构建、部署和测试三个App Builder可扩展性用例。
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 6ce75fe023cfb9c3be988787e8993db556cf3150
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 0%

---

# Adobe Commerce Developer Agent App Builder练习

使用Commerce Developer Agent (CDA)构建、部署和测试Adobe Commerce可扩展性用例的实践演练。 此练习涵盖三个用例：购物车数量限制webhook、高价值订单暂挂和事件驱动的暂挂订单存档（从Blueprint到功能测试）。

## 快速入门

### 如何报告问题和反馈

在整个练习过程中，您会遇到粗边 — 使用新特征时预期会出现粗边。 使用新用户引导期间提供的反馈模板，捕获任何问题并与您的Adobe项目联系人共享。

>[!TIP]
>
> 在报告问题时：
>
> * 包含`projectId` （显示在浏览器URL中）。
> * 在相关时包含屏幕截图。

### 先决条件

**帐户和访问权限**

* 在早期访问IMS组织中至少&#x200B;**开发人员**&#x200B;角色。
* 管理员可访问该组织中的Adobe Commerce as a Cloud Service (ACCS)实例，该实例位于&#x200B;**Cloud Service实例**&#x200B;下的experience.adobe.com。
* GitHub帐户。

**工具**

功能验证需要Edge Delivery Services (EDS)店面。 您将需要：

* Node.js 22+
* Adobe I/O CLI： `npm install -g @adobe/aio-cli`
* AIO CLI Commerce插件： `aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

将店面模板安装在空文件夹中，并在出现提示时选择您的ACCS实例：

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

启动店面：

```bash
cd storefront
npm run start
```

## 打开Commerce Developer Agent

1. 导航到&#x200B;**开发人员代理**&#x200B;下的Commerce开发人员代理，网址为experience.adobe.com。
1. 使用早期访问IMS组织凭据登录。

## 用例1：购物车最大单位webhook

此用例在使用同步Commerce webhook添加产品之前验证购物车数量限制。

### Blueprint阶段

输入以下提示并单击&#x200B;**生成Blueprint**：

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> 查找以下内容：
>
> * 将创建捕获需求的Blueprint (v1)。
> * 创建用于指导实施的任务。

通过在聊天框中输入详细信息或单击聊天框上方的某个丸子来优化Blueprint（*挑战假设*、*查找设计差距*&#x200B;等）。 在您满意后，单击&#x200B;**批准计划**&#x200B;以继续。

### 开发阶段

代理程序将过渡到“开发”阶段，并开始预配工作区。

>[!NOTE]
>
> 在“资源管理器”面板中查找以下文件：
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

配置完毕后，代理将显示实施任务的列表并开始生成。

>[!NOTE]
>
> 查找以下内容：
>
> * 生成的代码符合要求。
> * `Validate`流屏幕显示工作区验证进度(`aio app build`)。
> * 如果验证失败，代理会自行更正生成的代码。

对代码满意后，单击&#x200B;**集成**&#x200B;选项卡以继续。

### 配置集成

**连接或创建App Builder工作区**

要创建或连接App Builder项目，请按照屏幕上的说明操作。

如果连接到现有工作区，请确保它具有：

* 已添加`Runtime`服务。
* 添加了以下API：Adobe Commerce as a Cloud Service、I/O管理API、App Builder数据服务、I/O事件、Adobe I/O Events for Adobe Commerce。

如果创建新工作区，请手动添加&#x200B;**Adobe Commerce as a Cloud Service** API。

>[!IMPORTANT]
>
> 连接到现有App Builder项目后，展开&#x200B;**高级配置**&#x200B;并粘贴工作区JSON，然后单击&#x200B;**重新检查状态**&#x200B;以确认已安装所有必需的API。

单击&#x200B;**下一步**&#x200B;继续。

**连接到Commerce**

从列表中选择您的ACCS实例，或在&#x200B;**Commerce REST基本URL**&#x200B;字段中输入URL，然后单击&#x200B;**连接Commerce实例**。 单击&#x200B;**下一步**&#x200B;继续。

**连接到GitHub**

通过输入存储库URL并使用GitHub应用程序或个人访问令牌，将工作区连接到GitHub存储库。 单击&#x200B;**下一步**&#x200B;继续。

**配置环境变量**

填写项目所需的任何环境变量。

### 部署

单击&#x200B;**开发**&#x200B;返回开发阶段，然后在提示字段中要求代理进行部署。

>[!NOTE]
>
> 查找“确认部署”消息，其中显示“组织”、“项目”、“Workspace”和“运行时”命名空间。

确认部署。

>[!NOTE]
>
> 查找：
>
> * 显示预部署验证进度的`Validate`流屏幕。
> * 如果验证失败，代理会自行更正代码。
> * 显示部署进度(`aio app deploy`)的`Deploy`流屏幕。
> * 如果部署失败，代理将自行更正代码。

### 在应用程序管理中关联应用程序

1. 导航到您的ACCS实例管理员URL并登录。
1. 在左侧菜单中选择&#x200B;**应用程序**，然后选择&#x200B;**应用程序管理**。
1. 单击&#x200B;**+关联应用程序**（右上方）。
1. 选择CDA部署到的项目和Workspace，然后单击&#x200B;**关联**。

>[!NOTE]
>
> 查找显示应用程序名称和版本以及已实施功能（业务配置、Webhook、事件等）的卡。

### 在App Management中安装和配置

1. 在应用程序的行上，单击&#x200B;**安装**，然后单击&#x200B;**关闭**。
1. 在同一行中，单击&#x200B;**配置**&#x200B;以填写业务配置值，然后单击&#x200B;**关闭**。

>[!NOTE]
>
> 查找一个表格，其中显示Blueprint指定的每个配置字段，并预填了您指定的默认值。

### 功能测试

1. 在“应用程序管理”应用程序配置中，将&#x200B;**最大购物车单位**&#x200B;设置为3（快速测试的最小值）。
1. 在店面，从空的购物车开始。
1. 从产品详细信息页面(PDP)添加产品，直到总数量超过3 — 最后一次添加失败。
1. 在PDP上，您看到： *“您已达到项目的最大数量。”*
1. 在限制下，添加仍会成功。

>[!NOTE]
>
> 在产品列表页面(PLP)中，被阻止的添加以静默方式失败，并且没有消息 — 这是店面行为，而不是webhook失败。 希望使用PDP进行验证。

## 用例2：高价值订单暂挂和验证代码

导航回&#x200B;**Blueprint**&#x200B;阶段以开始此用例。

### Blueprint阶段

输入以下提示并单击&#x200B;**生成Blueprint**：

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> 查找以下内容：
>
> * 将创建一个捕获需求的Blueprint (v2)。
> * 原始计划任务将保留。
> * 添加了对应于新要求的新任务。

根据需要优化Blueprint，然后单击&#x200B;**批准计划**&#x200B;以继续。

### 开发、部署、关联和安装

按照使用案例1中使用的相同流程，从需求迁移到已安装的应用程序 — 无需重新配置集成。

>[!IMPORTANT]
>
> 若要选取对已关联应用所做的更改，您需要在应用管理中再次&#x200B;**取消关联**&#x200B;和&#x200B;**关联**。

### 功能测试

1. 在App Management应用程序配置中，将&#x200B;**订单保留阈值(USD)**&#x200B;设置为50（在测试购物车中容易超出）。
1. 确认订单自定义属性存在（默认`lab_verification_code`）。
1. 下单金额总计超过50美元的订单。
1. 等待约30秒（事件是异步的；非优先级投放最多可能需要约59秒）。
1. 在Commerce Admin → Sales → Orders中，打开订单。 状态为&#x200B;**已搁置** (`holded`)；自定义属性包含具有随机值的`lab_verification_code`。
1. 可选：首先下达低于$50的订单 — 此处理程序不会将其置于暂停状态。

## 用例3：针对持有订单的事件驱动存档

导航回&#x200B;**Blueprint**&#x200B;阶段以开始此用例。

### Blueprint阶段

输入以下提示并单击&#x200B;**生成Blueprint**：

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> 查找以下内容：
>
> * 将创建一个捕获需求的Blueprint (v3)。
> * 原始计划任务将保留。
> * 添加了对应于新要求的新任务。

根据需要优化Blueprint，然后单击&#x200B;**批准计划**&#x200B;以继续。

### 开发、部署、关联和安装

按照以前使用案例中使用的相同流程，从需求迁移到已安装的应用程序 — 无需重新配置集成。

>[!IMPORTANT]
>
> 若要选取对已关联应用所做的更改，您需要在应用管理中再次&#x200B;**取消关联**&#x200B;和&#x200B;**关联**。

### 功能测试

1. 确保用例2阈值足够低，可以进行测试（例如，应用程序配置中为$50）。
1. 在该阈值上方下达订单，以便用例2将其置于保留状态（约30秒）。
1. 在Adobe Developer Console →项目→暂存→事件中，打开暂挂订单存档事件的注册（在安装时添加或更新）。
1. 确认在订单移至暂挂状态后，事件已传送至该登记表。 对链接到`order-archive/archive-held-order`的Commerce事件使用事件跟踪或监视。

>[!NOTE]
>
> 事件是异步的 — 在订单被置于保留状态后，最多允许约30-59秒。

## 故障排除

如果CDA生成的应用程序未按预期运行或产生错误，请让代理从开发阶段进行故障排除。

>[!NOTE]
>
> CDA无法查看在其外部发生的步骤。 关联、安装、配置和功能测试均在Commerce Admin、App Management或店面中运行，而不是在CDA中运行。 如果问题出现在这些区域之一，则代理看不到问题发生，因此请给出问题所缺少的内容：
>
> * 您执行的操作和执行的位置（例如，“在应用程序管理中单击了安装”）。
> * 你所期望的。
> * 发生了什么。
> * 屏幕上显示的确切错误文本或消息。
> * 来自浏览器控制台或Adobe Developer Console App Builder日志和事件注册调试跟踪的任何相关错误。

报告越具体，代理诊断问题的能力就越强。

## 可选步骤

**下载代码**

要继续优化或编辑收藏的IDE，请单击“开发暂存资源管理器”工具栏上的下载图标来下载CDA生成的代码。 选择目标文件夹，然后单击&#x200B;**保存**，然后解压缩工作区包。

>[!NOTE]
>
> 查找：
>
> * “开发舞台资源管理器”中显示的所有文件都位于解压缩的文件夹中。
> * 使用`aio app build`构建项目时没有“编译”错误。

要使用CDA使用的相同座席技能，请在项目文件夹中安装这些技能：

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

然后启动IDE或CLI并开始提示。

**通过文件或链接附加上下文**

您可以使用文本文件或链接附加上下文，而不是直接在Blueprint或开发阶段中提示：

1. 单击聊天框上的附件图标。
1. 单击&#x200B;**添加文件**&#x200B;上载本地文本文件，或者输入URL并单击&#x200B;**添加链接**&#x200B;通过远程文件添加上下文。
1. 单击&#x200B;**完成**&#x200B;并输入提示以轻推代理。

>[!NOTE]
>
> 查找将附件中的上下文合并到下一阶段的代理。

## 已知问题和解决方法

**Blueprint阶段未生成任务**

要取消阻止并继续，请推动代理以生成任务。

**要推送到GitHub和从GitHub中提取的按钮无法正常工作**

相反，应从开发阶段下载项目ZIP文件。

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

<!-- ## Additional resources -->

<!-- Link to related Experience League or Adobe Developer documentation. -->
