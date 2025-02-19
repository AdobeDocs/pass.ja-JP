---
title: Adobe Pass Authentication 3.0.3 リリースノート
description: Adobe Pass Authentication 3.0.3 リリースノート
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0.3 リリースノート {#authn-303-rn}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-303}

* [ビルド番号](#build-number-303)
* [リリースの概要](#release-overview-303)

### ビルド番号 {#build-number-303}

Adobe Pass認証：adobe-pass-**3.0.3**

リリース日：2024 年 **月 10 日（PT）～2024 年 10 月 31 日（PT）**

### リリースの概要 {#release-overview-303}

#### REST API v2

##### コード

* REST API V2 の機能強化（Adobe Pass 3.0 のメジャーリリースが提供された以降 [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)）。
* `/sessions/{code}` に `notBefore` フィールドと `notAfter` フィールドを追加して、認証コードの有効性に関する情報を返すようにしました。
* プラットフォームの識別が向上しました。

##### マニュアル

* 新しい REST API v2 の使用を開始するには、[REST API v2 の概要 ](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) ドキュメントを参照してください。

##### ツール

* 新しい REST API v2 を試すには、[Adobe Developer](https://developer.adobe.com/adobe-pass) web サイトの新しいAdobe Pass認証ページを参照してください。
