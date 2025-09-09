---
title: Adobe Pass Authentication 3.4.0 リリースノート
description: Adobe Pass Authentication 3.4.0 リリースノート
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 3a275f64f7f8cffa3bdc0d546c8e2db517840069
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Adobe Pass Authentication 3.4.0 リリースノート {#authn-340-rn}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-340}

* [ビルド番号](#build-number-340)
* [リリースの概要](#release-overview-340)

### ビルド番号 {#build-number-340}

Adobe Pass認証：adobe-pass-**3.4.0**
リリース日：**09/09/2025 - 9/11/2025**

### リリースの概要 {#release-overview-340}

#### REST API v2

* ユーザー ID とトラッキング機能を向上させるために [&#128279;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md)Experience Cloud ID （ECID）のサポートを追加しました。
* REST API V2 [ 設定 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) のヘッダー AP-Device-Identifier と X-Device-Info をオプションに変更しました。

#### バグの修正

* REST API V2[ 決定 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) 応答からのトークンでMVPD ID とプロキシ MVPD ID が反転される問題を修正しました。
* REST API V2[ 決定 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) 応答からのトークンにリクエスター ID が存在しない問題を修正しました。
* REST API V2 [ 事前承認決定 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API に渡されるリソースが多すぎると、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources** ではなく、応答で空の決定リストが生成される問題を修正しました。

#### その他

* パッチが適用されたセキュリティ脆弱性。