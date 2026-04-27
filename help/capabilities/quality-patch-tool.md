---
title: Quality修补工具
description: 了解在诊断问题、找到解决方案并应用现有可用修补程序列表中找到的修补程序时，如何使用质量修补工具。
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00.000Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
TQID: https://experienceleague.adobe.com/GpcJqSCn3XqLZtm-QdQ-ka9c-RdkG-C6Hd3FpXrh8-I
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
  - id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 593
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
* 测试：始终先在暂存环境中测试修补程序，然后再将其应用于生产。 Unexpected issues can arise.
* Patch Dependencies: Some patches may depend on others. Be aware of any prerequisites.
* Customizations: If you&#39;ve made custom code changes, patches might conflict. Review the changes carefully.
* Back up: Back up your installation before applying patches to avoid data loss.

While the Quality Patches Tool is useful for applying a limited number of patches, it&#39;s not recommended for handling a large volume of patches. Applying too many patches can complicate future upgrades and maintenance. If you have numerous patches to apply, consider alternative approaches or consult with a Magento specialist. 

## 摘要

The Quality Patches Tool allows e-commerce platforms to enhance stability and security by applying patches. These patches address issues, improve performance, and optimize the system. Keeping your installation up-to-date ensures protection against vulnerabilities.

Before applying patches, it&#39;s crucial to test them in a staging environment. Ensure compatibility with your specific version of Adobe Commerce or Magento Open Source. Some patches may have dependencies, so review the prerequisites carefully.

 Back up your installation before applying patches to prevent data loss. If you&#39;ve made custom code changes, be aware that patches might conflict. Follow best practices and monitor the impact of each patch.

## Related articles and videos

* [Quality Patch Tools search](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=zh-Hans)
* [发行说明](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github for patches](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Usage of quality patch tool](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/quality-patches-tool/usage)
* [有关QPT的技术视频](https://experienceleague.adobe.com/zh-hans/docs/commerce-learn/tutorials/tools/quality-patch-tool)
