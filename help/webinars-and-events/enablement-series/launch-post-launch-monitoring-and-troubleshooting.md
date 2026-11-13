---
title: Adobe Commerce Cloud的启动、监控和故障排除
description: 了解如何使用暂存环境、详细支持票证和共享监视工具准备、启动、监视和对Adobe Commerce Cloud进行故障排除。
feature: Best Practices, Cloud
topic: Architecture, Commerce, Performance
old-role: Architect, Data Architect, Leader
role: Developer, Leader
level: Beginner, Intermediate
doc-type: Technical Video
duration: 3576
last-substantial-update: 2024-05-07
jira: KT-15467
exl-id: e3748ec2-b449-4cc2-927d-e7ba4f4dbaf8
TQID: 'https://experienceleague.adobe.com/sSK6X9LrKA5LUgdELTzfyNZu8RP9juyYGDdGQfPni4w'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
role_v2:
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
    internal-label: Leader
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: 7ac0cbe05d658701f20137fdea7b9c0b4423d57b
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%
---
# Adobe Commerce Cloud的启动、监控和故障排除

此网络研讨会提供了有关准备和启动网站的几个最佳实践和提示。 它强调选择适当的暂存环境并在支持票证中提供准确详细信息以促进与支持团队进行有效沟通的重要性。 为更好地进行协作和解决问题，建议使用与支持团队相同的监控工具，例如New Relic。 重点介绍对外围系统（如支付处理器）进行全面测试，以确保它们在启动期间正常运行。 该文件还强调，需要预测和规划潜在的生产问题，如性能瓶颈和高速缓存预热问题，以确保顺利成功地启动。 了解共享责任模型是讨论的另一个关键方面，其中阐明，某些任务（如数据库恢复和应用程序代码安全）是网站所有者的责任。 通过遵循这些最佳实践，可以简化网站启动操作，主动解决潜在问题，并成功启动启动

## 受众

* 开发团队、经理、主要开发人员、技术架构师
* 将Adobe Commerce作为升级、迁移或新商业产品进行实施的团队

## 视频内容

* 选择适当的暂存环境并在票证中提供相关详细信息。
* 使用与支持团队相同的监控工具(New Relic)，以便更好地沟通和解决问题。
* 描述问题时提供屏幕截图或New Relic链接，以帮助支持人员更有效地了解和故障排除。
* 了解共同责任模式，特别是在数据库恢复和安全性方面。
* 测试外围系统（如支付处理器），并确保在转换期间正确处理订单ID。
* 在生产期间预测和规划潜在问题，例如性能瓶颈和缓存预热需求。


>[!VIDEO](https://video.tv.adobe.com/v/3428990?learn=on)

## 相关教程

* [云基础架构和自托管：启动前检查清单](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/cloud-infrastructure-and-self-hosted/cloud-operations/commerce-cloud-best-practices/pre-launch-checklist)
* [云基础架构和自托管： New Relic警报](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/cloud-infrastructure-and-self-hosted/cloud-operations/tools/new-relic/new-relic-alert-notification-setup)
* [创建支持票证](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/monitoring-analytics-support/help-and-support/create-a-support-ticket)

{{$include /help/_includes/enablement-series-related-links.md}}
