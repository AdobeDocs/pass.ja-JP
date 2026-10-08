---
title: Adobe Pass Authentication 2.64 リリースノート
description: Adobe Pass Authentication 2.64 リリースノート
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%
---
# Adobe Pass Authentication 2.64 リリースノート {#authn-264-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-264}

* [ビルド番号](#build-number-264)
* [リリースの概要](#release-overview-264)

### ビルド番号 {#build-number-264}

Adobe Pass認証：adobe-pass-**2.64**

リリース日：**11/08/2022 - 11/10/2022**

### リリースの概要 {#release-overview-264}

* サーバー応答時間を短縮し、システム全体のパフォーマンスを向上させることを目的としたインフラストラクチャの更新。
* 新しいプラットフォーム識別メカニズムの改善。
* SAML アサーションから「in_response_to」パラメーターが欠落しているMVPDからの未承諾の認証応答をブロックする機能。

#### バグ修正

* 一部の従来のTempPass トークンの形式が正しくないことを修正しました。
* 2番目の画面認証フローの軽微な問題を修正しました。
