---
title: 版本横幅
description: 重复使用了可视元素来注释功能或应用于特定版本的页面
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# 版本横幅

## 仅EE功能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce功能" src="../assets/adobe-logo.svg" width="20" height="20" /> 仅在Adobe Commerce提供独家功能(<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">了解更多</a>)</td></tr>
</table>

## 仅B2B功能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce功能" src="../assets/b2b.svg" width="20" height="20" /> 独家功能仅在 <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">B2B for Adobe Commerce</a></td></tr>
</table>

## 400期 {#avoid-400-error}

>[!CAUTION]
>
>执行API调用时，请确保使用了某种类型的searchCriteria 。 您还可以考虑使用分页。 如果Adobe Commerce的结果过大，可能会满足Adobe Developer App Builder容量要求，并导致文件意外结束。 结果是由于400错误而导致的响应结果格式不正确。\
> 例如，假设需要从Adobe Commerce请求所有当前产品。 生成的URL类似于 `{{base_url}}rest/V1/products?searchCriteria=all`. 根据返回的目录的大小，App Builder可能无法使用json。 请改为使用分页，并发出一些请求以避免 `Response is not valid 'message/http'.`
