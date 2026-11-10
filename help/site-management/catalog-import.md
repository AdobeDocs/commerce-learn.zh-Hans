---
title: Adobe Commerce中的本机目录导入选项
description: 了解如何将目录导入到Adobe Commerce应用商店的一些本机选项。
kt: 13634
doc-type: Tutorial
duration: 211
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
TQID: 'https://experienceleague.adobe.com/-JG7blrxImSXjA2DP9soZfsicISW0hkP2zJeWdMpVBU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 1ca7a530f5f685af907ad1d161ee73e45de6b667
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 0%
---
# 用于导入目录的选项

有一些本机方法将目录导入Adobe Commerce。 每种方法都有其使用推理以及必须考虑的优点和缺点。

从以下任一选项中选择以了解详情。

>[!BEGINTABS]

>[!TAB 手动]

## 手动创建产品 {#manual-import}

考虑为很少更新的小型目录手动创建产品。 输入每个产品都需要一些时间，并且需要接受有关Commerce管理员的基础培训。 手动目录管理适合特定情况而不是大多数商店。 有关说明，请参阅[创建产品](https://experienceleague.adobe.com/en/docs/commerce-admin/catalog/products/product-create){target="_blank"}。 如果结合使用目录管理方法，请限制在引入自动化后进行手动编辑。 自动更新可能会覆盖手动更改并导致混淆。 当集成通过自动化和API管理目录时，通过[用户角色和权限](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/user-accounts/permissions-user-roles){target="_blank"}限制管理员中的目录管理。

### 何时考虑这种方法

* 目录非常小，不到50个产品
* 更新不频繁
* 您拥有所有产品详细信息、图像、视频，并且不希望花费时间学习如何将数据转换为CSV
* 创建产品时要包含添加图像和视频
* 您的团队`not`熟悉API和OAUTH的工作方式

>[!TAB 管理员CSV]

## 管理员CSV导入工具 {#admin-csv}

此工具允许商店所有者使用商务管理员提供的CSV权限导入目录。
[从Commerce管理员导入数据](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/import/data-import){target="_blank"}

优点：
从管理员上传CSV是直接进行目录管理的方法。 它允许对中等大小的目录进行更快的目录产品更新。

缺点：

* 慢速
* 服务器定义了最大上载文件大小，并且存储所有者并不总是能够轻松地对其进行调整。
* 需要管理员访问权限和某人来执行操作，自动化受限
* 计划导入限制为每天1次
* 必须单独上传关联的图像和视频

### 何时考虑这种方法

* 目录大小适中
* 更新每天不超过1次
* 如果需要增加最大上载文件大小，您可以访问服务器配置
* 您的团队`not`熟悉API和OAUTH的工作方式

>[!TAB 批量REST API]

## 批量REST API {#bulk-rest-api}

批量REST API允许自动化和更频繁的更新。 此API比使用CSV的管理员上传速度更快。
[批量端点文档](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints){target="_blank"}

优点：
导入不采用CSV格式的大型数据集的功能。

缺点：

* 必须单独上传关联的图像和视频
* 托管提供商的带宽限制限制了导入吞吐量

### 何时考虑这种方法

* 目录为任意大小
* 更新频繁，一天可以进行1次以上的更新
* 导入时间很重要，但并不重要，处理导入数据的短暂延迟是可以接受的
* 数据不是以CSV格式结构化的，您无法使用自动化对其进行转换

>[!TAB 异步REST API]

## 异步REST API {#async-rest-api}

异步Web端点截取到Web API的消息并将它们写入消息队列。 每次系统接受此类API请求时，都会生成一个UUID标识符。 Adobe Commerce在将消息添加到队列时包含此UUID。 然后，消费者从队列中读取消息并逐个执行这些消息。
[异步Web端点文档](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints){target="_blank"}

优点：

* 快速导入数据
* 支持存储范围，或者您可以指定`all`对所有现有存储执行操作

缺点：

* 不支持GET请求

### 何时考虑这种方法

* 频繁导入
* 从通过API提交这些请求并从消息队列进行处理开始，没有发生很短的延迟问题。


>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

此API导入数据的速度快于其他本机选项。

[导入数据REST CSV API](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
优点：

* 处理传入数据的最快方法
* 可以每天执行多次
* 对于大型请求，可以使用gzip压缩数据，以避免HTTP请求大小限制。

缺点：

* 必须单独上传关联的图像和视频
* 数据需要采用CSV格式

### 何时考虑这种方法

* 目录为任意大小
* 更新频繁，一天可以进行1次以上的更新
* 总体导入时间很重要
* 数据已经是CSV格式，或者可以使用自动化轻松地进行转换

>[!ENDTABS]

## 其他资源

* [使用新的REST CSV导入数据](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
* [导入数据主文档](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/import/data-import){target="_blank"}
* [Adobe Commerce版本2.4.6发行说明](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-6){target="_blank"}
* [用户、角色和权限](../cloud-infrastructure-and-self-hosted/store-administration/users-roles-permissions.md){target="_blank"}
