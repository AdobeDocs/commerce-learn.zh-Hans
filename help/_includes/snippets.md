---
title: 版本橫幅
description: 重複使用的視覺元素，以記下套用至特定版本的功能或頁面
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# 版本橫幅

## EE專用功能 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce功能" src="../assets/adobe-logo.svg" width="20" height="20" /> 僅於Adobe Commerce獨家功能(<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">瞭解更多</a>)</td></tr>
</table>

## 僅限B2B功能 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce功能" src="../assets/b2b.svg" width="20" height="20" /> 獨家功能僅適用於 <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Adobe Commerce的B2B</a></td></tr>
</table>

## 400個問題 {#avoid-400-error}

>[!CAUTION]
>
>執行API呼叫時，請確定使用了某種searchCriteria。 您也可以考慮使用分頁。 如果Adobe Commerce的結果太大，可能會符合Adobe Developer App Builder容量，並導致檔案意外結束。 結果會產生400錯誤的回應結果。\
> 例如，假設需要從Adobe Commerce請求所有目前產品。 產生的URL會類似 `{{base_url}}rest/V1/products?searchCriteria=all`. 根據傳回的目錄大小，json可能太大，App Builder無法取用。 請改用分頁並提出一些請求以避免 `Response is not valid 'message/http'.`
