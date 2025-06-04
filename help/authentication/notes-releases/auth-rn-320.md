---
title: Adobe Pass Authentication 3.2.0 リリースノート
description: Adobe Pass Authentication 3.2.0 リリースノート
exl-id: 4008512a-3155-4d96-9988-da0b0a496612
source-git-commit: 13b0bb640aa599109e8c2f68d1e16fbdc3840951
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Adobe Pass Authentication 3.2.0 リリースノート {#authn-320-rn}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-320}

* [ビルド番号](#build-number-320)
* [リリースの概要](#release-overview-320)

### ビルド番号 {#build-number-320}

Adobe Pass認証：adobe-pass-**3.2.0**

リリース日：**06/10/2025 - 06/12/2025**

### リリースの概要 {#release-overview-320}

#### REST API v2

* [ セッション API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) 応答でパラメーターが見つからない場合に `missing_parameters_fallback` 用される新しい理由が追加されました。
* [ セッション API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) 応答に新しいフィールド「device」が追加されました。

#### 新機能

* Degradation API を展開して、1 回の呼び出しで複数の MVPD に対して劣化ルールを適用する機能を追加します

#### バグの修正

* ユーザーメタデータの処理に失敗した場合に、認証フローを正常に終了できない問題を修正しました。
* 認証理由が正しく計算されない問題を修正しました。
* プロファイル応答に upstreamUserID が存在しない問題を修正しました。
* REST API V2 の認証リクエストを開始するためのスコーピング情報が認証リクエストに含まれていない問題を修正しました。
