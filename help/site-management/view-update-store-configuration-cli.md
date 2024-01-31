---
title: 了解如何使用命令行查看和设置管理员配置
description: 演示如何使用命令行查看和设置配置值
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
source-git-commit: 9d4b01d383e5492ccc0cbd27636a49581e8ffb5b
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# 了解如何使用命令行查看和设置管理员配置

演示如何使用Commerce CLI查看、设置和查找配置值。 了解值的保存位置以及默认值的来源。

## 此视频面向谁？

- Adobe Commerce开发人员

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

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

在终端中查看页面并显示行号 `cat -n vendor/magento/module-email/etc/config.xml`

## 其他资源

- [命令行工具](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [配置管理员安全](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
