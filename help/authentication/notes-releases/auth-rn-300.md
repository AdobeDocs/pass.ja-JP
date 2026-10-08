---
title: Adobe Pass Authentication 3.0 リリースノート
description: Adobe Pass Authentication 3.0 リリースノート
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%
---
# Adobe Pass Authentication 3.0 リリースノート {#authn-300-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-300}

* [ビルド番号](#build-number-300)
* [リリースの概要](#release-overview-300)

### ビルド番号 {#build-number-300}

Adobe Pass認証：adobe-pass-**3.0**

リリース日：**09/10/2024 - 09/12/2024**

### リリースの概要 {#release-overview-300}

#### REST API v2

##### コード

* adobe-pass-**3.0** リリース以降、新規および既存のクライアントアプリケーションは、最新のAdobe Pass機能を利用するために、新しいREST API v2を統合または移行できます。

##### 関連文書

* 新しいREST API v2を使用するには、次のドキュメントを参照してください。
  * [REST API v2 - API – 概要](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
  * [REST API v2 - フロー – 概要](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* REST API v1の公開ドキュメントのURLが変更されました。次のドキュメントを参照してください。
  * [REST API v1 - API – 概要](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
  * [REST API v1 - API - リファレンス](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### ツール

* 新しいREST API v2を試すには、[Adobe Developer](https://developer.adobe.com/adobe-pass) web サイトの新しいAdobe Pass認証ページを参照してください。

#### バグ修正

* ログアウトリクエストに存在する場合にリダイレクト URL パラメーターが使用されない問題を修正しました。
