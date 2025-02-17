---
title: Adobe Pass Authentication 3.0 リリースノート
description: Adobe Pass Authentication 3.0 リリースノート
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0 リリースノート {#authn-300-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-300}

* [ビルド番号](#build-number-300)
* [リリースの概要](#release-overview-300)

### ビルド番号 {#build-number-300}

Adobe Pass認証：adobe-pass-**3.0**

リリース日：**09/10/2024 - 09/12/2024**

### リリースの概要 {#release-overview-300}

#### REST API v2

##### コード

* adobe-pass-**3.0** リリース以降、新しいクライアントアプリケーションや既存のクライアントアプリケーションを新しい REST API v2 に統合または移行して、最新のAdobe Pass機能を活用できます。

##### マニュアル

* 新しい REST API v2 の使用を開始するには、次のドキュメントを参照してください。
   * [REST API v2 - API – 概要](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - フロー – 概要](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* REST API v1 の公開ドキュメントの URL が変更されました。次のドキュメントを参照してください。
   * [REST API v1 - API – 概要](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [REST API v1 - API - リファレンス](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### ツール

* 新しい REST API v2 を試すには、[Adobe Developer](https://developer.adobe.com/adobe-pass) web サイトの新しいAdobe Pass認証ページを参照してください。

#### バグの修正

* ログアウトリクエストに存在する場合に、リダイレクト URL パラメーターが使用されない問題を修正しました。
