---
title: 全球参考体系结构Monorepo
description: 了解如何将monorepo方法用于全球参考架构，以建立可扩展且可复原的商务体验
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe高级技术架构师Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰稿"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---

# Monorepo全局参考架构模式

{{only-for-on-prem-commerce-cloud}}

本指南介绍如何使用Monorepo全球参考架构(GRA)模式设置Adobe Commerce。

Monorepo GRA模式涉及一个Git存储库来托管所有常见的自定义项。 此单个Git存储库作为单独的编辑器包通过Composer公开。

![显示代码在monorepo GRA模式中存储位置的图表](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## 此模式的优缺点

优点：

- 非常适合于功能测试
- 通过共享代码存储库重用代码
- 软件包安装完全灵活，每个GRA软件包都可以单独升级、降级或反向移植
- 完全支持语义版本控制
- 无需特殊工具、复杂的基础架构或特殊的分支策略
- 支持编辑器支持的所有包类型
- 非常适用于临时环境，因为这些环境是可选的，但对于大批量交付团队来说，这些环境非常有用

缺点：

- 要部署未使用相同配置开发的软件包组合，需要严格的测试过程
- Monorepo GRA模式一开始可能很复杂。 分配一个可帮助团队处理系统的潜在客户

## 使用单独的包GRA模式设置Adobe Commerce

### 目录结构

使用单独包GRA模式的完整Adobe Commerce安装的最终目录结构具有以下目录结构：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

生产Git存储库具有以下目录结构：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

不同之处在于，生产实例从Composer安装，其中monorepo在包目录中保存每个包的自己副本。

### 准备生产存储库

为第一个Adobe Commerce实例创建存储库，它表示品牌X的Web商店。

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

使用`bin/magento setup:install`安装Adobe Commerce。 提交生成的`app/etc/config.php`和编辑器文件。 Composer可管理任何其他内容，因此Git中不应包含任何其他内容。

### 准备monorepo存储库

monorepo存储库以相同的步骤开始。

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

安装Adobe Commerce和`bin/magento setup:install`，提交并推送。

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### 为monorepo开发做准备

对于monorepo开发，对composer.json文件进行以下更改：

1. 更改包的名称和说明，以便明确此包是您的monorepo包。
1. 从composer.json中删除version属性，因为此版本使用此存储库的Git标记进行管理。
1. 将require部分替换为稍后创建的元包。
1. 将最低稳定性更改为dev。
1. 添加指向`packages/*/*`的路径类型存储库以托管monorepo包，包括它所包含的每个包的分支别名
1. 为项目本身添加分支别名

以下Git差异显示了干净的Adobe Commerce安装与上述更改之间的差异：

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### 使用中继

从GitHub上的[AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)下载示例代码，以获取本示例中使用的中继包和示例模块。

Composer中继资源将多个编辑器资源包捆绑到一个资源包下。 当需要元包时，它捆绑的所有包都会通过编辑器需要元包的部分自动安装。

在本例中，有两个隐含：

1. **antonevers/gra-meta-brand-x**：包含构成“品牌X”的所有内容的中继包
1. **antonevers/gra-meta-foundation**：包含始终安装在任何品牌中的所有内容的中继包

品牌隐喻需要基础隐喻。 当需要品牌隐喻时，基础隐喻也是自动需要的。 请参阅中继的两个文件composer.json以了解它们之间的关系：

antonevers/gra-meta-brand-x：

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antuevers/gra-meta-foundation：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

使用中继包可以很好地组织属于同一部分的代码。 使用中继来定义区域、全球、品牌特定的程序包组或任何适合您的分组。 如果您维护多个Adobe Commerce安装，则可以通过数据包安全通用的方式定义预期使用数据包的上下文。

`packages`目录中的monorepo中存在中继。 在那里，`vendor`的目录结构被镜像：

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### 添加和开发模块

monorepo中的模块存在于`packages`目录中。 这样，Composer便可以通过路径类型存储库找到它们。

从GitHub上的[AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)下载示例代码，以获取本示例中使用的中继包和示例模块。

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

如果需要，`packages`目录中可以有多个命名空间。

开发在packages目录中进行。 运行`packages`后，将在`vendor`目录中创建指向`composer update`目录内包的符号链接。 这样，代码就会成为Adobe Commerce安装的一部分。

运行`bin/magento module:enable --all`或仅针对特定模块启用已添加的模块。

到现在为止，您应该已经安装了有效的Adobe Commerce，并且安装了三个示例模块并且它们正在运行。 您可以通过运行以下命令来验证模块是否已安装以及是否正常工作：

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### 实现自动包创建

有多种选项可以实现自动包创建。 部分选项包括：

1. [私有包程序](https://packagist.com/)
1. [简化Monorepo Builder](https://github.com/symplify/monorepo-builder)
1. 构建您自己的解决方案

[Private Packagist](https://packagist.com/)自动识别Git monorepo中的包，并通过Composer公开它们。 它与Adobe Commerce兼容，具有速度快、维护量低和容易出错等特点，这正是本指南重点介绍私有打包程序选项的原因。

说明如何设置私有打包程序超出了本指南的范围，请参阅[文档](https://packagist.com/docs)。

一旦设置了组织同步并且Git存储库自动同步到专用包，就可以将包转换为Monorepo。

首先，转到packages选项卡并找到monorepo：

![包屏幕中显示monorepo包的Private Packagist屏幕快照](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

单击monorepo包，然后单击详细信息屏幕中的“编辑”，将转到以下页面：

![带有monorepo包编辑页面的Private Packagist屏幕快照](/help/assets/global-reference-architecture/packagist-packages-edit.png)

第一个输入字段下有一个链接，显示：创建多包存储库。 单击此链接。

使用多包配置拍摄了![私有包列表屏幕快照](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

定义可在monorepo中找到编辑器包的位置。 在示例中，位置为`packages/**/composer.json`。 更改位置以限制或扩大Private Packagist搜索要提取的包的位置。

包选项卡将在保存后显示找到的所有包，并且monorepo本身将不再显示为编辑器包：

![私有包管理器屏幕快照，包屏幕中显示了所有monorepo包](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

在Composer中为monorepo内的每个包以及Git中的monorepo上创建的每个标记或分支创建一个版本。

## 将包安装到生产环境

按照专用包程序中的说明将专用包程序添加为编辑器存储库。 专用包管理器可以也应该用作您的所有Composer存储库和Git存储库(包括packagist.org)的镜像。 这样，不必与开发人员共享凭据，并且您可以完全控制每个包。 此示例不遵循此最佳实践，因为它会公开显示Adobe Commerce代码库。

从GitHub下载[GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x)以查看生产商店的示例。

在生产存储中，没有`packages`目录，所有包都通过Composer进行安装。 唯一需要的软件包是：

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

然而，所有Adobe Commerce和整个GRA都是通过此隐喻的要求安装的。

## 版本控制

monorepo中的所有包都会收到与monorepo本身相同的版本。 可以将其视为发布整个应用程序的新版本。 但在生产环境中，您可以安装来自不同monorepo版本的包组合。

## 临时环境

如果您使用短时环境或计划使用它们，monorepo是一个很好的选择。 monorepo的每个版本和分支都包含所有Adobe Commerce、第三方和自定义模块文件。 通过在每个分支中进行完全安装，可以运行包括功能测试在内的每种类型的测试。 使用其他GRA设置（如单独的包或批量包GRA）时，您首先需要构建有效的Adobe Commerce环境，然后才能运行功能测试。 从DevOps的角度来看，monorepo可以简化操作过程。

## 代码示例

本文的代码示例已合并到一组Git存储库中，您可以利用它们进行概念验证。

- 示例monorepo存储库： <https://github.com/AntonEvers/gra-monorepo>
- 示例生产存储： <https://github.com/AntonEvers/gra-monorepo-brand-x>
