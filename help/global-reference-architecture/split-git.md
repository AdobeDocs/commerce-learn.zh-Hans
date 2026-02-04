---
title: 使用拆分Git全局参考架构设置Adobe Commerce
description: 了解如何使用拆分Git全局参考架构来设置Adobe Commerce，以实现高效的代码管理和简化的部署。​AEM
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe高级技术架构师Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰稿"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# 拆分Git全局参考架构模式

{{only-for-on-prem-commerce-cloud}}

本指南介绍如何使用拆分Git全局参考架构(GRA)模式设置Adobe Commerce。

拆分Git GRA模式涉及两个用于开发的Git存储库和每个Adobe Commerce实例的一个Git存储库。 在这些示例中，假定每个实例代表一个独特品牌。

![显示代码以拆分GRA模式存储位置的图表](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## 此模式的优缺点

优点：

- 通过共享代码存储库重用代码
- 简单的GRA模式，甚至适用于具有有限编辑器知识的团队
- 除了Adobe Commerce模块、主题和语言包之外，还可以通过此模型安装任何类型的编辑器包，包括composer-plugin、composer-metapackage、magento2-component和修补程序
- 可以分阶段发布，计划发布到各自维护窗口中的区域
- 支持Git标记用于管理目的，而非部署控制
- 确保在此精确的配置中开发和测试生产部署中的包组合

缺点：

- 与其他GRA模式相比，没有增加灵活性
- 无法升级或降级每个实例的单个模块，请始终升级或降级GRA整体
- 在大多数情况下，批量包模式会更适合，因为它同样简单，但更传统

## 使用拆分Git GRA模式设置Adobe Commerce

### 目录结构

拆分Git GRA模式有两种类型的存储库：开发存储库和安装存储库。 开发存储库仅包含完整Adobe Commerce安装的一部分。 安装存储库包含完整的Adobe Commerce安装，用于部署，但不用于开发。

使用拆分Git GRA模式进行完整Adobe Commerce安装的最终目录结构如下所示：

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

`app/code`、`app/i18n`和`app/design`目录被有意忽略，因为Composer不计算这些目录中的代码。 因此，在包中声明的依赖项不会自动安装。 拆分Git GRA模式通过在`packages/`中安装所有自定义代码并将该目录视为编辑器存储库而解决了此问题。 编辑器将`packages/`中的包符号链接到`vendor/`。

### 准备Git存储库

为以下用户创建3个Git存储库：

1. Adobe Commerce实例
2. 未通过编辑器安装的第三方代码
3. 模块、主题、语言包等形式的自定义项；您的GRA

在本指南中，以下名称用于这些存储库：

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

拆分Git GRA模式中的所有存储库将合并为一个存储库。 为了使Git允许合并多个存储库，所有三个存储库都需要共享历史记录。 创建一个具有单个提交的Git项目，并将其推送到所有远程。

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

将临时文件`.gitkeep`推送到所有远程位置时，会使用相同的提交哈希创建相同的初始提交，从而生成共享历史记录。 在一个远程数据库中创建的每个更改都可以合并到其他更改。

从这里，存储库存在差异。 gra-split-brand-x存储库包含特定于品牌的代码。 gra-split-3rdparty存储库仅包含第三方代码。 gra-split-gra存储库仅包含您的全局引用架构基础，该基础包含您的所有自定义代码。

在gra-split-brand-x存储库中安装Adobe Commerce。

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

在gra-split-3rdparty和gra-split-gra存储库中创建初始提交。 最简单的方法是在单独的目录中签出这些存储库。

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

这两个存储库存储第三方软件包和GRA软件包。 可能存在每个Adobe Commerce实例专属的代码。 创建一个位置，将这些本地包存储在gra-split-brand-x存储库中。

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### 存储不同类型代码的位置

Adobe Commerce是一个编辑器应用程序。 首选的安装方式始终通过Composer存储库。 仅当模块供应商不通过Composer存储库提供安装服务时，您才可以将第三方模块存储在第三方存储库中。 自定义代码的首选位置位于GRA存储库中。 当某个模块仅由一个特定实例使用时，它将变为本地代码。

摘要：

- **Adobe Commerce**：存储在编辑器存储库中。
- **第三方模块**：存储在编辑器存储库中。
- **第三方模块回退选项**：存储在gra-split-3rdparty Git存储库中。
- **GRA Foundation代码**：存储在gra-split-gra Git存储库中。
- **本地代码**：存储在gra-split-brand-x Git存储库中。

### 将包存储连接到编辑器

Composer可以将packages目录视为Composer存储库。 通知编辑器包在包目录中的位置。

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer在三个存储目录中查找两个级别的composer.json文件。 在三个代码存储目录中创建子目录，它们与`vendor/`目录中显示的完全相同。

例如：如果包通常安装在`vendor/example-corp/module-example/`中，则将其存储在`packages/3rdparty/example-corp/module-example/`中。 需要时，编辑器会将包符号链接到`vendor/example-corp/module-example/`。

使用编辑器包的命名空间和名称作为目录结构。 例如：传统上存在于`app/code/MyCorp/MyCustomization/`中的模块在composer.json中具有名称`my-corp/module-my-customization`。 将此包存储在`packages/gra/my-corp/module-my-customization`中。

### 在实例存储库中包含新包

将第三方和GRA中的包远程合并到gra-split-brand-x存储库中。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

结果会生成以下目录结构：

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

第三方和GRA Foundation存储库中的更改将合并到品牌存储库中。 这样，第三方和GRA代码仅在一个位置进行维护。 通过Git合并将更改移动到品牌。

Adobe Commerce不会自动识别新模块。 运行编辑器要求在合并后添加新包。 每次在合并后更新某个包时，运行编辑器更新。

### 安装示例模块

作为概念验证，请安装示例模块以了解GRA模式的工作原理。

运行`composer install`和`bin/magento install`，然后再继续。

GitHub上有3个测试模块：

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### 安装本地模块

将模块添加到本地代码池非常简单。 下载并解压该模块。 Composer需要它。 使用`bin/magento`启用它并在品牌存储库中提交文件。

```bash
cd gra-split-brand-x
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

如果您看到上述输出，则可以将其安全地提交到品牌存储库。

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### 安装和开发GRA基础模块

将模块添加到GRA存储库与安装本地模块不同。 默认情况下，提交将添加到origin/main，即gra-split-brand-x存储库。 对GRA模块所做的更改应推送到gra-split-gra存储库，并随后合并到gra-split-brand-x存储库。

### 创建开发环境

创建一个将所有代码池组合到一个位置的开发环境。 您可以通过符号链接将代码分别推送到本地、GRA和第三方存储库。 首先，在您的品牌、GRA和第三方存储库目录旁边创建一个新的开发目录。

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

结果会生成以下目录结构：

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

在gra-development目录中运行`composer install`和`bin/magento install`。

现在可以直接从`packages/3rdparty`、`packages/gra`和`package/local`目录提交更改。 Git将更改提交到目录符号链接到的Git存储库。 在`packages/3rdparty`、`packages/gra`或`package/local`目录中发出git commit命令很重要。 不要在项目根目录下运行git提交。

### 安装示例模块

在包目录中安装第三方和GRA示例模块。

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

最后一条命令应产生以下输出，以证明模块已安装并正常工作：

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

如果您看到上述输出，则可以将其安全地提交到品牌存储库。 运行`git remote -v`以验证您是否正在提交到正确的远程数据库。

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### 将代码交付给实例

将GRA和第三方存储库合并到gra-split-brand-x存储库，以将代码交付到Adobe Commerce实例。 运行`composer require`、`bin/magento module:enable`并提交结果。

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## 分支策略

如果您在第三方和GRA存储库中镜像商店存储库的分支策略，则此GRA模式可与所有分支策略配合使用。 对于发行版，请在所有三个存储库中创建具有相同名称的发行版分支。 在准备发布期间，将发布分支合并到存储存储库中。

有时，您有一个票证分支，它要求更改本地代码和第三方代码或GRA代码。 在这种情况下，需要在所有相关存储库中创建票证分支。

切勿将第三方和GRA承诺合并到票证分支内的品牌存储库中。 相反，请检查开发环境中每个代码池的正确分支。 只有在构成版本或构成QA分支时，才能合并到品牌存储库中。

## 代码示例

本文的代码示例作为一组Git存储库提供，您可以使用这些存储库来测试概念验证。

- 示例生产存储： <https://github.com/AntonEvers/gra-split-brand-x>
- 第三方代码存储库： <https://github.com/AntonEvers/gra-split-3rdparty>
- GRA代码存储库： <https://github.com/AntonEvers/gra-split-gra>
- 示例本地模块： <https://github.com/AntonEvers/module-example-local>
- 示例GRA模块： <https://github.com/AntonEvers/module-example-gra>
- 第三方模块示例： <https://github.com/AntonEvers/module-example-3rdparty>
