---
title: 单独的软件包全球参考体系结构
description: 使用单独的包GRA优化Adobe Commerce。 了解灵活版本化包管理的设置、好处和最佳实践。
kt: 16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
source-git-commit: 916586d3b71b8b74baa04af2ab39abc86ec94f5b
workflow-type: tm+mt
source-wordcount: '2088'
ht-degree: 0%

---


# 单独的软件包全局参考体系结构模式

本指南介绍如何使用单独包全局参考架构(GRA)模式设置Adobe Commerce。

单独的包GRA模式涉及每个公共包的一个Git存储库和每个Adobe Commerce实例的一个Git存储库。 通用包通过具有专用编辑器存储库的Composer公开。

此全局参考架构模式完全基于Composer，旨在从所有Composer功能中获取最大利益。

![显示代码以单独的包GRA模式存储位置的图表](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## 此模式的优缺点

优点：

- 通过共享代码存储库重用代码
- 软件包安装完全灵活，每个GRA软件包都可以单独升级、降级或反向移植
- 完全支持语义版本控制
- 无需特殊工具、复杂的基础架构或特殊的分支策略
- 支持编辑器支持的所有包类型

缺点：

- 在这种GRA模式中进行开发在开始时难度稍大一些，但有一段较短的学习曲线
- 要部署未使用相同配置开发的软件包组合，需要严格的测试过程

## 使用单独的包GRA模式设置Adobe Commerce

### 目录结构

使用单独包GRA模式的完整Adobe Commerce安装的最终目录结构如下所示：

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

`app/code`、`app/i18n`和`app/design`目录被有意忽略。 单独的包GRA会安装来自Composer的每个包。 即使软件包仅安装在单个Adobe Commerce实例中。

### 准备商店存储库

为第一个Adobe Commerce实例创建存储库，它表示品牌X的Web商店。

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

使用`bin/magento setup:install`安装Adobe Commerce。 提交生成的`app/etc/config.php`。

### 创建程序包资料档案库

此全局参考架构模式中的每个包都有自己的Git存储库。 以下是示例包，其中包含表示GRA模块、第三方模块和本地模块的Adobe Commerce模块。

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

使用这些示例创建您自己的包。

### 创建metapackage存储库

元包控制此GRA模式中GRA公共代码库的范围。 它们定义基础中的内容：始终一起安装的包版本的组合。 示例：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

上面的代码片段是一个隐喻的composer.json。 因为元包只包含composer.json文件，不包含其他代码。 上面的代码也是完整的隐喻。 将其托管在Git存储库中，您便拥有了一个可安装的元包编辑器存储库。 它需要一个示例GRA模块、第三方模块以及Adobe Commerce核心。 它还需要gra-component-foundation ，这将在下一章中介绍。

使用中间包可以捆绑包，而无需在包之间创建依赖关系。 因此，即使包之间不存在技术依赖关系，通过中继包，也可以使它们安装在一起。 如果您在项目中需要此中间包，则会安装该中间包所需的任何包或中间包。 因此，如果您创建一个空白编辑器项目，而您只需要此包，则编辑器将安装Adobe Commerce以及GRA和第三方模块。

这样，您可以确保每个存储都包含相同的一组基础软件包。

同样，您可以定义定义存储x的中继标记。它需要基础暗包，需要完整的GRA基础和一个本地模块：

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Brand-X中继是可选的。 您还可以跳过品牌隐含，并直接在商店编辑器项目中要求这些依赖项。 为本地模块创建元包的好处是，在商店Git存储库中，只有包存储库中没有任何功能分支和功能提取请求。 这是一项安全措施。 此外，您可以选择在包存储库中应用语义版本控制，并在主项目中使用不同的Git标记，例如，跟踪命名的版本。 由你来决定。

### 供应商目录之外的GRA Foundation文件

有时您需要将文件存储在供应商目录之外。 如`.gitignore`，位于`dev/`目录中的文件或域验证文件。 magento2-component软件包类型就是为此目的而设计的。 查看<https://github.com/AntonEvers/gra-component-foundation>。

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

此软件包的类型为magento2-component，它包含一个src目录，该目录托管复制到Adobe Commerce根目录中的文件。 此文件中的映射将`/src/gitignore`复制到主编辑器项目中的`/.gitignore`。

这样，您就可以使供应商目录之外的文件也成为GRA基础的一部分。

### 开发GRA基础模块

开发在供应商目录中进行。 要求编辑器从源安装基础包。 这样做会从Git中签出包，而不是从下载的存档中安装包。

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

通过此命令，已使用Git签出安东尼命名空间中的包。 当您输入vendor/antonevers/module-gra目录时，也会输入module-gra Git存储库。 您现在可以直接从供应商目录创建、签出和合并分支，并以这种方式进行开发。

### 将第三方模块纳入GRA基础

将第三方包添加到GRA中继。 如果无法从编辑器存储库安装第三方代码，请为其创建包。 创建Git存储库，添加包内容（应用程序/代码/供应商/包中的所有内容），并确保在存储库的根目录下存在有效的composer.json文件。 您现在可以通过Composer安装此包。

## 设置专用编辑器存储库

在全球参考架构中，私有存储库不是强制性的。 它可加快部署和安装速度，减少composer.json中的存储库配置，并提高安全性。 其他编辑器存储库和Adobe Commerce市场的凭据存储在您的专用存储库中。 没有与代码捆绑在一起的或开发人员计算机上捆绑的更加敏感的凭据。

此外，某些专用存储库还提供其他功能，例如，当您的某个存储库的某个依赖项中包含安全漏洞时，会发送电子邮件通知。

当您在composer.json中有多个VCS存储库时，会出现速度缓慢问题。 在执行升级时，需要读取每个编辑器存储库，并且对于50个程序包具有50个存储库的单个编辑器存储库的开销至少是单个编辑器存储库的开销的50倍。

![显示缺少编辑器存储库时速度变慢位置的图表](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

包括私有编辑器存储库形式的编辑器镜像。 镜像包含来自其他编辑器存储库的所有包以及所有Git托管包的副本。 使用专用编辑器存储库，您还可以获得细粒度访问控制。

通过Git同步，专用编辑器存储库会自动检测Git存储库中的新包以及现有包的新版本。

您可以使用Satis托管自己的专用存储库： <https://composer.github.io/satis/>。 在<https://antonevers.github.io/gra-composer-repository/>处查看公共存储库示例。 此存储库在代码示例中用作编辑器存储库。 需要执行其他措施来将Satis存储库设为私有。

有一些解决方案可供您配置并忽略：专用打包程序<https://packagist.com/>，它由编写Composer或JFrog Artifactory <https://jfrog.com/artifactory/>的同一人创建。

## 投放代码

利用中继包，可通过以下3个步骤来交付代码。

1. 将更改合并到包中，并对更改的包进行版本控制。
2. （可选，仅当添加了新包时才可用）要求将新包添加到元包中，并对元包进行版本控制。
3. （可选，仅当添加了新包时才能使用）在Adobe Commerce中需要新元包并进行部署。

使用包版本控制部署范围。 创建包的稳定版本意味着此包已准备好进行生产部署。

要构建新版本，请在主编辑器项目中运行编辑器更新，其中包含完整商店安装。 已安装所有最新版本的软件包。

## 版本控制

在单独的包GRA中进行版本控制是Git中标记模块的同义词。 Git标记会创建Composer安装的包的编号版本。
正确的版本控制方法可让您的包自动流动，同时保持安全。

两个示例：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

此示例显示了依赖项的严格定义。 精确版本中需要3个包。 安装中使用此中继包进行编辑器更新不执行任何操作。 它始终在这些精确版本中安装这3个包，即使有较新版本可用也是如此。

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

此示例说明依赖项的松散定义。 使用`~1.0`可以安装这些包的任何版本（如果它们大于或等于`1.0.0`且小于`2.0.0`），并首选最高可用版本。 您可以在<https://getcomposer.org/doc/articles/versions.md>了解有关如何定义版本依赖关系的更多信息：

> ~运算符的最佳解释是： `~1.2`等同于`>=1.2 <2.0.0`，而`~1.2.3`等同于`>=1.2.3 <1.3.0`。

只要您发布了上述任何软件包的新版本，就会自动随Composer更新一起安装。

应用语义版本控制。 您可以在<https://semver.org/>上了解有关语义版本控制的所有信息。 特别是，常见问题解答是必须阅读的。 通过语义版本控制，“1.0.0”中的数字称为MAJOR.MINOR.PATCH。 应该可以安全地引入包的次要版本和修补程序版本，而不会破坏应用程序。
您可以自动包括修补程序并手动选择次要升级。 请注意，通过手动选择每个次要更改，这样做会增加额外的开销：

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

当然，所有这些只有在您始终一致地应用语义版本控制时才有效。 不仅在中间包中，常规包的要求也应该宽松地定义依赖关系。 如果您的系统中具有一个严格依赖关系，则该包将受限于严格的定义。

通过键入composer depends \&lt;包名称\>查找这些严格的依赖项。 有关详细信息，请参阅<https://getcomposer.org/doc/03-cli.md#depends-why>。

## 分支策略

您可以使用各种分支策略来支持此全局引用策略模式，前提是主分支是您对包进行版本控制的唯一分支。 如果跨多个分支进行版本，则会引入在版本之间随机丢失功能的风险。 仅在主分支上创建稳定版本。

仅在程序包存储库中创建功能分支。 不在应用商店安装存储库中。 只需使用Composer，即可继续为商店引入任何更改。 避免Git必须在部署存储库中合并。

分支策略中常见的分支类型以及应存在于中的存储库：

**功能分支**：位于包存储库中，任何其他位置都存在。

**版本分支**：在任意存储库（包、中包、存储安装存储库）中创建。 在计划版本时，在包版本分支中对更改进行版本控制之前，先对它们进行分组。 假设您正在准备代码名称为“Unicorn”的版本。 您可以在包含更改的包中创建release-unicorn分支。 将其中的任何内容合并，然后在您的中继包中需要“dev-release-unicorn as 1.4.0”。 了解有关别名的更多信息，以了解那里发生的情况： <https://getcomposer.org/doc/articles/aliases.md>。

**QA/开发分支**：类似于发布分支。

**主分支**：必须存在于每个存储库中，并且应始终是代表生产或生产就绪状态的分支。 主要分支是标记发行版本代码的位置。
请确保您选择的分支策略和维护开销很小。 例如，在修补程序版本之后将主分支合并回QA、UAT、版本或开发分支是一项开销维护任务。 程序包越多，存储库越多，重复开销任务也越多。

使用mixu/gr等工具对批次中的多个Git存储库执行例行操作： <https://github.com/mixu/gr>

## 命名约定

使用单独的包GRA模式时，如果基础元包需要，则包是GRA基础的一部分。 在元包中添加或删除包以将其移入和移出基础。

利用中继，可以灵活地确定软件包的安装范围。 软件包的名称中不包含与软件包的预期用途相关的任何词语，这一点格外重要。 当您决定将该包从GRA基础中取出时，名称antonevers/module-gra-store-locator可能会变得混乱。 避免范围(GRA、foundation、local)。 避免地区（欧洲、中东和非洲、西班牙、全球）。 最好避免使用为生成包的商店的名称。 选择仅与包中添加的功能相关的名称。 这样，您就可以在任何需要的地方（在不可预见的未来情形中）重复使用它们。 名称antonevers/module-store-locator将非常好。

确保相关包一起显示在概述中。 将名称从通用内部版本转换为特定内部版本。 因此，使用antonevers/module-b2b-tax-exempt代替antonevers/tax-exempt-module-b2b。

## 代码示例

此博客帖子的代码示例已合并到一组Git存储库中，您可以使用这些存储库来玩概念验证。

- 示例生产存储： <https://github.com/AntonEvers/gra-separate-brand-x>
- 基础模块示例： <https://github.com/AntonEvers/module-example-gra>
- 第三方模块示例： <https://github.com/AntonEvers/module-example-3rdparty>
- 示例本地模块： <https://github.com/AntonEvers/module-example-local>
- 基础元包示例： <https://github.com/AntonEvers/gra-meta-foundation>
- 示例本地中继（可选）： <https://github.com/AntonEvers/gra-meta-brand-x>
- 示例编辑器存储库： <https://github.com/AntonEvers/gra-composer-repository>