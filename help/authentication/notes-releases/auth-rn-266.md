---
title: Adobe Pass Authentication 2.66 リリースノート
description: Adobe Pass Authentication 2.66 リリースノート
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%
---
# Adobe Pass Authentication 2.66 リリースノート {#authn-266-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-266}

* [ビルド番号](#build-number-266)
* [リリースの概要](#release-overview-266)

### ビルド番号 {#build-number-266}

Adobe Pass認証：adobe-pass-**2.66.0.1**

リリース日：**07/11/2023 - 07/13/2023**

### リリースの概要 {#release-overview-266}

このリリースでは、新しいREST APIの内部アップデートを継続しました。

#### バグ修正

* SAML ベースのMVPDのログアウトフローを修正しました。この場合、ログアウトリクエストにRelayState パラメーターがありませんでした。 リリース後に設定の更新をターゲットにして、影響を受けるMVPDのログアウトフローを復元します。
* SOAP認証エンドポイントの設定でSSL証明書を更新する機能を追加しました。
* 一部のESM レポートで「プログラマー」フィールドに誤ったデータが記録されるコーナーケースを修正しました。
