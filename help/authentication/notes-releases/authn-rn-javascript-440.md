---
title: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0 リリースノート {#javascript-sdk-440-rn}

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-number-440}

Adobe Pass認証：JavaScript 4.4.0

リリース日：**06/22/2021**

## リリースの概要 {#release-overview-440}

### 新機能

プリフライト認証

* 新しい事前認証 API – これは、プリフライトフローの認証機能に使用される新しい API 呼び出しです。プリフライトフローの強化されたエラーレポートも導入されています。
* この機能は、Primetime Authentication 設定で有効にする必要があるので、リクエスト時に使用できます。 この機能を有効にする方法の詳細については、担当の TAM にお問い合わせください。
* checkPreauthorizedResources API を廃止します。

プラットフォームの識別

* すべてのSDK呼び出しに AP-SDK-Identifier ヘッダーを追加して、SDKのタイプとバージョンをより正確に識別します。

その他

* 内部アーキテクチャの改善。

### バグの修正

* setRequestor と getAuthentication が同時に呼び出されたときに発生する競合状態を修正しました。
* 権限がステージング環境に正しく読み込まれない問題を修正しました。
* Safari ブラウザーでバックグラウンドログアウトフローが完了せず、ページが更新されるまでユーザーが認証されているように見える問題を修正しました。 タイムアウトが導入され、現在は 30 秒に設定されています。この間に Primetime Authentication サーバーから応答がない場合、SDKは setAuthenticationStatus コールバックを呼び出します。

## リリースパッケージ {#release-package-440}

実稼動 URL はhttps://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL はhttps://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.jsです。
