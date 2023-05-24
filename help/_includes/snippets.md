---
title: 版本横幅
description: 重用了可视元素，以注释应用于特定版本的功能或页面
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# 版本横幅

## 仅用于EE的功能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce功能" src="../assets/adobe-logo.svg" width="20" height="20" /> 仅Adobe Commerce中的独家功能(<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">了解详情</a>)</td></tr>
</table>

## 仅限B2B功能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce功能" src="../assets/b2b.svg" width="20" height="20" /> 独家功能仅适用于 <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">适用于Adobe Commerce的B2B</a></td></tr>
</table>

## 400个问题 {#avoid-400-error}

>[!CAUTION]
>
>在执行API调用时，请确保使用某种类型的searchCriteria。 您也可以考虑使用分页。 如果Adobe Commerce产生的结果过大，则可能会达到Adobe Developer App Builder的容量，并导致文件意外结束。 结果响应结果格式不正确，出现400错误。\
> 例如，假设需要从Adobe Commerce请求所有当前产品。 生成的URL将类似于 `{{base_url}}rest/V1/products?searchCriteria=all`. 根据返回的目录的大小，json可能太大而无法供App Builder使用。 请改用分页并发出一些请求以避免 `Response is not valid 'message/http'.`
