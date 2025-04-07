---
title: 检测IP地址
description: 了解如何检测Adobe Commerce云环境的IP地址，以增强安全性并简化服务器通信
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
source-git-commit: a14a878217a145ecee0b29247ec7ccb224edd883
workflow-type: tm+mt
source-wordcount: '1106'
ht-degree: 0%

---


# 了解如何检测Commerce Cloud项目中所有类型环境的IP地址

了解如何在Adobe Commerce云项目中检测不同环境的IP地址。 通过使用一系列命令(包括Adobe Commerce CLI、sed、xargs、dig、grep和sort -u)，用户可以确定用于开发、暂存和生产环境的IP地址。

## 此视频面向谁？

* 开发人员：了解如何为Adobe Commerce Cloud项目收集IP地址。
* 需要限制访问第三方或后端系统的DevOps和安全团队

## 视频内容 {#video-content}

* 了解如何发现Adobe Commerce Cloud中任何环境的IP地址。

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## 用于获取IP地址的命令

请注意，您需要使用项目ID和环境名称，而不是占位符信息。  可能还需要更改`{1..3}`以匹配Adobe Commerce Cloud群集中的节点数，但3个最常见。

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

magento-cloud CLI工具旨在帮助开发人员和系统管理员高效地管理Adobe Commerce Cloud项目和环境。 它扩展了Cloud Console的功能，使用户能够执行日常任务并在本地运行自动化。 主要功能包括管理集成环境、签出和合并环境、列出变量以及使用SSH连接到远程环境。 该工具通过允许直接从本地工作站执行命令而简化了工作流程，增强了整体开发和部署过程。

在示例代码的此初始部分`magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1`中，它正在请求环境的URL。 返回的值类似于此`http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`。 偶尔它看起来更像此`http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`。  第一个命令相当简单，现在是时候进入下一个命令了。

有关详细信息，请阅读[云CLI概述](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## 使用`sed`进行搜索和替换

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

UNIX®Linux®中的命令`sed`表示流编辑器。 它用于对输入流（文件或管道输入）执行基本文本转换。 常见用途包括搜索、查找和替换、插入和删除文本。 命令`sed`逐行处理文本并应用指定的操作，使其成为用于文本操作和脚本编写的强大工具。

如前所述，通常从`magento-cloud` cli返回两种类型的URL。 中间有一个包含`.com.c.c`的变量。 该变体需要处理。 如果检测到此结构，则需要从URL的开头到`.com.c.c`删除所有内容。  然后，剩下的只是URL的最后一部分。 示例URL类似于`http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`。  检测到此模式时，目标是在`.c.`之后保留所有内容。  在提供的此示例代码中，`sed 's/.\.c\.(.)/\1/'`用于获取此部分并忽略原始返回值的其余部分。 URL的其余部分类似于`abcikdxbg789.ent.magento.cloud/`。\
在`sed`中有两个命令正在执行。 它们以分号分隔。 我的`sed`命令`;s/.$//'`的第二部分是删除任何尾随斜杠（如果存在），以清理该URL，使其看起来像`abcikdxbg789.ent.magento.cloud`。  此时，URL已清理完毕，并准备好执行下一个命令。

## 带有dig的Xargs

```bash
xargs -I% dig +short {1..3}."%"
```

UNIX®Linux®中的`xargs`命令用于从标准输入构建和执行命令行。 它接受管道或文件的输入，并将其转换为另一个命令的参数。 它对于处理超出外壳限制的大量参数特别有用。 命令`xargs`可用于执行移动、复制或删除文件等操作。 它通过在单个执行中将多个参数传递给命令来实现高效的批处理。

`dig`命令（Domain Information Groper的简称）是用于查询DNS （域名系统）服务器的网络管理工具。 它有助于检索有关DNS记录的信息，如A、AAAA、MX和CNAME记录。 命令`dig`通常用于解决DNS问题、验证DNS配置以及收集有关域名及其关联IP地址的详细信息。 通过使用各种选项和标记，用户可以自定义输出以显示特定的详细信息或简明摘要。

将`xargs`与`dig`结合使用会使其变得复杂，但这是必需的。 目标是获取清理过的URL并将其保存。  将URL另存为变量后，即会将其插入`dig`命令中。

创建命令`dig`是为了收集DNS信息。 要减少返回的数据量，请使用参数`+short`。 通过将`dig`与`+short`结合使用，可以返回IP地址，有时还会返回字符串。

在该命令部分，`xargs`将保存该URL `abcikdxbg789.ent.magento.cloud`并将其插入到下一个命令`dig`中。 结合迭代保存该URL的技术使使用`dig`命令更加容易。 请记住，我的示例代码是实现目标的一种方式，您可以随意修改内容以满足您的需求和期望。

此时，URL已准备就绪。 接下来，让我们看一下如何检查群集中的每台服务器。 对于Adobe Commerce Cloud，群集中的每个服务器都有一个数字。 服务器标识符是刚刚清理并准备就绪以供使用的URL的前缀。 使用`{1..3}`可以快速轻松地检查服务器。 通过使用`{1..3}`通知`dig`命令执行3次。 下面显示了如果您要实时观察执行的情况。

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

为了更好地说明，`dig`使用的示例URL类似于：

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

如果`{1..3}`修改为`{1..6}`，则它看上去将类似于此：

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## 命令`grep`

有时，字符串会作为来自`dig`的结果的一部分返回。 为此，目标仅是IP地址，并且这些地址由带句点的数字组成。 要确保最终输出只包含数字，可以调整命令。 完成后，最终语法为` grep '^\d'`。  通过添加`'^\d'`，命令`grep`仅保留数字而忽略其他内容。

## 命令`sort`

通过使用`sort -u`，它从IP列表中删除所有重复项。 仅在查看开发级别时才会出现重复。

这些较低级别的环境是多租户的，并与许多其他项目共享基础服务器。 开发环境是单个服务器，而不是群集。 因此，当dig命令在每次迭代中循环时，它会多次返回相同的IP。 因此，通过使用命令`sort -u`，将删除所有重复的IP，并且只保留唯一的IP地址。



## 相关文档

* [地区IP地址](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses|https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
