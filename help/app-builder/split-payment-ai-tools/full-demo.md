---
title: 拆分付款POC — App Builder完整演示
description: 了解拆分付款、REST、App Builder I/O和操作员如何在此Luma演示中接受/拒绝工作，以及可阻止购物车的可配置预订单合计。
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 861
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---

# 创建拆分付款POC：App Builder完整演示

这是基于Adobe Commerce和Adobe App Builder构建的分段支付概念验证的端到端演练。 该演示假定您已经使用人工智能工具和提示来生成进程中的Commerce扩展和App Builder应用程序；本视频说明在该代码合并、使用本机Luma主题部署到Adobe Commerce Cloud网站并且App Builder项目处于实时状态之后发生的情况。

购物者支付部分现金和部分&#x200B;**[!UICONTROL Store Credit]**。 Commerce拥有同步签出功能以及店面需要的API；App Builder处理编排、操作员工作流和I/O事件使用者。 参考实施使用Commerce (PaaS)项目和Luma原生结账，而不是Edge Delivery Services店面，这仍然是许多商家的常用路径。 如果您在其他拓扑中使用&#x200B;**Adobe Commerce as a Cloud Service**，则App Builder代码保持不变，但店面和正在进行的工作会有所不同。 对于Luma上的本地、自托管和Commerce云中的Luma，本视频演示了在用于新功能的进程内代码和App Builder之间的实际划分。

## 视频

>[!VIDEO](https://video.tv.adobe.com/v/3484109?captions=chi_hans&learn=on)

## 此视频面向谁？

* 规划Commerce可扩展性的技术架构师
* 实施签出和集成的开发人员
* 需要在PHP和App Builder之间实现实际分割的实施团队

## 在店面上设置方案

演示帐户具有&#x200B;**[!UICONTROL Store Credit]**，您在结账时看到该余额。 添加产品，直到订单总计约为$80为止。 这个大小为您提供了空间，既可以显示成功的接受路径，也可以显示拒绝路径，类似于卡片拒绝，而无需数学算术与您作斗争。

**[!UICONTROL Split payment]**&#x200B;仅提供给已登录的客户，因为会话必须公开商店点数。 来宾签出不显示拆分选项。

1. 在&#x200B;**[!UICONTROL Payment]**&#x200B;步骤中，选择现金方式（演示将&#x200B;**[!UICONTROL Cash on delivery]**&#x200B;重命名为&#x200B;**Just Cash**）。
1. 此时付款方式下将显示一个拆分付款区域。 现金字段已预填充到订购总数，组件从会话中读取&#x200B;**[!UICONTROL Store Credit]**。
1. 对于~$80购物车，设置&#x200B;**$50**&#x200B;现金和&#x200B;**$30**&#x200B;商店点数，以便组件显示$50 + $30 = $80。
1. 金额有效后，下订单。

UI触发对用于拆分的新&#x200B;**[!UICONTROL Commerce]** REST API的调用。 签出保持同步： Commerce应用商店积分，在订单上存储拆分行，并发出一个App Builder以后使用的I/O事件。 对于客户而言，成功页面是即时的。

## 查看Commerce管理员中的订单

在&#x200B;**[!UICONTROL Sales]** > **[!UICONTROL Orders]**&#x200B;中，打开新订单。 **[!UICONTROL Payment information]**&#x200B;区域显示拆分，例如&#x200B;**$50**&#x200B;现金和&#x200B;**$30**&#x200B;商店点数。 尚未收取现金时状态为&#x200B;**待处理**；创建订单时已获得商店积分。

**[!UICONTROL Comments]**&#x200B;可以包含App Builder添加的文本。 App Builder中的&#x200B;**[!UICONTROL Payment orchestrator action]**&#x200B;接收了I/O事件，检查了拆分金额，并使用经过身份验证的REST附加注释。 **[!UICONTROL Commerce]**&#x200B;会像任何其他REST调用一样处理此调用，并且不需要知道编写器。

## 使用App Builder操作员功能板（ERP或后台）

简单的HTML体验作为&#x200B;**[!UICONTROL App Builder]** Web操作运行（此视图没有&#x200B;**[!UICONTROL Admin]**）。 仪表板将&#x200B;**[!UICONTROL Commerce Order]** REST API和筛选器调用到&#x200B;**待处理**，以便您能够找到$50的现金分支。

您通常将此模式与ERP或欺诈工具集成。 演示了两个操作：

* **接受** — 确认已收到现金（在生产中，通常是来自收银机或商店系统的自动发帖）。
* **拒绝** — 模拟欺诈或失败的收集步骤（在生产中，通常从风险服务中自动进行）。

**接受并完成订单**

1. 在&#x200B;**[!UICONTROL App Builder]**&#x200B;应用中，使用&#x200B;**接受**&#x200B;确认现金。
1. 应用程序调用一个新的&#x200B;**[!UICONTROL Commerce]**&#x200B;终结点以结束现金行。 核心订单流程（开票，在此版本中自动装运）以本机&#x200B;**[!UICONTROL Commerce]**&#x200B;行为运行。 在&#x200B;**[!UICONTROL Admin]**&#x200B;中，该路径都不需要人；编排和端点位于App Builder中。

在&#x200B;**[!UICONTROL Admin]**&#x200B;中刷新同一订单：当现金被接受时，状态将变为&#x200B;**完成**，其中包含发票，如果产品可发运，则包含发运以缩短演示中的完整生命周期。

**拒绝并取消**

1. 打开仍在拒绝方案中的预建订单。
1. 在&#x200B;**[!UICONTROL App Builder]**&#x200B;应用中，使用&#x200B;**拒绝**&#x200B;来模拟欺诈或集合失败。
1. 在&#x200B;**[!UICONTROL Admin]**&#x200B;中，订单为&#x200B;**已取消**。 历史记录显示已拒绝的现金状态，注释解释了结果，购物者并未像最终销售那样收取商店贷记额度，并且商店贷记发票因取消而失效。 生产系统还将通知客户，并解除对库存或服务的保留。

## 订单总阈值（创建订单前验证）

App Builder或&#x200B;**[!UICONTROL Commerce]**&#x200B;配置可以支持验证规则；此生成块阻止购物车值超过&#x200B;**$100**&#x200B;的订单（您可以更改的演示上限）。 值检查不是最常见的业务规则 — 库存或可用性检查比较典型 — 但它表明，在创建订单之前，可在&#x200B;**[!UICONTROL Commerce]**&#x200B;中的&#x200B;**[!UICONTROL Payment]**&#x200B;运行验证，而App Builder仍处理跟进自动化。

1. 添加产品，直至总数超过&#x200B;**$100**。
1. 仍然显示拆分&#x200B;**[!UICONTROL UI]**；超限结果仅出现在&#x200B;**下订单**&#x200B;上。
1. 购物者看到一般错误，例如&#x200B;**无法处理付款。 请重试或联系支持人员。**
1. **[!UICONTROL cart]**&#x200B;未更改，因此客户可以降低总计，选择其他方法或联系支持人员。

风险分数、区域规则或类似内容可以插入此处；**[!UICONTROL Commerce]**&#x200B;阻止订单，并且&#x200B;**[!UICONTROL App Builder]**&#x200B;可以拥有通知并在事后进行案例工作。

## 本地Commerce、云中的Commerce和&#x200B;**Adobe Commerce as a Cloud Service**

演练与您管理的基础结构上的&#x200B;**[!UICONTROL Adobe Commerce]**&#x200B;或传统店面的&#x200B;**[!UICONTROL Commerce in the cloud]** (PaaS)匹配。 对于具有其他前端且此形状中无进程内模块的&#x200B;**[!UICONTROL Adobe Commerce as a Cloud service]** (SaaS)，App Builder部分大致相同，而店面和扩展工作则不同。 在所有情况下，相同的原则都成立：让&#x200B;**[!UICONTROL Commerce]**&#x200B;执行&#x200B;**[!UICONTROL Commerce]**&#x200B;请求内必须发生的事情，并将&#x200B;**[!UICONTROL App Builder]**&#x200B;用于其余的体验。

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
