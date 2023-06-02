---
title: 创建模块
description: 创建一个返回带有一个参数的json的页面。
topic: Development
kt: 5602
doc-type: video
activity: use
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 32d1366758fa6453a48570cfd08d10a93559a974
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 创建模块

模块是结构元素 [!DNL Commerce]  — 整个系统基于模块构建。 通常，创建自定义项的第一步是构建模块。

## 此视频面向谁？

- 开发人员

## 添加模块的步骤

- 创建模块文件夹。
- 创建etc/module.xml文件。
- 创建registration.php文件。
- 运行bin/magento设置。
- 升级脚本以安装新模块。
- 检查模块是否正常工作。

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## 有用的资源

- [模块参考指南](https://developer.adobe.com/commerce/php/module-reference/)
