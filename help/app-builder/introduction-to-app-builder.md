---
title: Adobe Systems 商务的进程外扩展性
description: 了解 Adobe Systems 应用程序生成器以及它为什么是进程外扩展性的一个重要方面。
landing-page-description: 了解应用程序生成器以及它如何帮助 Adobe Systems 商务开发策略。
short-description: 瞭解什麼是App Builder以及它如何協助Adobe Commerce開發策略。
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 0%

---

# App Builder簡介

過去，Adobe Commerce開發一直使用程式內的擴充功能。 進程內模型需要任何新程式碼才能與升級、伺服器的PHP版本，以及Commerce使用的許多其他基本伺服器應用程式和服務相容。 Adobe Developer App Builder使用程式外擴充功能來避免這些相容性問題。

## 適用於Adobe Commerce的App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder是整合及建立自訂體驗以延伸Adobe解決方案的無伺服器擴充性平台，現在可供Adobe Commerce使用。 透過App Builder，您可以建立安全且可擴展的應用程式，這些應用程式可擴充Commerce原生功能並與協力廠商解決方案整合。 作為開發人員，您現在可以利用Adobe Commerce的流程外擴充功能，而這又可提供即期和長期好處。

App Builder提供統一的協力廠商擴充性架構，可整合及建立可擴充的自訂應用程式 [!DNL Adobe Commerce]. 由於此擴充性架構是以Adobe的基礎架構所建置，因此開發人員可以建置自訂微服務，以及擴充和整合 [!DNL Adobe Commerce] 跨其他Adobe解決方案和協力廠商整合。

App Builder為客戶提供擴充功能的方式 [!DNL Adobe Commerce] 在各種使用案例中：

* 中介軟體擴充性 — 藉由建立自訂聯結器或利用預先建立的整合套件，將外部系統與Adobe應用程式連線。
* 核心服務擴充性 — 透過自訂功能和商業邏輯擴充預設行為，進而擴充核心應用程式功能。
* 用户体验扩展性-扩展核心体验以支持业务需求或版本特定于客户的数字资产、商店和后勤应用程序。

Adobe Systems 开发人员应用程序生成器是基于云的解决方案，这意味着它会自动进行缩放。 此服务也是全局分发的，无论您的地理位置如何，都可以获得最佳性能。

## 为何要了解有关应用程序生成器的更多信息

由于 Adobe Systems 商务不是完全 SAAS 产品，因此您开发的代码可能会增加复杂性和升级问题。 通过使用进程外扩展性（如应用程序生成器），您可以为 Adobe Systems 商务商店提供自定义的独特功能，而无需处理进程内的方法。

其他好处包括：

* 通过分离功能，可加快启动的时间。
* 现在可以更轻松地进行升级。 自定义功能位于商务代码库之外，可防止升级时出现兼容性问题。
* 在商务外部移动功能和逻辑可释放通常由进程内开发方法使用的资源。

## 建筑 {#architecture}

Adobe Developer App Builder不是現成可用的解決方案，而是提供通用、一致且標準化的開發平台，用於擴充Adobe Cloud解決方案(例如Adobe Commerce)，包括：

* 用於自訂微服務和擴充功能開發的Adobe Developer主控台。 在存取建立外掛程式和整合所需的所有工具和API時，建立和管理專案。
* 開放原始碼工具、SDK和程式庫，用於建置自訂擴充功能和整合。 使用React Spectrum (Adobe的UI工具組)，讓所有Adobe應用程式擁有一個通用UI。
* 服務，例如I/O Runtime，用於在Adobe的無伺服器平台上託管基礎結構，以及I/O事件，用於事件式整合。 Adobe也提供儲存資料和檔案的立即可用支援。
* Adobe Experience Cloud可讓您提交擴充功能和整合內容，以在Experience Cloud組織中發佈。系統管理員可以檢閱、管理和核准這些擴充功能。 發佈後，您的自訂App Builder擴充功能和工具即可與其他Adobe Experience Cloud應用程式搭配使用。

下圖說明在App Builder上建置的標準應用程式如何使用這些功能：

![架構](/help/assets/app-builder/app-builder-architecture.jpeg)

如需App Builder架構的詳細資訊，請參閱 [架構概述](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## AmazonSales Channel擴充功能 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Amazon Sales Channel 扩展仍处于开发阶段，尚未正式发布。  这些视频和教程旨在向您展示如何将 Adobe Systems 开发人员应用程序生成器用于实际用例。

以下教程演示如何使用应用程序生成器扩展将 Adobe Systems 商务连接到 Amazon Sales Channel。

* [技术概述应用程序生成器](../app-builder/app-builder-technical-overview.md)
* [扩展性框架](../app-builder/extensibility-framework-commerce-eventing.md)
* [功能演示应用程序生成器](../app-builder/app-builder-functional-demonstration.md)

## 与应用程序生成器开始使用 {#additional-resources}

可撰寫商務策略的概觀，包括初始設定，請參閱以下部落格：

[App Builder如何協助提升商務平台的商業靈敏度](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

為協助您開始使用App Builder，Adobe已建立下列檔案：

* [App Builder入門](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 繼續學習使用檔案 {#appbuilder-documentation}

App Builder為開發人員提供影片和檔案，包括指南和參考檔案，以協助您開發自己的自訂應用程式：

* [App Builder檔案](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder影片](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 嘗試其中一個範例應用程式 {#appbuilder-codesamples}

準備好開始開發了嗎？ 下列連結包含協助您入門的範例應用程式：

* [Adobe Systems 开发人员网站上的应用程序生成器 Code 实验室](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 支持 {#support}

对于开发人员支持请求，请使用 [ 体验联盟论坛 ](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) {target="_blank"} 寻求帮助。

{{$include /help/_includes/app-builder-related-links.md}}
