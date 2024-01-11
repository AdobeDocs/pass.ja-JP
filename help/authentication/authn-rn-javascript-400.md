---
title: Adobe Pass Authentication JavaScript 4.0.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.0.0 リリースノート
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.0.0 リリースノート {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-no-javascript-sdk-400}

Adobe Pass認証：JavaScript 4.0.0

リリース日： **07/05/2018**


## リリースの概要 {#overview-javascript-sdk-400}

* プラットフォーム間で統合されたセキュリティメカニズムである、動的なクライアント登録メカニズムのサポートを実装しました。 プラットフォーム間のセキュリティアクセスの管理は、TVE ダッシュボードを使用して、プログラマー自身がおこなうことができます。
* すべての機能で、すべてのサードパーティ Cookie とほぼすべてのファーストパーティ Cookie を使用しなくなり（ファーストパーティ Cookie を使用する個別化を除く。将来は削除される）、サードパーティ Cookie に関する新しいブラウザーポリシー（例：cookie）との互換性と将来性が向上 Safari の ITP)。 以前の 3.x リリースは、ファーストパーティ Cookie のみを使用することで、幸せなフローに対して期待どおりに動作しますが、新しいブラウザーバージョンのリリースでは動作する可能性があります。
* プリフライト承認チェックのキャッシュを無効にする機能をサポートしました。
* このバージョンは後方互換性がなく、新しいパス ( https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js ) でホストされます。 この新しい JS SDK を活用するには、新しい動的クライアント登録メカニズムを使用して Web アプリケーションを登録し、新しい JS SDK URL を指すために、プログラマーが移行プロセスを実行する必要があります。


## リリースパッケージ {#rel-pkg-javascript-sdk-400}

実稼動 URL は、 https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL はhttps://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.jsです。
