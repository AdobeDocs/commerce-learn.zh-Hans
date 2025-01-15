---
title: 使用批量包优化Adobe Commerce全球参考架构
description: 了解如何使用批量包全局参考架构来设置Adobe Commerce，以实现高效的代码管理和版本控制。
kt: 16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="由Adobe高级技术架构师Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰稿"
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# 批量包全局参考架构模式

本指南介绍如何使用批量包全局参考架构(GRA)模式设置Adobe Commerce。

批量包GRA模式涉及一个Git存储库来托管所有常见的自定义项。 此单个Git存储库通过Composer作为单个编辑器包公开，其中包含多个Adobe Commerce模块。

![显示代码在批量包GRA模式中存储位置的图表](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## 此模式的优缺点

优点：

- 通过共享代码存储库重用代码
- 灵活地在不同实例上安装不同的GRA历史版本，从而实现分阶段发布
- 灵活地支持并维护GRA的多个主要版本
- 支持GRA的语义版本控制
- 简单明了，开发人员不需要比常规单商店开发模式更多的技能
- 无需特殊工具、复杂的基础架构或特殊的分支策略
- 发行版中的软件包组合始终一起开发和测试

缺点：

- 只能升级完整的GRA，包括其中包含的所有包。
- GRA批量包中不支持Adobe Commerce模块、语言包和主题以外的编辑器包，因此没有中继包、magento2组件包、Composer插件和修补程序

## 使用拆分Git GRA模式设置Adobe Commerce

### 目录结构

批量包GRA通过编辑器存储库安装所有可重用代码。 特定于单个实例的代码位于该实例的Git存储库中。 特定于实例的代码不会在Adobe Commerce的其他实例中重用。

使用批量包GRA模式进行完整Adobe Commerce安装的最终目录结构如下所示：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

`app/code`、`app/i18n`和`app/design`目录被有意忽略，因为Composer不计算这些目录中的代码。 因此，在包中声明的依赖项不会自动安装。 批量包GRA模式通过在`packages/`中安装一些自定义代码并将该目录视为编辑器存储库来解决此问题。 编辑器将`packages/`中的包符号链接到`vendor/`。

### 准备Git存储库

为共享GRA代码和第一个存储创建两个Git存储库。 从具有以下文件结构的GRA存储库开始：

结果会生成以下目录结构：

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

此目录结构仅提及GraOne和GraTwo模块的composer.json文件和registration.php文件。 实际上，这些模块中有更多文件。

运行以下命令启动Git存储库：

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

composer.json文件内容：

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

将composer.json包的命名空间更改为您自己的命名空间。

src/registration.php文件内容：

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

registration.php文件在Adobe Commerce模块中查找其他registration.php文件并执行它们。

使用<https://github.com/AntonEvers/gra-bulk-foundation>中的代码创建两个示例模块。 Composer不评估示例模块中的composer.json文件。 他们习惯性地在那。 如果您决定将模块移动到其他位置，则composer.json文件将再次变得必需。

### 设置商店存储库

部署存储库包含整个Adobe Commerce安装，包括GRA代码。 创建部署存储库：

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

使用`bin/magento setup:install`安装Adobe Commerce。 使用编辑器在部署存储库中安装GRA示例模块：

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

最后一条命令应产生以下输出，以证明模块已安装并正常工作：

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

您可以创建多个批量包来组织代码。 例如，第三方代码的第三方批量包无法通过Composer获得。 您传统上安装在`app/code`中的所有内容现在都应位于批量包的`src/`目录中。 该规则的例外是仅在单个实例中使用的代码。 这些包称为本地包。

### 安装本地包

部署资料档案库托管本地包。 它们不在GRA批量包中。 本地包的位置不是`app/code`，而是`packages/local`。 指示编辑器将此目录视为存储库：

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

添加在<https://github.com/AntonEvers/module-example-local>上托管的示例模块：

```bash
mkdir -p packages/local
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

最后一条命令应产生以下输出，以证明模块已安装并正常工作：

```bash
Local module is installed successfully and working!
```

将本地模块提交到品牌存储库：

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## 代码位置概述

仅当第三方不通过Composer存储库提供安装时，您才可以将第三方模块存储在Foundation存储库的`src/`目录或专用的第三方批量包中。

- **Adobe Commerce core**：可通过repo.magento.com获取。
- **第三方模块**：通过Marketplace或供应商自己的编辑器存储库提供。
- **第三方模块回退选项**：存储在批量包的`src/`中。
- **GRA Foundation代码**：存储在Foundation批量包的`src/`中。
- **本地代码**：存储在部署存储库的`packages/local`目录中。

## 开发GRA模块

从源安装批量包以在批量包目录中启用Git：

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

已使用Git签出批量包。 当您输入`vendor/antonevers/gra-bulk-foundation`目录时，您也正在输入gra-bulk-foundation Git存储库。 您可以在此目录中创建、签出和合并分支。

将Composer依赖项添加到GRA批量软件包根目录下的composer.json文件，这是Composer评估的批量软件包中的唯一文件。

## 将第三方模块包含到GRA批量包

在GRA基础的根目录下composer.json的require部分中添加第三方包，以将它们添加到GRA中。 这样，始终通过编辑器在所有实例中安装包。

## 交付代码

要将代码传送到主分支，有2个路径。 首先，本地模块，这些模块合并到主分支。 对这些模块运行编辑器更新。 不允许开发人员更新其票证分支中的composer.lock以减少冲突。 仅更新暂存和生产分支中的composer.lock文件，这降低了冲突风险。

其次，GRA批量包，这些包合并到GRA批量存储库的主分支中。 然后，您可以向主分支添加Git标记，以生成编辑器包的版本。 需要在部署存储库的composer.json中新版本的GRA批量包才能安装它。

## 分支策略

只要镜像GRA批量存储库中部署存储库的分支策略，此GRA模式可与所有分支策略配合使用。 对于发行版，在两个存储库中创建具有相同名称的发行版分支。 要进行开发，请在两个存储库中创建票证分支。

在票证分支中，您几乎不必更新composer.lock文件。 只需在开发环境中为存储和GRA Foundation存储库查看正确的分支即可。 例外情况是更新GRA foundation composer.json文件中的要求。 升级部署存储库中的GRA基础仅在构建版本或QA分支时完成。

## 代码示例

本文的代码示例作为一组Git存储库提供，您可以使用这些存储库来测试概念验证。

- 示例生产存储： <https://github.com/AntonEvers/gra-bulk-brand-x>
- GRA代码存储库： <https://github.com/AntonEvers/gra-bulk-foundation>
- 示例本地模块： <https://github.com/AntonEvers/module-example-local>
