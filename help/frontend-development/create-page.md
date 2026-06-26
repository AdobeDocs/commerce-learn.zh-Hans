---
title: 创建新页面
description: 了解如何在Adobe Commerce中创建一个新页面，该页面使用一个参数（包括模块设置、routes.xml配置和控制器操作）返回JSON。
jira: KT-5602
doc-type: Technical Video
duration: 259
feature: Page Content, Native Luma Frontend Development, Themes, Configuration
topic: Commerce, Development
role: Developer
level: Beginner
exl-id: aa830d15-0095-450f-83a8-a4ea489d6aae
TQID: https://experienceleague.adobe.com/WtDUQ2sH27ci33UMLBtuNac1oo2CVfBlnFdAbwM3dmY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: fbed285653d7ed0daa8f6ac947c75e4a03647a23
workflow-type: tm+mt
source-wordcount: 106
ht-degree: 0%

---

# 创建新页面

{{only-for-on-prem-commerce-cloud}}

创建一个返回带有一个参数的JSON的页面。

## 此视频面向谁？

* 开发人员

## 添加页面的步骤

* 创建模块
* 添加&#x200B;**routes.xml**&#x200B;文件
* 添加控制器（操作）文件

## 创建模块的步骤

* 创建模块文件夹
* 创建`etc/module.xml`文件
* 创建`registration.php`文件
* 要安装新模块，请运行`bin/magento setup:upgrade`脚本
* 验证模块是否正常工作

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/35816?learn=on)

## 有用的资源

[前端开发人员指南](https://developer.adobe.com/commerce/frontend-core/guide/)

