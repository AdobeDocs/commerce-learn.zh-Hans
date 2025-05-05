---
title: 使用CLI重置管理员URI
description: 了解如何在Adobe Commerce Cloud CLI中重置管理员URI。 当管理员URL更改导致访问问题时，此方法会很方便。
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 123
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 25ee35b730cc6265665a87c9c37d24e88c41b60e
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# 使用cli重置管理员URI

了解如何使用Adobe Commerce Cloud cli命令重置管理员URI。 如果管理员URL从管理员更改为其他管理员，但出现错误，导致您无法访问管理员，则此功能会非常有用。

>[!VIDEO](https://video.tv.adobe.com/v/3439702/?learn=on&captions=chi_hans)

## 本教程中使用的一些命令

将配置更改为预期自定义管理路径URL为0：

`$ php bin/magento config:set admin/url/use_custom 0`

将自定义管理员路径URL的配置更改为0

`$ php bin/magento config:set admin/url/use_custom_path 0`
