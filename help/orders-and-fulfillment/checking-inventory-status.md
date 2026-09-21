---
title: 库存状态检查：开发和性能
description: 了解如何评估是否需要在Adobe Commerce中进行实时清单检查，并查看您商店的开发和性能注意事项。
feature: Best Practices, Inventory
topic: Development, Performance
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 496
last-substantial-update: 2024-05-09
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
    internal-label: Order Management System
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
    internal-label: 2FA
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
    internal-label: B2B
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: fcf0f5116ba75bd618f4ed80591ecc96132cdb45
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%
---
# 库存状态检查开发和性能注意事项

库存的准确性是一个重要考虑因素。 有一些本机功能可以帮助确保这种风险尽可能低，例如延期交割和设置缺货阈值。 这两个主题都可以在[Adobe Experience League](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/inventory/configuration/backorders)上阅读以获取进一步说明。

在某些项目和用例中，需要对Adobe Commerce应用商店进行实时库存状态检查。 本教程提供了insight来处理此对话时的开发和性能注意事项。

## 验证是否需要此请求

准备尽可能多地讨论请求。 最重要的是验证此项目是否可接受本机功能。 查找此请求背后的推理，以验证Adobe Commerce的本机功能是否无法满足此请求。

另一个考虑因素是开发、测试和维护此功能的成本。 利益相关者的意见并不一定就意味着某种要求。 在Adobe Commerce核心功能之外进行库存验证会产生相关成本。 这些成本以技术债务、更多测试和验证以及其架构的使用文档和支持文档的形式提供。

## 确定可接受的库存更新节奏

尝试考虑清单检查以及如何通过3种方法完成检查。 每一种方法都有好处和限制。 它们还增加了复杂性，并且需要针对错误处理进行更多的测试和思考。 请记住，当您决定实施自定义解决方案时，会添加一些责任和注意事项。 示例包括回退流程、监控、测试和疑难解答，这些均由开发团队负责。 要包括的一些有用项目是新的支持文档、培训和监控，以确保开发团队可以支持整个功能。 副作用是开发团队拥有该流程，不再利用核心Adobe Commerce应用程序提供的本机功能。 Adobe支持无法协助进行此级别的自定义。

第一种方法是使用本机功能。 使用本机功能带来的风险最小，并且有许多好处。 如果采用这种方法，即表示您可以依赖Adobe Commerce为该功能的使用提供的所有现有文档和教程。 库存管理有很多方面，因此首先要考虑的是应用程序附带的内容。 但是，在某些情况下，在订购时商业中找到的数据不准确。 数据如何不同步的示例如下：允许直接在Order Management系统中的Adobe Commerce应用程序之外进行销售。 一个原因是，要确保在Adobe Commerce中表示准确的库存水平，需要某种集成，以使Adobe Commerce信息尽可能接近准确。 如果不能接受过度销售，则添加缺货阈值是在达到零之前阻止物料销售的好方法。 Adobe Commerce的本机同步功能每天最多为1次。 此频率对于某些用例已经足够，但对于其他用例则不够频繁。 有关详细信息，请阅读[计划的导入和导出](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export)。

第二种方法是`near real-time`。 近乎实时仍使用本机功能。 但是，这需要进行一些额外工作，以提供集成，该集成会经常向商业馈送以按计划更新其清单。 例如，每小时。 此选项需要考虑集成的工作方式，但使用“批量api”并拥有一些中间件来执行数据转换并将其推送到商业是一种好方法。 考虑使用Adobe App Builder或类似平台来完成大量工作，并以更频繁的节奏将信息推送到Adobe Commerce。

第三种方法，也是风险最大、责任最复杂的方法，是对外部API或数据源的实时实时清单检查。 对外部系统进行实时库存检查存在风险，并且还有其他几个需要考虑的因素。 以下是需要评估的一小部分其他内容：

* 外部系统能否接受REST或GraphQL请求
* 端点是否存在任何与网站流量不一致的限制，例如每分钟的X个请求数
* 加载下的响应时间有何变化
* 如果响应时间较长，会出现什么情况？是否自动终止此项，并使用回退选项，如本机清单。
* 可以使用什么类型的监控来确保API请求在容差限制之内

## 非本机清单管理的注意事项

尽可能保持自定义项不复杂。
库存的组织有多平坦，是1 sku还是可用库存的总量，或者还有其他属性需要考虑。

如果库存信息相当平坦（例如sku和总可用数量），则近乎实时的选项会被展开。 “近实时”概念是指有一个从源收集库存，然后填充用于响应请求的存储引擎的后台操作。 为此，您可以使用Redis、Mongo或其他非关系数据库。 这些选项非常快速，非常适用于键/值对。 如果数据更复杂，则需要在商务应用程序内部或外部使用关系数据库。 通过从商务数据库卸载此项，可以将核心商务应用程序与这些事务隔离。 另一组好处是使来自商业应用程序、CPU、RAM和其他应用程序的I/O不再使用。 要从Adobe Commerce应用程序服务器中保存资源，请利用新API从站外存储中提取数据。 此过程需要一个中间件来帮助转换任何数据。 然后，确保调用应用程序可以获得预期的结果。 通过使用带有API网格的Adobe App Builder，可以转换数据并返回格式正确的数据。

在有多个库存来源时，将Adobe App Builder与API网格结合使用也是一个很好的选择。


## 将执行逻辑移出进程

Adobe Developer App Builder提供了一个统一的第三方可扩展性框架，用于集成和创建自定义体验以扩展Adobe解决方案。 Adobe Commerce可以使用Adobe Developer App Builder。 此方法是一个极好的用例，可用于扩展某些通常在核心应用程序中出现的功能并将其移动到异地。 通过从Commerce应用程序中移除功能，这会减少Commerce应用程序的模块数量和复杂性。 反过来，流程中自定义项数量减少会降低升级和维护的复杂性。

为了启发如何完成此任务，Adobe的团队已创建了一些文档，这些文档是灵感的绝佳来源，并提供了工作代码示例。 当购物者将产品添加到购物车时，第三方库存管理系统检查该商品是否有库存。 如果是，则允许添加产品。 否则，显示错误消息。 有关代码示例和详细信息，请转到[Webhook用例](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart)。

## 何时进行清单检查

何时检查库存是否仍然可用取决于业务利益相关者，软件架构师将听取其他关键利益相关者的一些意见。 某些适当的时间包括：将项目添加到购物车时，以及进入结帐工作流时。 任何其他事件都会在不需要时将负载添加到后端系统。 请记住，我们的目标是只有在库存问题最为重要时才能发现问题。 请仔细考虑影响库存状态检查总体目标的其他检查，并仅在利益相关者了解额外负载的潜在风险时才允许进行这些检查。

## 研究您的库存来源

需要对外部库存来源进行全面调查。 应评估的项目包括：可用的API选项、对GraphQL的支持以及预期的响应时间。 如果库存源的连接带宽有限或者从未打算在实时请求中使用，则排除使用能力，因此架构师需要考虑采用近乎实时的方式。 如果API请求次数超过定义的参数，则不会将其作为一个可行的选项。 例如，对于一次性请求，api响应为200毫秒，但在中等负载下会上升到500到900毫秒。 随着负载增加，这种情况会变得更糟，并会禁用实时库存调用。

请务必使用简单的请求以及与实时网站上预期流量相似的大量数据来测试API响应时间。 切记同时测试商务中的所有区域以模拟真实世界的场景。 如果在产品页面、购物车中以及结账期间发生实时库存调用，则负载测试必须同时模拟所有这些调用，以模拟真实的客户行为。

## 回退选项

如果清单源已关闭并且监视可用，则建议使用Adobe Commerce的本机功能。 但是，通过适当的监控，客户体验可以动态变化以反映实时库存检查的丢失。 这意味着为了避免过度销售，销售或活动会提前取消或从显示内容中删除。 与店主讨论回退计划，以便每个人都了解在库存来源停止工作时接管任务的自动流程。

## 结论

实时检查库存的决定很重要。 确保网站所有者、开发团队和其他人接受全面教育，并了解所有收益和潜在隐患取决于开发人员主管或架构师。 通过提供周到的计划（包括原因和回退流程）是获得成功的关键。

实时清单检查可以完成，但需要在QA周期期间围绕测试和验证进行研究和思考。 确保负载测试和端到端自动化测试有助于确保捕获并分类所有潜在问题。

如果监控检测到呼叫失败或响应时间缓慢，请采取措施使网站保持在线状态并最大限度地减少客户的不满。 后备选项包括使用本机功能到禁用促销、通知开发团队或将请求重新路由到辅助后端系统。 由于每个系统在某个时间点都会遇到问题，因此应当像实际集成一样仔细规划回退机制的实施方式。 任何自动化或需要手动操作的内容都应明确记录。
