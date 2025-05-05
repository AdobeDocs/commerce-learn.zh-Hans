---
title: 了解Adobe Commerce自带的目录导入选项
description: 了解如何将目录导入到Adobe Commerce应用商店的一些本机选项。
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
source-git-commit: b0fe49352b00a68554e662327cd66983c30d8285
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---

# 用于导入目录的选项

有一些本机方法将目录导入Adobe Commerce。 每种方法都有其使用推理以及必须考虑的优点和缺点。

从以下任一选项中选择以了解详情。

>[!BEGINTABS]

>[!TAB 手动]

## 手动创建产品 {#manual-import}

如果您的目录有限并且更新不频繁，手动创建它们可能是最佳选择。 输入每个产品都需要时间，并且有关如何使用Commerce管理员需要进行一些有限的培训。 手动目录管理不是大多数商店的正确选择，但在某些情况下，它可能是合理的。 若要查看有关此过程的其他文档，请访问[创建产品](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html?lang=zh-Hans){target="_blank"}。 请不要忘记，您可以使用多种方法管理您的目录，但是，一旦使用自动化，必须限制手动编辑。 自动更新有机会覆盖手动执行的任何更改，因此会导致混淆。 一旦与Adobe Commerce的集成使用自动化和API来管理目录，建议限制从管理员到[用户角色和权限](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html?lang=zh-Hans){target="_blank"}的目录管理。

### 何时考虑这种方法

- 目录非常小，例如少于50个产品
- 更新不频繁
- 您拥有所有产品详细信息、图像、视频，并且不希望花费时间学习如何将数据转换为CSV
- 创建产品时要包含添加图像和视频
- 您的团队`not`熟悉API和OAUTH的工作方式

>[!TAB 管理员CSV]

## 管理员CSV导入工具 {#admin-csv}

此工具允许商店所有者使用商务管理员提供的CSV权限导入目录。
[从Commerce管理员导入数据](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=zh-Hans){target="_blank"}

优点：
从管理员上传CSV是直接进行目录管理的方法。 它允许对中等大小的目录进行更快的目录产品更新。

缺点：

- 慢速
- 服务器上定义的最大上传文件大小，商店所有者可能无法轻松调整。
- 需要管理员访问权限和某人来执行操作，自动化受限
- 计划导入限制为每天1次
- 必须单独上传关联的图像和视频

### 何时考虑这种方法

- 目录大小适中
- 更新每天不超过1次
- 在必须增加最大文件上传大小的情况下，您可以对服务器配置进行一些访问
- 您的团队`not`熟悉API和OAUTH的工作方式

>[!TAB 批量REST API]

## 批量REST API {#bulk-rest-api}

批量REST API允许自动化和更频繁的更新。 此API比使用CSV的管理员上传速度更快。
[批量端点文档](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

优点：
导入不采用CSV格式的大型数据集的功能。

缺点：

- 必须单独上传关联的图像和视频
- 受托管提供商的带宽限制

### 何时考虑这种方法

- 目录为任意大小
- 更新频繁，一天可以进行1次以上的更新
- 导入时间很重要，但并不重要，处理导入数据的短暂延迟是可以接受的
- 数据不是以CSV格式结构化的，您无法使用自动化对其进行转换

>[!TAB 异步REST API]

## 异步REST API {#async-rest-api}

异步Web端点截取到Web API的消息并将它们写入消息队列。 每次系统接受此类API请求时，都会生成一个UUID标识符。 Adobe Commerce在将消息添加到队列时包含此UUID。 然后，消费者从队列中读取消息并逐个执行这些消息。
[异步Web端点文档](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

优点：

- 快速导入数据
- 支持存储范围，或者您可以指定`all`对所有现有存储执行操作

缺点：

- 不支持GET请求

### 何时考虑这种方法

- 频繁导入
- 从通过API提交这些请求并从消息队列进行处理开始，没有发生很短的延迟问题。


>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

与所有其他本机选项相比，此API选项允许非常快速的导入。

[导入数据REST CSV API](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
优点：

- 处理传入数据的最快方法
- 可以每天执行多次
- 对于大型请求，可以使用gzip压缩数据，以避免HTTP请求大小限制。

缺点：

- 必须单独上传关联的图像和视频
- 数据需要采用CSV格式

### 何时考虑这种方法

- 目录为任意大小
- 更新频繁，一天可以进行1次以上的更新
- 总体导入时间很重要
- 数据已经是CSV格式，或者可以使用自动化轻松地进行转换

>[!ENDTABS]

## 其他资源

- [使用新的REST CSV导入数据](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [导入数据主文档](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=zh-Hans){target="_blank"}
- [Adobe Commerce 2.4.6版发行说明](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html?lang=zh-Hans){target="_blank"}
- [用户、角色和权限](../site-management/users-roles-permissions.md){target="_blank"}
