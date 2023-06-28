---
title: 创建购物车价格规则
description: 了解如何创建根据一组条件在购物车中应用折扣的购物车价格规则。
doc-type: feature video
audience: all
activity: use
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Catalogs, Customers, Shopping Cart
topic: Commerce, Administration
role: Admin, Leader, User
level: Beginner, Intermediate
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---

# 创建购物车价格规则

购物车价格规则根据一组条件对购物车中的商品应用折扣。 当满足条件或客户输入有效优惠券代码时，可自动应用折扣。 应用后，折扣会显示在小计下的购物车中。 购物车价格规则可以根据需要通过更改其状态和日期范围用于季节或促销。

## 此视频面向谁？

- 电子商务营销人员
- 网站管理员

## 视频内容

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## 定价显示问题

有一些唯一方案要求每个行项目显示其提供的折扣，但值可能不完全匹配。 原因在于，当购物车价格规则折扣应用于多个产品时，值未平均分为两位小数。

>[!BEGINSHADEBOX]

购物车价格规则=应用于购物车中2种产品的10%折扣价格规则生效的条件：购物车中的项目总数为2操作应用产品价格折扣的百分比，折扣金额为10

购物车中添加了2件商品，每件售价19.95美元

要获得折扣额乘以产品价格，请执行以下操作： 0.1

19.95 x 0.1 = 1.995

这就是问题所在，我们有3位小数，而不是2位。 将这笔钱转换为美元现在是一个问题

>[!ENDSHADEBOX]

### 解决方案

考虑到网站负责人（是唯一受此问题影响的人），确定显示以美元折扣订购的每件商品是最合适的。 为了确保正确计算整个订单金额，决定舍入第一个项目，其他项目舍去第三个小数。 查看此方案：

>[!BEGINSHADEBOX]

与生效的购物车规则上相同10%折扣将2个产品添加到19.95的购物车

每个产品可获得1.995美元的折扣产品1 - 19.95 x 0.1 = 1.995 2 - 19.95 x 0.1 = 1.995

共计有3.99条给予客户折扣

在管理员中向商店所有者显示行项目时，我们需要调整第一个项目，并将其四舍五入到2.000。第二项，我们将第三个小数乘积1 = 2.00乘积2 = 1.99

这两种产品的总折扣现在加总后与提供给客户的实际折扣匹配。
>[!ENDSHADEBOX]

下面是一个屏幕截图，它将显示在具有此方案的订单的管理员中：

![显示具有不同值的已排序项目的“管理员”视图](../assets/commerce-admin-cart-price-rule-values-different.png)

### 其他可能的解决方案以及未使用它们的原因

>[!BEGINSHADEBOX]

与生效的购物车规则上相同10%折扣将2个产品添加到19.95的购物车

每件产品应有1.995美元的折扣，但如果我们只是把它们凑在一起购买，就会显示太多的折扣。

产品1 - 19.95 x 0.1 = 1.995产品2 - 19.95 x 0.1 = 1.995

转换为四舍五入所有项目产品1新值为2.00产品2新值为2.00

实际提供给客户的总折扣为3.99，但是如果我们查一查，就会发现给了4.00美元，这是不正确的。

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

类似的问题如果对于所有项目舍弃小数点后三个，则会显示提供的折扣太少。

>[!BEGINSHADEBOX]

与生效的购物车规则上相同10%折扣将2个产品添加到19.95的购物车

每个产品应获得$1.995的折扣，但是，如果我们仅舍弃小数点后三个，就会发生这种情况：产品1 - 19.95 x 0.1 = 1.995产品2 - 19.95 x 0.1 = 1.995

转换为拖放所有项目的第三个小数点产品1新值为1.99产品2新值为1.99

实际提供给客户的折扣总额为3.99，但如果我们删去小数点后三个，就会显示给了$3.98，这是不正确的。

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]


## 其他资源

- [创建购物车价格规则 —  [!DNL Commerce] Merchandising and Promotions指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [优惠券代码 —  [!DNL Commerce] Merchandising and Promotions指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)
