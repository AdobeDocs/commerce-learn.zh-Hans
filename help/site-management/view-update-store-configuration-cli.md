---
title: 使用命令行查看和设置管理员配置
description: 了解如何使用命令行查看和设置管理员配置。
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00.000Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
TQID: https://experienceleague.adobe.com/W0kaFd-DJAPA1kFO0hWaBZXsSarojbt5Hnv3QkW85hA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080bid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 168
ht-degree: 0%

---

# 使用命令行查看和设置管理员配置

演示如何使用Commerce CLI查看、设置和查找配置值。 了解值的保存位置以及默认值的来源。

## 此视频面向谁？

* Adobe Commerce开发人员

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3427123?learn=on)

## 本教程中使用的一些命令

将密码安全设置更改为建议设置：

`$ php bin/magento config:set admin/security/password_is_forced 0`

显示销售订单自动复制功能的电子邮件地址

`$ php bin/magento config:show sales_email/order/copy_to`

对于在管理员中具有值的配置，显示空结果

`php bin/magento config:show trans_email/ident_sales/email`

## 本教程中使用的Mysql查询

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## 在何处查找默认销售电子邮件

如何查找在代码库中某处定义的配置值？
`grep -rnw vendor/magento/ -e 'sales@example.com'`

在终端中查看页面并显示行号`cat -n vendor/magento/module-email/etc/config.xml`

## 其他资源

* [命令行工具](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
* [配置管理员安全](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
