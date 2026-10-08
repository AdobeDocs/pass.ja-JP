---
title: Adobe Pass Authentication 3.1.0 リリースノート
description: Adobe Pass Authentication 3.1.0 リリースノート
exl-id: cf9fc8e2-4b37-4b0a-a6ed-cda1b6738e76
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%
---
# Adobe Pass Authentication 3.1.0 リリースノート {#authn-310-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-310}

* [ビルド番号](#build-number-310)
* [リリースの概要](#release-overview-310)

### ビルド番号 {#build-number-310}

Adobe Pass認証：adobe-pass-**3.1.0**

リリース日：**02/25/2025 - 02/27/2025**

### リリースの概要 {#release-overview-310}

#### REST API v2

* REST API v2 [ ログアウト API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)応答で、通常のログアウトとパートナーのシングルサインオン ログアウトを区別する新しい`partner_logout` アクション名と`partner_interactive` アクションタイプ。
* REST API v2 [ セッション API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)および[ セッション SSO API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)応答のアクション名に関する詳細なインサイトを提供する新しい`reason` フィールド。

#### バグ修正

* REST API v2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)を介してSpectrum サブスクライバーが認証できない問題を修正しました。
* REST API V2 [Authenticate API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)で生成されたイベントがESMで適切に集約されない問題を修正しました。
* REST API v2 [ プロファイル API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)応答のユーザープロファイルに対して`notBefore` タイムスタンプの誤った計算が発生する問題を修正しました。

#### JavaScript SDK

* [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md)のバージョン 3.5.0を削除しました。
