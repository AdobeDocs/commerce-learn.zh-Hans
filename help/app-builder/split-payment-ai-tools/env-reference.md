---
title: 拆分付款POC：环境变量参考
description: 了解如何将Commerce OAuth、基本URL、支付阈值和可选演示设置映射到Orchestrator、UI扩展和模拟环境文件。
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: d5f1e76c3a5127698f2933810fca218b79082571
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 拆分付款POC：环境变量参考

在每个组件中使用相同的四个Commerce OAuth凭据。 在&#x200B;**[!UICONTROL Commerce Admin]**&#x200B;中，创建一个&#x200B;**[!UICONTROL Integration]**，然后重复使用下面每个`.env`文件中的四个值。 （有关激活步骤，请参阅[拆分付款POC：先决条件和环境设置](./prerequisites-and-setup.md)。）

## 四个OAuth凭据（在所有位置使用）

| 变量 | 从何处获得 |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[您的集成]* |
| `COMMERCE_CONSUMER_SECRET` | 与上述相同 — 值仅在激活时显示 |
| `COMMERCE_ACCESS_TOKEN` | 与上面相同 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 与上面相同 |


## App Builder orchestrator

### `split-payment-orchestrator/.env`

从orchestrator目录中的`.env.example`复制。 不提交此文件。

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Experience Cloud UI扩展(commerce-checkout-starter-kit)

### `commerce-checkout-starter-kit/.env`

此组件使用两个凭据集：IMS用于使用&#x200B;**[!UICONTROL Admin]** UI SDK进行订单列表，OAuth 1.0a用于接受和拒绝操作。

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## 模拟脚本

### `commerce-backend-ui-1/.env.simulation`

从同一目录中的`.env.simulation.example`复制。

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## 注释

**`PAYMENT_THRESHOLD`** — 必须匹配&#x200B;**[!UICONTROL Commerce]**&#x200B;系统配置中的`split_payment/general/threshold`。 如果缺少值、不是数值或小于或等于`0`，则两侧均默认为`100`。 如果您在&#x200B;**[!UICONTROL Commerce]**&#x200B;中更改阈值，请更新App Builder `.env`以匹配。

**`DEMO_UI_SECRET`** — 可选，但建议用于任何非localhost的部署。 具有仪表板URL的任何人都可以列出订单，如果列表为空，则运行“接受”和“拒绝”。 对于实际的暂存环境，请设置共享密钥。

**`COMMERCE_BASE_URL`** — 绝不要包含结尾斜杠。 Commerce REST客户端会自动附加`/rest/V1/`。

**`AIO_CLI_ENV`** — 对于&#x200B;**[!UICONTROL Stage]**&#x200B;工作区，设置为`stage`。 部署到&#x200B;**[!UICONTROL Production]**&#x200B;时更改为`prod`。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
