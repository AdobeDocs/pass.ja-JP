---
title: Adobe Pass Authentication 2.64.1 リリースノート
description: Adobe Pass Authentication 2.64.1 リリースノート
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%
---
# Adobe Pass Authentication 2.64.1 リリースノート {#authn-264-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-2641}

* [ビルド番号](#build-number-2641)
* [リリースの概要](#release-overview-2641)

### ビルド番号 {#build-number-2641}

Adobe Pass認証：adobe-pass-**2.64.1**

リリース日：**01/31/2023 - 02/02/2023**

### リリースの概要 {#release-overview-2641}

このリリースでは、次の機能が追加されています。

* SAML アサーションに「in_response_to」パラメーターがないMVPDからの未承諾の認証応答をブロックする機能。
* セキュリティ要件に準拠するために、redirect_url パラメーターの検証を改善しました。
* 最初のデバイスから提供される情報を用いて、第2の画面認証要求に対するデバイス情報のロギングを改善する。
