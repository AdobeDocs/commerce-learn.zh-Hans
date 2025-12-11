---
title: Quality修补工具
description: 了解在诊断问题、找到解决方案并应用现有可用修补程序列表中找到的修补程序时，如何使用质量修补工具。
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# Quality修补工具

了解在诊断问题、找到解决方案并应用现有可用修补程序列表中找到的修补程序时，如何使用质量修补工具。

## 您将学习的内容

了解如何分类问题，然后使用一些基本技术查找质量补丁以应用修复。

## 受众

* 开发人员了解如何查找问题并利用此工具为已知问题应用GIT修补程序

## 视频内容

Quality Patches Tool是适用于Adobe Commerce和Magento Open Source的命令行实用程序。 下面是它允许您执行的操作：

* 查看有关最新质量修补程序的一般信息。
* 将高质量的修补程序应用到您的安装。
* 如果需要，还原应用的修补程序

这些修补程序由Adobe开发人员Magento Open Source社区开发，用于增强稳定性和性能。 请记住，不建议应用大量修补程序，因为这会使未来的升级复杂化。

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## 为何使用品质修补工具

如果您希望执行以下操作，可能需要使用Adobe Commerce或Magento Open Source的Quality Patches Tool：

增强稳定性和性能：高质量的修补程序可解决问题、提高安全性并优化安装。
保持最新：应用修补程序可确保您的系统为最新状态并受到保护。
还原更改：如果修补程序导致意外问题，您可以使用工具还原它。 请记住，它最适合应用数量有限的修补程序，以避免使未来的升级复杂化。  

## 使用质量修补工具的限制或问题

虽然质量修补程序工具提供了许多好处，但需要牢记以下几点：

* 兼容性：确保这些修补程序与特定版本的Adobe Commerce或Magento Open Source兼容。
* 测试：始终先在暂存环境中测试修补程序，然后再将其应用于生产。 可能会出现意外问题。
* 修补程序依赖关系：某些修补程序可能依赖于其他修补程序。 了解任何先决条件。
* 自定义项：如果您进行了自定义代码更改，则修补程序可能会发生冲突。 仔细查看更改。
* 备份：在应用修补程序之前备份安装，以避免数据丢失。

虽然质量修补程序工具可用于应用数量有限的修补程序，但不建议用于处理大量修补程序。 应用过多的修补程序会使未来的升级和维护工作变得复杂。 如果您要应用许多修补程序，请考虑其他方法或咨询Magento专家。 

## 摘要

借助质量修补程序工具，电子商务平台可以通过应用修补程序来增强稳定性和安全性。 这些修补程序可解决问题、提高性能并优化系统。 保持安装处于最新状态可确保针对漏洞提供保护。

在应用修补程序之前，在暂存环境中测试这些修补程序至关重要。 确保与特定版本的Adobe Commerce或Magento Open Source兼容。 某些修补程序可能具有依赖关系，因此请仔细查看先决条件。

 在应用修补程序之前备份您的安装，以防止数据丢失。 如果您进行了自定义代码更改，请注意，修补程序可能会发生冲突。 遵循最佳实践并监控每个修补程序的影响。

## 相关文章和视频

* [Quality Patch Tools search](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=zh-Hans)
* [发行说明](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* 修补程序的[Github](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [使用品质修补工具](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/quality-patches-tool/usage)
* QPT上的[技术视频](https://experienceleague.adobe.com/zh-hans/docs/commerce-learn/tutorials/tools/quality-patch-tool)
