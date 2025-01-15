---
title: 使用Adobe Commerce优化代码重用
description: 了解如何使用全局参考架构模式优化Adobe Commerce中的代码重用，从而提高多个实例的性能和合规性。
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="由Adobe高级技术架构师Tony Evers提供" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="托尼·埃弗斯撰稿"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# 全球参考体系结构实施技术

有多种方法可使用Adobe Commerce优化代码重用。 这四种实现技术各有其优点。 本文中的示例按照从简单到复杂的顺序排列。 选择最适合您的项目和未来路线图的策略。 从一种策略迁移到另一种策略可能很耗时。

## 何时使用全局参考体系结构

根据您拥有的实例数量，全局参考架构可能很有价值。 实例是使用其自己的数据库独立安装的Adobe Commerce。 计算生产数据库的数量，以了解您拥有的实例数量。 如果您维护多个实例，或者如果您预见将来会出现这种情况，则可以从全局参考体系结构中受益。 实例共享的功能越多，全局参考架构增加的价值就越大。

在以上任何一种场景中，都建议您使用Adobe Commerce的多个实例进行探索。

1. **不同的商店所有者**：如果您为多个商店所有者维护代码，而每个商店所有者都有自己的独特商店，则可能需要单独的实例来有效维护其各自的需求。
2. **符合国家法规**：某些法规要求客户数据必须存储在特定区域内。 在这种情况下，为确保遵守这些条例，必须分开处理。
3. **跨地理区域的操作差异**：在多个区域操作可能意味着不同的维护计划和要求。 使用单独的实例可以灵活高效地管理这些变体。
4. **高强度Flash销售**：进行大规模闪存销售的商店通常需要优化的服务器性能。 由单独的实例提供的专用基础架构可确保在此类高需求期间实现最佳性能。
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

Adobe Commerce核心模块和第三方模块直接通过Composer存储库安装。 Git存储库可用作编辑器存储库。 在此模式中，整个GRA共享代码库托管在单个或几个Git存储库中，并通过Composer进行安装。 关键特征是多个模块、语言包或主题托管在单个编辑器包中，从而简化了开发。

![显示代码在批量包GRA模式中存储位置的图表](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### 单独的包GRA模式

![表示“单独包”GRA模式的图标](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

每个Adobe Commerce模块、语言包或主题都作为单独的编辑器包安装。 每个自定义都有自己的Git存储库。 这意味着实例的组成具有终极的灵活性，并具有可靠的编辑器依赖关系管理。 为了优化性能，所有包都镜像到单个专用编辑器存储库中。

![显示代码以单独的包GRA模式存储的位置的图表](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### 莫纳雷波GRA模式

![表示“monorepo” GRA模式的图标](/help/assets/global-reference-architecture/monorepo.png){align="center"}

所有开发都在单个代码存储库中进行。 自动化会为新版本生成包，并将它们发布到编辑器存储库。 该模式将批量软件包方法的低开发开销与单独软件包方法的灵活性结合在一起。 monorepo模式也非常适合运行自动化功能测试。

![显示代码在monorepo GRA模式中存储位置的图表](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## 选择GRA模式

GRA模式的选择是通过评估项目复杂性、对灵活性的需要以及开发团队的适应能力做出的。

Adobe Commerce经验不足的团队最好从简单开始。 但是，如果项目因其特性而需要更复杂的GRA模式，请不要妥协。

与每种模式相关的常见项目特征：

1. **无GRA模式**：Adobe Commerce的单个实例没有扩展计划。 多个Adobe Commerce实例，它们之间具有极小的通用性。

2. **拆分Git GRA模式**：希望避免为其自定义设置使用Composer的团队，在大多数情况下，批量包模式是拆分Git的首选模式。

3. **批量包GRA模式**：具有高相互依赖性的自定义代码库。 实例都具有非常相似的自定义包组合。 不会频繁地提升或降级各个包。 团队在代码管理方面经验很少，并且需要简单性。

4. **单独的包GRA模式**：需要灵活的发行范围管理。 预计在未来5年内，定制软件包的数量将不超过50个。 公共代码的潜在全局和区域层。 没有迁移到Monorepo模式的计划。 该团队在技术上非常熟练，并且严格遵守流程。

5. **Monorepo GRA模式**：独立包GRA模式的所有特征。 预计在未来5年内，将有50多项方案出台。 需要广泛的自动化测试。 支持临时环境。 企业级的复杂代码库，需要在不以同等速率扩展维护成本的情况下进行扩展。

### 错误选择的成本

可以从一种模式迁移到另一种模式。 下图显示了从一种模式移动到另一种模式时产生的影响级别。 绿色线条显示影响较低，黄色和琥珀色线条显示对于复杂迁移而言较为复杂。

![显示所有4个GRA模式之间的彩色箭头的图表，指示从一种模式移动到另一种模式的困难程度。](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
