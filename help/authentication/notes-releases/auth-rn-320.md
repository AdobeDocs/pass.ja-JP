---
title: Adobe Pass Authentication 3.2.0 リリースノート
description: Adobe Pass Authentication 3.2.0 リリースノート
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%
---
# Adobe Pass Authentication 3.2.0 リリースノート {#authn-320-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-320}

* [ビルド番号](#build-number-320)
* [リリースの概要](#release-overview-320)

### ビルド番号 {#build-number-320}

Adobe Pass認証：adobe-pass-**3.2.0**

リリース日：**06/10/2025 - 06/12/2025**

### リリースの概要 {#release-overview-320}

#### REST API v2

* [&#x200B; セッション API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)応答でパラメーターが見つからない場合に、新しい理由`missing_parameters_fallback`が追加されました。
* [Sessions API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)応答に新しいフィールド「デバイス」が追加されました。

#### 新機能

* Degradation APIを拡張して、1回の呼び出しで複数のMVPDにデグラデーション ルールを適用する機能を追加します

#### バグ修正

* ユーザーのメタデータ処理が失敗した場合に、認証フローが正常に終了しない問題を修正しました。
* 承認理由が正しく計算されない問題を修正しました。
* プロファイル応答にupstreamUserIDが存在しない問題を修正しました。
* 認証リクエストにREST API V2のスコープ情報が含まれない問題を修正しました。認証要求を開始します。
