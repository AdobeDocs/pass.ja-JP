---
title: Adobe Pass Authentication 3.0.3 リリースノート
description: Adobe Pass Authentication 3.0.3 リリースノート
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0.3 リリースノート {#pt-authn-303-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-303}

* [ビルド番号](#build-number-303)
* [リリースの概要](#release-overview-303)

### ビルド番号 {#build-number-2651}

Adobe Pass認証：adobe-pass-**3.0.3**
リリース日：2024 年 **月 10 日（PT）～2024 年 10 月 31 日（PT）**

### 新機能 {#new-features-303}

#### REST API v2 {#rest-apis}

##### コード

* REST API V2 の機能強化（Adobe Pass 3.0 のメジャーリリースが提供された以降 [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)）。
* `/sessions/{code}` に `notBefore` フィールドと `notAfter` フィールドを追加して、認証コードの有効性に関する情報を返すようにしました。
* プラットフォームの識別が向上しました。

##### マニュアル

* 新しい REST API v2 の使用を開始するには、[REST API v2 の概要 ](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) ドキュメントを参照してください。

##### ツール

* 新しい REST API v2 を試すには、[Adobe Developer](https://developer.adobe.com/adobe-pass) web サイトの新しいAdobe Pass認証ページを参照してください。
