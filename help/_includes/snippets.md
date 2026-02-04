---
title: 版本横幅
description: 重用了可视元素来记录应用于特定版本的功能或页面
source-git-commit: 180bfd303df77d95c88fa518253ddff6a67c76d4
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# 版本横幅

## EE专用功能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce功能" src="../assets/adobe-logo.svg" width="20" height="20" /> 仅在Adobe Commerce中独占的功能（<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">了解更多</a>）</td></tr>
</table>

## 仅限B2B的功能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce功能" src="../assets/b2b.svg" width="20" height="20" /> 仅对Adobe Commerce<a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html">的</a>B2B提供独占功能</td></tr>
</table>

## 400问题 {#avoid-400-error}

>[!CAUTION]
>
>在执行API调用时，请确保使用某种类型的searchCriteria。 您也可以考虑使用分页。 如果Adobe Commerce返回的结果过大，则可能会满足Adobe Developer App Builder容量，并导致文件意外结束。 结果是400错误的响应结果。\
> 例如，假设需要从Adobe Commerce请求所有当前产品。 生成的URL类似于`{{base_url}}rest/V1/products?searchCriteria=all`。 根据返回的目录的大小，json可能太大，App Builder无法使用。 改为使用分页并进行一些请求以避免`Response is not valid 'message/http'.`

## 仅限PaaS和Commerce Cloud {#only-for-on-prem-commerce-cloud}


**此信息适用于：**

| 内部部署 | Adobe Commerce Cloud | Adobe Commerce as a Cloud Service | Adobe Commerce Optimizer |
| --- | --- | --- | --- |
| 是 | 是 | 否 | 否 |
