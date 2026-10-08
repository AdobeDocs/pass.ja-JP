---
title: Adobe Pass Authentication 3.4.0 リリースノート
description: Adobe Pass Authentication 3.4.0 リリースノート
exl-id: ad572617-f607-419d-a085-70c025465080
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%
---
# Adobe Pass Authentication 3.4.0 リリースノート {#authn-340-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-340}

* [ビルド番号](#build-number-340)
* [リリースの概要](#release-overview-340)

### ビルド番号 {#build-number-340}

Adobe Pass Authentication: adobe-pass-**3.4.0**
リリース日：**09/16/2025 - 09/18/2025**

### リリースの概要 {#release-overview-340}

#### REST API v2

* ユーザーの識別と追跡機能を向上させるために、[Experience Cloud ID （ECID） &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md)のサポートを追加しました。
* REST API V2 [設定API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)のヘッダーAP-Device-IdentifierとX-Device-Infoをオプションに変更しました。

#### バグ修正

* REST API V2 [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)の応答からトークンでMVPD IDとプロキシ MVPD IDが反転される問題を修正しました。
* REST API V2 [決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)応答のトークンに依頼者IDが存在しない問題を修正しました。
* REST API V2 [Preauthorize Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) APIにリソースが多すぎると、[拡張エラーコード &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources**&#x200B;ではなく、応答に空の意思決定リストが表示される問題を修正しました。

#### その他

* パッチを適用したセキュリティの脆弱性：
