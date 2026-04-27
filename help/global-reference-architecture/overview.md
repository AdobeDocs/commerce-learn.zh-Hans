---
title: 使用Adobe Commerce优化代码重用
description: 了解如何使用全局参考架构模式优化Adobe Commerce中的代码重用，从而提高多个实例的性能和合规性。
kt: 15773
doc-type: tutorial
duration: 287
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe高级技术架构师Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰稿"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
TQID: https://experienceleague.adobe.com/1-cE8TS4syjsMuX3VmhQu5zhFX-z3yxV-GlwxVl7eqM
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1127
ht-degree: 0%

---

# 全球参考体系结构实施技术

{{only-for-on-prem-commerce-cloud}}

有多种方法可使用Adobe Commerce优化代码重用。 这四种实现技术各有其优点。 本文中的示例按照从简单到复杂的顺序排列。 选择最适合您的项目和未来路线图的策略。 从一种策略迁移到另一种策略可能很耗时。

## 何时使用全局参考体系结构

根据您拥有的实例数量，全局参考架构可能很有价值。 实例是使用其自己的数据库独立安装的Adobe Commerce。 计算生产数据库的数量，以了解您拥有的实例数量。 如果您维护多个实例，或者如果您预见将来会出现这种情况，则可以从全局参考体系结构中受益。 实例共享的功能越多，全局参考架构增加的价值就越大。

在以上任何一种场景中，都建议您使用Adobe Commerce的多个实例进行探索。

1. **不同的商店所有者**：如果您为多个商店所有者维护代码，而每个商店所有者都有自己的独特商店，则可能需要单独的实例来有效维护其各自的需求。
2. **符合国家法规**：某些法规要求客户数据必须存储在特定区域内。 在这种情况下，为确保遵守这些条例，必须分开处理。
3. **跨地理区域的操作差异**：在多个区域操作可能意味着不同的维护计划和要求。 使用单独的实例可以灵活高效地管理这些变体。
4. **高强度闪存销售**：进行大规模闪存销售的商店通常需要优化的服务器性能。 由单独的实例提供的专用基础架构可确保在此类高需求期间实现最佳性能。
5. **品牌或国家/地区之间的显着差异**：当品牌或国家/地区之间的差异很大时，使用单个实例会导致代码仅用于某些品牌或国家/地区。 单独的实例可以消除不需要代码的品牌和国家/地区不必要的代码，从而提高性能和稳定性。

## 全球参考体系结构模式

在“无GRA模式”旁边，有四种类型的GRA模式。

![5个GRA模式图标：无GRA、拆分、批量、单独和单卷图。](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### 无GRA模式

![描述“无GRA”的图标](/help/assets/global-reference-architecture/no-gra.png){align="center"}

未使用GRA模式时，每个Adobe Commerce实例都是一个唯一的应用程序。 除了通过手动将代码从一个实例移动到另一个实例外，没有代码重用。 这些拷贝总是不同的。 要确保每个实例都具有相同的更改但仍可按预期工作，需要付出的努力可能会变得非常巨大。 在这种情况下，三个实例需要一个实例三倍的维护工作。

![一个图表，其中显示了3个商店，每个商店都是前者的副本，所有3个副本中都有独特的开发。](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### 拆分Git GRA模式

![描述“拆分”GRA模式的图标](/help/assets/global-reference-architecture/split-git.png){align="center"}

此模式包括用于开发的Git存储库和每个实例一个Git存储库。 实例中的每个文件都在一个开发存储库中进行维护。 它们一起组成了整个GRA。 每行代码仅存在于一个开发存储库中，并使用编织技术安装到实例中，从而导致代码重用。

![显示代码以拆分GRA模式存储位置的图表](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### 批量软件包GRA模式

![表示“批量”GRA模式的图标](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Adobe Commerce核心模块和第三方模块直接通过Composer存储库安装。 Git存储库可用作编辑器存储库。 在此模式中，整个GRA共享代码库托管在单个或几个Git存储库中，并通过Composer进行安装。 The key characteristic is that multiple modules, language packs or themes are hosted in a single composer package, simplifying development.

![A diagram showing where code is stored in a bulk packages GRA pattern](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### The separate packages GRA pattern

![An icon representing the &quot;separate packages&quot; GRA pattern](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

Each Adobe Commerce module, language pack or theme is installed as a separate composer package. Each customization has its own Git repository. This means ultimate flexibility in the composition of the instances and has reliable Composer dependency management. For performance optimization, all packages are mirrored in a single private composer repository.

![A diagram showing where code is stored in a separate packages GRA pattern](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### The monorepo GRA pattern

![An icon representing the &quot;monorepo&quot; GRA pattern](/help/assets/global-reference-architecture/monorepo.png){align="center"}

All development takes place in a single code repository. Automation generates packages for new versions and publishes them to a composer repository. The pattern combines the low development overhead of the bulk packages approach with the flexibility of the separate packages approach. The monorepo pattern is also ideal for running automated functional tests.

![A diagram showing where code is stored in a monorepo GRA pattern](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Choosing a GRA pattern

The choice for a GRA pattern is made by assessing the project complexity, the need for flexibility, and the development team&#39;s ability to adapt.

Teams with little Adobe Commerce experience best start simple. However, if the project demands a more complex GRA pattern due to its characteristics, do not compromise.

Common project characteristics related to each pattern:

1. **No GRA pattern**: Single instance of Adobe Commerce without plans to extend. Multiple instances of Adobe Commerce with minimal commonality between them.

2. **Split Git GRA pattern**: Teams that wish to avoid Composer for their customizations, in most cases Bulk packages pattern is a preferred pattern to Split Git.

3. **Bulk package GRA pattern**: Customization codebase with high interdependency. Instances all have very similar combinations of custom packages. No frequent promotion or demotion of individual packages. Teams with little experience in code management and in need of simplicity.

4. **Separate packages GRA pattern**: Flexible release scope management needed. 50 or less custom packages are anticipated in the next 5 years. Potentially, global and regional layers of common code. No plans to migrate to a Monorepo pattern. The team is technically adept and has strict process adherence.

5. **Monorepo GRA pattern**: All characteristics of the Separate packages GRA pattern. More than 50 packages are anticipated in the next 5 years. Need for extensive automated testing. Support for ephemeral environments. Enterprise scale complex codebase that needs to scale without scaling maintenance cost at an equal rate.

### The cost of a wrong choice

Migration from one pattern to another is possible. The diagram below shows the level of impact of moving from one pattern to another. Green lines show low impact, yellow and amber lines show moderately complex to complex migrations.

![A diagram showing colored arrows between all 4 GRA patterns, indicating the level of difficulty of moving from one to the other.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
