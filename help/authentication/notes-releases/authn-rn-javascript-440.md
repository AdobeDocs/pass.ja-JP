---
title: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%
---
# Adobe Pass Authentication JavaScript 4.4.0 リリースノート {#javascript-sdk-440-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## ビルド番号 {#build-number-440}

Adobe Pass認証：JavaScript 4.4.0

リリース日：**06/22/2021**

## リリースの概要 {#release-overview-440}

### 新機能

プリフライト認証

* 新しい事前認証API – これは、プリフライト認証機能に使用される新しいAPI呼び出しです。これにより、プリフライトフローのエラーレポートが強化されます。
* この機能は、Primetime Authentication設定で有効にする必要があるため、リクエスト時に利用できます。 この機能を有効にする方法の詳細については、TAMにお問い合わせください。
* checkPreauthorizedResources APIを非推奨（廃止予定）。

プラットフォームの特定

* すべてのSDK呼び出しにAP-SDK-Identifier ヘッダーを追加して、SDKのタイプとバージョンをより適切に識別します。

その他

* 内部アーキテクチャの改善：

### バグ修正

* setRequestorとgetAuthenticationが同時に呼び出されたときに生成される競合条件を修正します。
* ステージング環境で使用権限が正しく読み込まれない問題を修正しました。
* Safari ブラウザーでバックグラウンドログアウトフローを完了できない問題を修正し、ページの更新が発生するまでユーザーが認証されたように見えました。 現在30秒に設定されているタイムアウトが導入されたため、この期間中にPrimetime Authentication Serverからの応答がない場合、SDKはsetAuthenticationStatus コールバックを呼び出します。

## リリースパッケージ {#release-package-440}

実稼動URLはhttps://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
