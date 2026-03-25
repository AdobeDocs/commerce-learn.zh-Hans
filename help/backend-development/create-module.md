---
title: 创建模块
description: 在Adobe Commerce中创建和注册模块，运行安装程序，并将登录到管理区域、店面和REST API上下文中的PSR记录器的插件。
jira: KT-5614
doc-type: Technical Video
duration: 1113
activity: use
last-substantial-update: 2026-03-23T00:00:00Z
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 1e67193c9b80c929ec391acef771562fb930cc67
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 创建模块

模块是[!DNL Commerce]的结构元素 — 模块构成系统的主干。 通常，通过构建模块来开始自定义。

## 此视频面向谁？

* 后端开发人员

## 添加模块的步骤

1. 创建模块文件夹。
2. 创建`etc/module.xml`文件。
3. 创建`registration.php`文件。
4. 运行`bin/magento setup:upgrade`以注册并安装模块。
5. 检查模块是否正常工作。

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml文件

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

### registration.php文件

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

### 添加插件

接下来，向基本模块中添加功能。 在Adobe Commerce开发中，您可以使用插件作为基本工具。 本视频和教程向您说明如何创建插件。

>[!VIDEO](https://video.tv.adobe.com/v/3420255?learn=on)

### 对于插件，请记住以下事项

* 您在`di.xml`中声明所有插件。
* 您可以为每个插件指定一个唯一的名称。
* 您可以选择设置`disabled`和`sortOrder`属性。
* 通过选择包含`di.xml`文件的文件夹来设置插件范围。
* 您可以在Target方法调用之前、之后或周围运行插件。
* 避免`around`插件。 它们会引诱您，但它们通常代表错误的选择，并导致性能问题。

### 插件代码示例

本教程使用以下XML和PHP类向第一个模块添加插件。

### app/code/Training/Sales/etc/adminhtml/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A Plugin that executes when the admin user places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="admin-training-sales-add-logging" type="Training\Sales\Plugin\AdminAddLoggingAfterOrderPlacePlugin" disabled="false" sortOrder="0"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/frontend/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when a customer uses the LoginPost controller from the Luma frontend -->
    <type name="Magento\Customer\Controller\Account\LoginPost">
        <plugin name="training-customer-loginpost-plugin"
                type="Training\Sales\Plugin\CustomerLoginPostPlugin" sortOrder="20"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/webapi_rest/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when the REST API is used OR when the Luma frontend places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="rest-training-sales-add-logging" type="Training\Sales\Plugin\RestAddLoggingAfterOrderPlacePlugin"/>
    </type>
</config>
```

### app/code/Training/Sales/Plugin/AdminAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class AdminAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this admin area order
        $this->logger->notice('An ADMIN User placed an Order - $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

### app/code/Training/Sales/Plugin/CustomerLoginPostPlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Psr\Log\LoggerInterface;
use Magento\Framework\App\RequestInterface;

/**
 * Introduces Context information for ActionInterface of Customer Action
 */
class CustomerLoginPostPlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @var RequestInterface
     */
    private RequestInterface $request;

    /**
     * @param LoggerInterface $logger
     * @param RequestInterface $request
     */
    public function __construct(LoggerInterface $logger, RequestInterface $request)
    {
        $this->logger = $logger;
        $this->request = $request;
    }

    /**
     * Simple example of a before Plugin on a public method in a Controller
     */
    public function beforeExecute()
    {
        $login = $this->request->getParam('login');
        $username = $login['username'];
        $this->logger->notice( "Login Post controller was used for " . $username );
    }
}
```

### app/code/Training/Sales/Plugin/RestAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class RestAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this customer driven order
        $this->logger->notice('A Customer placed an Order for $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

## 有用的资源

* [模块参考指南](https://developer.adobe.com/commerce/php/module-reference/){target="_blank"}
* [插件](https://developer.adobe.com/commerce/php/development/components/plugins/){target="_blank"}
