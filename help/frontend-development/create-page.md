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
TQID: 'https://experienceleague.adobe.com/WtDUQ2sH27ci33UMLBtuNac1oo2CVfBlnFdAbwM3dmY'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: 17d326fa-534a-55a5-b46f-8ae1de1e2f75
    internal-label: Page Content
  - id: 8b440f30-6794-5ed6-981f-391de4e9b0cc
    internal-label: Themes
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: aa64eb87-f03e-49eb-a367-66fa1adc2192
    internal-label: Native Luma Frontend Development
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '106'
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

