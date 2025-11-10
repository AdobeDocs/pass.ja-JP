---
title: Adobe Pass Authentication 2.64.1 リリースノート
description: Adobe Pass Authentication 2.64.1 リリースノート
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64.1 リリースノート {#authn-264-rn}

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-2641}

* [ビルド番号](#build-number-2641)
* [リリースの概要](#release-overview-2641)

### ビルド番号 {#build-number-2641}

Adobe Pass認証：adobe-pass-**2.64.1**

リリース日：**01/31/2023 - 02/02/2023**

### リリースの概要 {#release-overview-2641}

このリリースでは、次の機能が追加されました。

* SAML アサーションに「in_response_to」パラメーターがない場合に、MVPD からの未承諾の認証応答をブロックする機能。
* セキュリティ要件に準拠するために、redirect_url パラメーターの検証を改善しました。
* 初期デバイスから提供される情報を使用して、2 番目の画面認証要求に対するデバイス情報のロギングを改善する。
