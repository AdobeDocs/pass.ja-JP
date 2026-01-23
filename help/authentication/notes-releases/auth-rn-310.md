---
title: Adobe Pass Authentication 3.1.0 リリースノート
description: Adobe Pass Authentication 3.1.0 リリースノート
exl-id: cf9fc8e2-4b37-4b0a-a6ed-cda1b6738e76
source-git-commit: 0c6aec04ae9df410228730b5bce6ced1aeecd312
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---

# Adobe Pass Authentication 3.1.0 リリースノート {#authn-310-rn}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-310}

* [ビルド番号](#build-number-310)
* [リリースの概要](#release-overview-310)

### ビルド番号 {#build-number-310}

Adobe Pass認証：adobe-pass-**3.1.0**

リリース日：**2025/25/02/25 - 2025/27/02**

### リリースの概要 {#release-overview-310}

#### REST API v2

* REST API v2[Logout API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) 応答で通常のログアウトとパートナースシングルサインオンログアウトを区別する新しい `partner_logout` アクション名と `partner_interactive` アクションタイプ。
* REST API v2 [ セッション API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) および [ セッション SSO API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) 応答のアクション名に関するより多くのインサイトを提供する新しい `reason` フィールド。

#### バグの修正

* Spectrum サブスクライバーが REST API v2 [ 認証 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) を介して認証できない問題を修正しました。
* REST API V2[ 認証 API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) を介して生成されたイベントが ESM に適切に集計されない問題を修正しました。
* REST API v2 [Profiles API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) 応答のユーザープロファイルのタイムスタンプ `notBefore` 誤って計算される問題を修正しました。

#### JavaScriptSDK

* [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md) のバージョン 3.5.0 を削除。
