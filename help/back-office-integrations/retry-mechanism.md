---
title: 使用重试机制的本机功能
description: 将Adobe I/O Events的重试机制用于弹性应用程序，包括重试条件和视觉指示器。
landing-page-description: 了解并利用Adobe I/O Events的内置重试机制来增强应用程序可复原性并有效管理事件激活。
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# 利用Adobe I/O Events重试机制实现应用程序可复原性

此视频概述了关于利用Adobe I/O Events内置重试机制增强应用程序可复原性的全面指南。 了解特定的HTTP响应状态代码如何触发事件重试。 Adobe I/O Events使用指数和固定的重试回退策略，间隔从1分钟增加到15分钟。 该文档还详细说明了重试指示器如何在开发人员控制台中显示，并显示警告图标和指示失败和重试事件的圆形箭头等可视提示。

了解重试机制如何在“使用者”运行时操作的上下文中运行，并确定是否重试了事件。 成功的响应以200状态代码表示，而错误响应包括具有“statusCode”属性的错误对象。 “consumer”运行时操作根据下游处理结果确定要返回的HTTP响应代码，确保高效的事件处理和最终成功的激活。

## 受众

* 希望了解触发事件重试的特定HTTP响应状态代码的开发人员。
* 希望了解Adobe I/O Events在重试时采用的指数级和固定回退策略的团队。
* 希望了解开发人员控制台中的可视化指标如何表示失败和重试事件的开发人员。

## 视频内容

* Adobe I/O Events具有内置的现成重试机制，该机制可根据特定HTTP响应状态代码自动重试事件激活。
* Adobe I/O Events实现的重试机制涉及指数型回退策略和固定回退策略。
* 开发人员控制台中的可视指示器，例如失败事件的警告图标和重试事件的圆形箭头图标。
* “消费者”运行时操作在确定用于事件处理的相应HTTP响应状态代码方面发挥着关键作用。

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 相关文档

* [Webhook无法处理事件](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook已关闭并标记为不稳定](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
