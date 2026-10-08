---
title: Adobe Pass Authentication 3.0.3 リリースノート
description: Adobe Pass Authentication 3.0.3 リリースノート
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%
---
# Adobe Pass Authentication 3.0.3 リリースノート {#authn-303-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-303}

* [ビルド番号](#build-number-303)
* [リリースの概要](#release-overview-303)

### ビルド番号 {#build-number-303}

Adobe Pass認証：adobe-pass-**3.0.3**

リリース日：**10/29/2024 - 10/31/2024**

### リリースの概要 {#release-overview-303}

#### REST API v2

##### コード

* REST API V2の機能強化（Adobe Pass 3.0のメジャーリリースでは[REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)が提供されているので）。
* 認証コードの有効性に関する情報を返すために、`/sessions/{code}`に`notBefore`と`notAfter`のフィールドを追加しました。
* プラットフォーム識別の向上：

##### 関連文書

* 新しいREST API v2で開始するには、[REST API v2の概要](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) ドキュメントを参照してください。

##### ツール

* 新しいREST API v2を試すには、[Adobe Developer](https://developer.adobe.com/adobe-pass) web サイトの新しいAdobe Pass認証ページを参照してください。
