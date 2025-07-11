---
title: 使用Fastly拒绝访问整个网站
description: 使用Fastly Edge ACL和自定义VCL限制Adobe Commerce站点访问
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 200
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
source-git-commit: 810d1a17e9fe564e8450b091bbeb5574d7d76075
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# 使用Fastly拒绝对整个网站的访问

了解如何使用Fastly Edge ACL和自定义VCL代码片段限制对Adobe Commerce Cloud网站的访问。 此分步指南仅允许特定IP地址，从而帮助您保护启动前环境，确保您的开发站点保持私有状态。

## 您将学习的内容

使用Fastly Edge ACL和自定义VCL限制Adobe Commerce站点访问 | 安全的启动前环境

## 此视频面向谁？

* DevOps工程师
* Adobe Commerce开发人员
* 站点可靠性工程师

>[!VIDEO](https://video.tv.adobe.com/v/3464779/?learn=on&enablevpops)

## 代码示例

这是使用的VCL的示例

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## 相关文档

* [正在检测恶意IP地址](https://experienceleague.adobe.com/zh-hans/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [用于允许请求的自定义VCL](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [用于阻止请求的自定义VCL](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)