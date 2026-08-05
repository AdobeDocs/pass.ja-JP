---
title: Adobe Pass Authentication 3.8.0 リリースノート
description: Adobe Pass Authentication 3.8.0 リリースノート
hold: true
source-git-commit: ce9e8de3d69699d03cf68c86be1bb811967501dc
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Adobe Pass Authentication 3.8.0 リリースノート {#authn-380-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-380}

* [ビルド番号](#build-number-380)
* [リリースの概要](#release-overview-380)

### ビルド番号 {#build-number-380}

Adobe Pass認証：adobe-pass-**3.8.0**\
リリース日：**08/11/2026 - 08/13/2026**

### リリースの概要 {#release-overview-380}

このリリースでは、Adobe Pass認証サービス全体の安定性、機能強化、およびセキュリティアップデートに焦点を当てています。

#### バグ修正

* deviceIdの特定の無効な文字が原因で、V2 APIでHTTP 500 エラーが発生する問題を修正しました。

#### 機能強化

* ローリングトークンの更新をサポートするための更新トークンの処理が改善されました。
* Analytics用のセカンダリデバイスでのvisitorId認識機能が強化されました。
* セキュリティ制御を強化し、システム全体の整合性を向上させるために、URL パラメーター検証を強化しました。
* TVE Dashboard バージョン 1.5.2 （UIのマイナーな改善あり）。
