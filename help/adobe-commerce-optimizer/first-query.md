---
title: 如何查询数据
description: 了解如何使用GraphQL查询Adobe Commerce Optimizer产品数据，包括如何使用jq和结构搜索查询变量来设置JSON响应的格式。
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 204
last-substantial-update: 2025-08-13
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
TQID: https://experienceleague.adobe.com/IxrS6rwleWgU0-jtwu4hUavQuZesbQ6h5z7zVZR2xCo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 127
ht-degree: 0%

---

# 查询数据Adobe Commerce Optimizer

了解如何在Adobe Commerce Optimizer实例上使用GraphQL查询数据。

## 此视频面向谁？

* Commerce解决方案架构师和开发人员

## 视频内容

* 使用GraphQL查询数据
* 使用jq使json更易于阅读

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on)

## 代码示例

请确保将诸如`{{insert-your-graphql-endpoint-url}}`、`{{insert-your-ac-view-id}}`和`{{your-search-query-string}}`之类的值与您的查询所需的值交换。

基本示例查询

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

使用`jq`对输出进行美化打印的基本示例查询

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 相关内容

* [促销API快速入门](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 指南](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}

