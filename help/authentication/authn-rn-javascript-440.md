---
title: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0 リリースノート {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-no-javascript-sdk-440}

Adobe Pass認証：JavaScript 4.4.0

リリース日：**06/22/2021**


## リリースの概要 {#overview-javascript-sdk-440}

### 新機能 {#new-features-javascript-sdk-440}

プリフライト認証

* 新しい事前認証 API – これは、プリフライトフローの認証機能に使用される新しい API 呼び出しです。プリフライトフローの強化されたエラーレポートも導入されています。
* この機能は、Primetime Authentication 設定で有効にする必要があるので、リクエスト時に使用できます。 この機能を有効にする方法の詳細については、担当の TAM にお問い合わせください。
* checkPreauthorizedResources API を廃止します。

プラットフォームの識別

* すべての SDK 呼び出しに AP-SDK-Identifier ヘッダーを追加して、SDK のタイプとバージョンをより正確に識別します。

その他

* 内部アーキテクチャの改善。


### バグの修正 {#bug-fixes-javascript-sdk-440}

* setRequestor と getAuthentication が同時に呼び出されたときに発生する競合状態を修正しました。
* 権限がステージング環境に正しく読み込まれない問題を修正しました。
* Safari ブラウザーでバックグラウンドログアウトフローが完了せず、ページが更新されるまでユーザーが認証されているように見える問題を修正しました。 タイムアウトが導入され、現在は 30 秒に設定されています。この間に Primetime Authentication サーバーから応答がない場合、SDK は setAuthenticationStatus コールバックを呼び出します。

## リリースパッケージ {#rel-pkg-javascript-sdk-440}

実稼動 URL はhttps://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL はhttps://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.jsです。
