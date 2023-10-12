---
title: 创建产品属性
description: 创建一个返回带有一个参数的json的页面。
kt: 14131
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, User
level: Beginner, Intermediate
exl-id: 98257e62-b23d-4fa9-a0eb-42e045c53195
source-git-commit: 9f3195be50cab2457ac67bd92e9296b7c18c94b4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# 创建产品属性

添加产品属性是中最常用的操作之一 [!DNL Commerce]. 属性是解决许多与产品相关的实际任务的强大方法。 向产品添加下拉列表类型属性的过程非常简单。

在此视频中：

- 添加一个名为clothing_material的属性，可能的值包括：棉花、皮革、丝绸、牛仔布、毛皮和羊毛
- 在产品视图页面上以粗体文本显示此属性
- 将其分配给“默认”属性集并添加限制
- 添加新属性

## 此视频面向谁？

- 刚开始使用Commerce的开发人员，他们需要了解如何以编程方式创建产品属性

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/35789?quality=12&learn=on)

## 代码示例

首先创建所需的文件夹、xml和PHP文件：

- app/code/Learning/ClothingMaterial/registration.php
- app/code/Learning/ClothingMaterial/etc/module.xml
- app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php
- app/code/Learning/ClothingMaterial/Setup/installData.php

### app/code/Learning/ClothingMaterial/registration.php

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Learning_ClothingMaterial',
    __DIR__);
```

### app/code/Learning/ClothingMaterial/etc/module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Learning_ClothingMaterial" setup_version="0.0.1"/>
</config>
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Backend;

use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\Backend\AbstractBackend;
use Magento\Framework\Exception\LocalizedException;

class Material extends AbstractBackend
{
    /**
     * @param Product @object
     * @throws LocalizedException
     */
    public function validate($object)
    {
        $value =$object->getData($this->getAttribute()->getAttributeCode());
        // Be sure to validate that your ID number it is likely to be different

        if (($object->getAttributeSetId() == 9) && ($value == 'fur')) {
            throw new LocalizedException(__('Bottoms cannot be fur'));
        }
    }
}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Frontend;

use Magento\Eav\Model\Entity\Attribute\Frontend\AbstractFrontend;
use Magento\Framework\DataObject;

class Material extends AbstractFrontend
{

    public function getValue(DataObject $object): string
    {
        $value = $object->getData($this->getAttribute()->getAttributeCode());
        return "<b>$value</b>";
    }

}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Source;

use Magento\Eav\Model\Entity\Attribute\Source\AbstractSource;

class Material extends AbstractSource
{
    /**
     * Get all options
     *
     * @retrun array
     */
    public function getAllOptions(): array
    {
       if(!$this->_options){
           $this->_options = [
               ['label'    => __('Cotton'),     'value' => 'cotton'],
               ['label'    => __('Leather'),    'value' => 'leather'],
               ['label'    => __('Silk'),       'value' => 'silk'],
               ['label'    => __('Fur'),        'value' => 'fur'],
               ['label'    => __('Wool'),       'value' => 'wool'],
           ];
       }
       return $this->_options;
    }
}
```

### app/code/Learning/ClothingMaterial/Setup/installData.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Setup;

use Learning\ClothingMaterial\Model\Attribute\Frontend\Material as Frontend;
use Learning\ClothingMaterial\Model\Attribute\Source\Material as Source;
use Learning\ClothingMaterial\Model\Attribute\Backend\Material as Backend;
use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface;
use Magento\Framework\Setup\InstallDataInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class InstallData implements InstallDataInterface
{
    protected $eavSetupFactory;

    public function __construct(
        \Magento\Eav\Setup\EavSetupFactory $eavSetupFactory
    )
    {
        $this->eavSetupFactory = $eavSetupFactory;
    }

    /**
     * @param ModuleDataSetupInterface $setup
     * @param ModuleContextInterface $context
     * {@inheritDoc}
     *
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     * @suppressWarnings(PHPMD.ExessiveMethodLength)
     * @SuppressWarnings(PHPMD.NPathComplexity)
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $eavSetup = $this->eavSetupFactory->create();
        $eavSetup->addAttribute(
            Product::ENTITY,
            'clothing_material',
            [
                'group'         => 'General',
                'type'          => 'varchar',
                'label'         => 'Clothing Material',
                'input'         => 'select',
                'source'        => Source::class,
                'frontend'      => Frontend::class,
                'backend'       => Backend::class,
                'required'      => false,
                'sort_order'    => 50,
                'global'        => ScopedAttributeInterface::SCOPE_GLOBAL,
                'is_used_in_grid'               => false,
                'is_visible_in_grid'            => false,
                'is_filterable_in_grid'         => false,
                'visible'                       => true,
                'is_html_allowed_on_frontend'   => true,
                'visible_on_front'              => true,
            ]
        );
    }
}
```

## 有用的资源

[创建产品属性](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/add-product-attribute.html)

[添加自定义文本字段属性](https://developer.adobe.com/commerce/php/tutorials/admin/custom-text-field-attribute/)
