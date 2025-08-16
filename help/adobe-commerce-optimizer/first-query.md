---
title: 如何查询数据
description: 了解如何查询Adobe Commerce Optimizer实例的数据。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 查询数据Adobe Commerce Optimizer

了解如何在Adobe Commerce Optimizer实例上使用GraphQL查询数据。

## 此视频面向谁？

* Commerce解决方案架构师和开发人员

## 视频内容

* 使用GraphQL查询数据
* 使用jq使json更易于阅读

>[!VIDEO](https://video.tv.adobe.com/v/3470810?learn=on&enablevpops&captions=chi_hans)

## 代码示例

请确保将诸如`{{insert-your-graphql-endpoint-url}}`、`{{insert-your-ac-source-locale}}`和`{{your-search-query-string}}`之类的值与您的查询所需的值交换。

基本示例查询

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

使用`jq`对输出进行美化打印的基本示例查询

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 相关内容

* [促销API快速入门](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/zh-hans/docs/commerce/optimizer/overview){target="_blank"}
