---
title: Adobe Pass Authentication 2.67 リリースノート
description: Adobe Pass Authentication 2.67 リリースノート
exl-id: d899fe96-a273-4681-90a5-bde54cc2f3b3
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%
---
# Adobe Pass Authentication 2.67 リリースノート {#authn-267-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-267}

* [ビルド番号](#build-number-267)
* [リリースの概要](#release-overview-267)

### ビルド番号 {#build-number-267}

Adobe Pass認証：adobe-pass-**2.67.0.1**

リリース日：**09/12/2023 - 09/14/2023**

### リリースの概要 {#release-overview-267}

* 新しいREST APIの内部アップデートを継続しました。
* 内部アーキテクチャの継続的な改善：

#### MVPD アップデート

* Adobeとの&#x200B;**DirecTV Puerto Rico**&#x200B;統合のアップデート。 詳細については、担当のTAMにお問い合わせください。

#### バグ修正

* REST APIとFireTV SDKを使用するアプリケーション間で、Adobe-Subject-Token ヘッダーを使用して取得されたSSOが破損する問題を修正しました。
