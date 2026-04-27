---
title: 使用CLI重置管理员URI
description: 了解如何在Adobe Commerce Cloud CLI中重置管理员URI。 当管理员URL更改导致访问问题时，此方法会很方便。
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00.000Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
TQID: https://experienceleague.adobe.com/OgyaTHVhQSRFApJPYwkzodSyAJcBjwk8kIrw4VBXltQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 107
ht-degree: 0%

---

# 使用cli重置管理员URI

了解如何使用Adobe Commerce Cloud cli命令重置管理员URI。 如果管理员URL从管理员更改为其他管理员，但出现错误，导致您无法访问管理员，则此功能会非常有用。

>[!VIDEO](https://video.tv.adobe.com/v/3435066?learn=on)

## 本教程中使用的一些命令

将配置更改为预期自定义管理路径URL为0：

`$ php bin/magento config:set admin/url/use_custom 0`

将自定义管理员路径URL的配置更改为0

`$ php bin/magento config:set admin/url/use_custom_path 0`
