---
title: 拆分付款POC：教程快速参考
description: 了解拆分付款POC文件映射：哪个Experience League页面与Luma结账的每个AI提示、建议的部分顺序和创作注释相匹配。
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '1455'
ht-degree: 0%

---

# 拆分付款POC：教程快速参考

本页总结了如何组织分割支付概念验证教程系列。 它会将每个编号的源提示文件映射到其发布的Experience League页面，列出读者建议的分区顺序，并收集作者和维护者的注释。


## 逐文件引用

### [创建拆分付款POC：App Builder和AI工具](create-a-split-payment-poc-app-builder-and-ai-tools.md)

**Source标签：** `00_TUTORIAL_OVERVIEW.md` （概念性概述；发布的系列随此页面打开。）

**目的：**&#x200B;本教程的简介和方向。

**为什么需要它：**&#x200B;在“如何”之前为开发人员提供“原因”。 解释他们将构建的内容、受众是谁，以及关键是，如果他们的站点与干净的Luma安装不同，则必须自定义哪些内容。 还用作系列其余部分的目录。

**教程使用：**&#x200B;正在打开部分。 在任何技术步骤之前设置上下文。


### [拆分付款POC：架构和设计决策](split-payment-poc-architecture-and-decisions.md)


**目的：**&#x200B;对PoC中的每个架构决策进行深入说明。

**为什么需要它：**&#x200B;使用此PoC时最常见的困惑点是了解&#x200B;*为什么* PHP与App Builder中的某些内容。 如果没有此上下文，开发人员会尝试移动无法移动的项目（例如商店信用申请）并失败。 本文档用明确的推理来预防这些故障。

**涵盖的关键主题：**

* “什么必须保持在PHP中”规则及其原因
* 阈值双重执行模式
* 为什么商店信用申请不能异步
* 插件处理的五个Commerce边缘案例
* 扩展属性数据从→的结账→报价→I/O事件→订单流出

**教程使用：**“架构”或“工作原理”部分。 经验丰富的Commerce开发人员可以跳过，但对于App Builder的新手至关重要。


### [拆分付款POC：先决条件和环境设置](split-payment-poc-prerequisites-and-setup.md)


**用途：**&#x200B;在编写代码或运行提示之前完成预检核对清单。

**为什么需要它：**&#x200B;安装阶段在教程中具有最高的失败率。 本文档确保正确配置Commerce管理员（准确的COD标题、启用商店点数、测试客户余额、创建集成），并确保App Builder项目连接I/O事件和Commerce事件提供程序，以及本地环境具有正确的版本。

**强调项：**

* 投放时收现标题必须刚好是`Cash`（关键 — JavaScript进行字符串匹配）
* Commerce集成必须&#x200B;*激活*（不只是保存），OAuth凭据才能正常工作
* 此处解释了`entity_id`与`increment_id`的区别，以防止在整个

**教程使用：**“先决条件”或“入门”部分。 应该以交互方式完成 — 而不仅仅是阅读。


### [拆分付款POC：环境变量引用](split-payment-poc-env-reference.md)


**用途：**&#x200B;所有三个组件的所有环境变量都集中在一处。

**为什么需要它：**&#x200B;相同的四个OAuth凭据出现在三个具有不同变量名称（`COMMERCE_*`与`COMMERCE_INTEGRATION_*`）的不同文件中。 只有一个引用会阻止在一个凭据集出现拼写错误时进行“为何无法正常工作”调试。

**组件已覆盖：**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` (IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**教程使用：**&#x200B;引用侧边栏或“配置”部分。 还用作生成提示的助手。


### [拆分付款POC：Commerce模块AI提示](split-payment-poc-commerce-module-prompt.md)


**用途：**&#x200B;完整、自包含的AI提示生成整个`Client_SplitPayment` PHP模块。

**为什么需要它：**&#x200B;这是PHP端的主AI生成工件。 编写此脚本的目的是让没有PHP经验的开发人员可以将其交付给Cursor（使用Claude）并获得工作模块。 每个文件都被指定，每个行为契约都被定义，关键实现注释被包含在内，并且提供了用于以后启用该模块的命令。

**覆盖范围：**

* 完整的文件结构（30多个文件）
* 数据库架构、扩展属性、REST端点、I/O事件配置
* 具有完整行为规范（不仅仅是签名）的所有PHP类
* KnockoutJS签出组件规范
* 这五个边缘大小写插件包含有关每种大小写存在原因的解释
* 非Luma主题的自定义标注

**教程用法：**“构建Commerce模块”部分。 提示本身就是人工因素 — 开发人员将其复制到他们的人工智能工具中并运行它。


### [拆分付款POC：App Builder orchestrator AI提示](split-payment-poc-app-builder-orchestrator-prompt.md)


**用途：**&#x200B;用于生成`split-payment-orchestrator` App Builder应用程序的完整、自包含的AI提示。

**为什么需要它：**&#x200B;四个App Builder操作(`payment-orchestrator`、`payment-accept`、`payment-decline`、`demo-dashboard`)是“移出PHP的内容”故事的核心。 此提示会生成所有具有完整行为规范、正确`app.config.yaml`结构和事件注册配置的规则集。

**覆盖范围：**

* 包含所有四个操作和I/O事件注册的`app.config.yaml`
* `commerce-client.js` OAuth 1.0a共享客户端
* 具有完整逻辑规格的全部四个操作
* `store-credit.js`已弃用的桩模块（解释&#x200B;*为什么*&#x200B;它未使用 — 这在教学上很重要）
* 演示仪表板，具有HTML渲染、订单过滤和安全性
* 包含所有变量的`.env.example`

**教程使用：**“构建App Builder应用程序”部分。 伴随Commerce module提示符。


### [拆分付款POC：Experience Cloud UI扩展AI提示](split-payment-poc-experience-cloud-ui-prompt.md)


**用途：** AI提示生成可选的Experience Cloud Admin UI SDK扩展。

**为什么需要它：** Orchestrator提示中的演示仪表板刻意粗糙 — 它是概念验证，而不是生产。 此部分向开发人员展示下一步骤：使用管理UI SDK将拆分付款仪表板嵌入到Commerce管理外壳程序中。 原始提示中完全缺失它。

**覆盖范围：**

* Admin UI SDK扩展的`ext.config.yaml`
* 订单功能板和订单详细信息的React组件
* 后端操作使用IMS身份验证进行列出，使用OAuth接受/拒绝
* 模拟脚本（也用于测试）

**教程使用：**&#x200B;可选的“更进一步”或“生产路径”部分。 如果本教程仅侧重于PoC，则可以跳过。


### [拆分付款POC：测试和验证指南](split-payment-poc-testing-and-verification.md)


**用途：**&#x200B;分步测试指南按正确的验证顺序覆盖了每个组件。

**为什么需要它：**&#x200B;生成组件只完成一半的工作。 开发人员需要明确的验证路径，从最低级别（数据库列、REST端点）开始，构建到完整的端到端流程。 最初的提示有一个设置核对清单，但没有明确的测试流程。

**覆盖范围：**

* Commerce模块安装验证
* 管理员配置验证
* REST端点冒烟测试（curl命令）
* 结账UI演练（包括验证案例）
* 阈值保护测试
* 全订单投放测试
* 接受和拒绝的模拟脚本用法
* 演示仪表板使用情况
* App Builder操作日志检查
* 具有特定修复的10种常见故障模式

**教程使用：**“测试”或“验证”部分。 也可用作故障排除参考。


### [拆分付款POC：概念验证后的后续步骤](split-payment-poc-next-steps.md)


**用途：**&#x200B;将PoC发展为生产就绪模式的路线图。

**为什么需要它：** PoC教程可能会让开发人员知道“现在怎么办？” 感觉。 本文档将演示的自然进展映射到生产：使用Experience Cloud扩展替换演示仪表板，连接真正的ERP，添加API Mesh，扩展App Builder工作流程，以及生产部署核对清单。

**密钥内容：**

* 体系结构演变图（PoC →阶段2→阶段3）
* ERP集成模式（哪些发生了变化，哪些保持不变）
* API网格代理模式
* 生产部署核对清单
* 关键参考链接

**教程使用：**“后续步骤”结束部分。 对于设定实际项目的范围而言，作为对话起始者也很有用。


## 建议的教程部分

基于这些文件，读者适用的典型结构为：

| 教程部分 | Experience League页面 |
| --- | --- |
| 简介和学习目标 | [创建拆分付款POC：App Builder和AI工具](create-a-split-payment-poc-app-builder-and-ai-tools.md) |
| 端到端演示（视频） | [创建拆分付款POC：App Builder完整演示](create-a-split-payment-poc-app-builder-and-ai-tools-full-demo.md) |
| 架构：哪些人生活在哪里 | [拆分付款POC：架构和设计决策](split-payment-poc-architecture-and-decisions.md) |
| 先决条件和设置 | [拆分付款POC：先决条件和环境设置](split-payment-poc-prerequisites-and-setup.md) |
| 环境变量 | [拆分付款POC：环境变量引用](split-payment-poc-env-reference.md) |
| 步骤1：构建Commerce模块 | [拆分付款POC：Commerce模块AI提示](split-payment-poc-commerce-module-prompt.md) |
| 步骤2：构建App Builder orchestrator | [拆分付款POC：App Builder orchestrator AI提示](split-payment-poc-app-builder-orchestrator-prompt.md) |
| 步骤3：测试端到端流量 | [拆分付款POC：测试和验证指南](split-payment-poc-testing-and-verification.md) |
| 第4步（可选）：管理员UI扩展 | [拆分付款POC：Experience Cloud UI扩展AI提示](split-payment-poc-experience-cloud-ui-prompt.md) |
| 后续步骤和生产路径 | [拆分付款POC：概念验证后的后续步骤](split-payment-poc-next-steps.md) |


## 面向教程作者的重要说明

**根据Luma主题假设：**&#x200B;每个生成提示都生成用于干净的Luma安装的代码。 教程应在顶部清楚地说明这一点 — 具有自定义结账的开发人员将需要调整`LayoutProcessorPlugin`注入路径并可能调整RequireJS配置。 系列简介和构建提示包括此内容的自定义标注。

**在PHP模块上：**&#x200B;该教程显式不提供PHP代码作为复制粘贴下载。 AI提示符&#x200B;*生成*&#x200B;它。 这是有意为之 — 它教导了使用AI创建Commerce扩展的模式，而不仅仅是复制粘贴样板。 但是，当在干净环境中出现提示时，生成的代码应该与`app/code/Client/SplitPayment/`中的工作代码完全匹配。

**在$100阈值上：**&#x200B;这是硬编码的PoC值。 本教程应注意，实际商店将需要通过Commerce管理配置而不是编译时常量来配置此设置。

**商店点数依赖关系：**&#x200B;生成的拆分付款流需要`Magento_CustomerBalance`（本地商店点数模块）。


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
