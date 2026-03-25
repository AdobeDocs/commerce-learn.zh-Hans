---
title: 创建购物车价格规则
description: 了解如何创建在满足您定义的条件时在购物车中应用折扣的购物车价格规则。
doc-type: Tutorial
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: User
level: Beginner
duration: 353
jira: KT-17148
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 0%

---

# 创建购物车价格规则

购物车价格规则会根据您设置的条件，对购物车中的商品应用折扣。 当满足条件或客户输入有效优惠券代码时，将自动应用折扣。 折扣显示在小计下的购物车中。 您可以通过更改其状态和日期范围来打开或关闭季节或促销活动规则。

## 此视频面向谁？

* 电子商务营销人员
* 网站管理员

## 视频内容

* 创建购物车价格规则和可选优惠券代码。
* 了解折扣在购物车中的显示方式以及促销情况。

>[!VIDEO](https://video.tv.adobe.com/v/343835?learn=on)

## 定价显示问题

在某些情况下，每个行项目必须显示应用的折扣，但显示的值可能并不完全匹配。 当购物车价格规则对多个产品应用一个折扣，并且拆分未平均分配到两位小数时会发生这种情况。

>[!BEGINSHADEBOX]

购物车价格规则=对购物车中2个产品应用了10%的折扣
价格规则生效的条件：购物车中的项目总数为2
操作应用产品价格折扣的百分比，且折扣金额为10

购物车中添加了2个项目，每个项目的价格为19.95美元

要获得折扣额乘以产品价格0.1

19.95 x 0.1 = 1.995

这就是问题所在，我们有3位小数，而不是2位。 将这笔钱兑换成美元现在成了一个问题

>[!ENDSHADEBOX]

### 解决方案

对于Admin中的商家来说，最明确的方法是显示每个订购行的美元折扣。 要保持订单总计正确，请将第一行项目向上四舍五入，并将第三个小数点放在剩余的行项目上。 查看此方案：

>[!BEGINSHADEBOX]

与上面生效的购物车规则相同的10%折扣
将2种产品(19.95)添加到购物车

每件产品可获得1.995美元的折扣
产品1 - 19.95 x 0.1 = 1.995
2 - 19.95 x 0.1 = 1.995

总共3.99元作为客户的折扣

在管理员中向商店所有者显示行项目时，
我们需要调整第一个项目并将其四舍五入到2.000。对于第二项，删除第三个小数。
产品1 = 2.00
产品2 = 1.99

这两种产品的合计总折扣现在与提供给客户的实际折扣匹配。
>[!ENDSHADEBOX]

以下是一个屏幕截图，它将显示在具有此方案的订单的管理员中：

![显示具有不同值的排序项的管理员视图](../assets/commerce-admin-cart-price-rule-values-different.png)

### 其他可能的解决方案以及未使用它们的原因

>[!BEGINSHADEBOX]

与上面生效的购物车规则相同的10%折扣
将2种产品(19.95)添加到购物车

每个产品可获得1.995美元的折扣，
然而，如果我们只是把它们围起来，就会显示太多的折扣。

产品1 - 19.95 x 0.1 = 1.995
产品2 - 19.95 x 0.1 = 1.995

转换为对所有项目进行向上舍入
产品1新值为2.00
产品2新值为2.00

实际提供给客户的3.99毛额为折扣，
但是，如果我们汇总，就会显示4.00美元已经提供，这是不正确的。

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

类似的问题如果舍弃所有项目的小数点后三位，则会显示提供的折扣太少。

>[!BEGINSHADEBOX]

与上面生效的购物车规则相同的10%折扣
将2种产品(19.95)添加到购物车

每个产品应获得1.995美元的折扣，但如果我们只舍弃小数点后三位，就会出现以下情况：
产品1 - 19.95 x 0.1 = 1.995
产品2 - 19.95 x 0.1 = 1.995

转换为拖放所有项目的第三个小数
产品1新值为1.99
产品2新值为1.99

实际提供给客户的3.99毛额为折扣，
但是，如果我们舍弃小数点后三位，则会显示$3.98已提供，并且不正确。

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]

## 其他资源

* [创建购物车价格规则 —  [!DNL Commerce] 促销和促销指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html?lang=zh-Hans){target="_blank"}
* [优惠券代码 —  [!DNL Commerce] 促销和促销指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html?lang=zh-Hans){target="_blank"}
