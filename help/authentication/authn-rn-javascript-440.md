---
title: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.4.0 リリースノート
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0 リリースノート {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-no-javascript-sdk-440}

Adobe Pass認証：JavaScript 4.4.0

リリース日： **06/22/2021**


## リリースの概要 {#overview-javascript-sdk-440}

### 新機能 {#new-features-javascript-sdk-440}

プリフライト認証

* 新しい事前認証 API — これは、プリフライト承認機能に使用される新しい API 呼び出しです。プリフライトフローの拡張エラーレポートも導入されます。
* この機能は Primetime 認証設定で有効にする必要があるので、リクエストに応じて使用できます。 この機能の有効化方法の詳細については、担当の TAM にお問い合わせください。
* checkPreauthorizedResources API を廃止します。

プラットフォームの識別

* すべての SDK 呼び出しに AP-SDK-Identifier ヘッダーを追加して、SDK のタイプとバージョンをより識別しやすくします。

その他

* 内部アーキテクチャの改善。


### バグの修正 {#bug-fixes-javascript-sdk-440}

* setRequestor と getAuthentication が同時に呼び出される際に発生する競合状態を修正しました。
* ステージング環境で権限が正しく読み込まれない問題を修正しました。
* Safari ブラウザーでバックグラウンドのログアウトフローが完了しない問題に対処し、その結果、ページの更新がおこなわれるまで、ユーザーは認証済みのように見えました。 タイムアウトが導入され、現在 30 秒に設定されています。そのため、この期間に Primetime Authentication サーバーからの応答がない場合、SDK は setAuthenticationStatus コールバックを呼び出します。

## リリースパッケージ {#rel-pkg-javascript-sdk-440}

実稼動 URL は、 https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL はhttps://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.jsです。
