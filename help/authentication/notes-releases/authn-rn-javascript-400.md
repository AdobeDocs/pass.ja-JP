---
title: Adobe Pass Authentication JavaScript 4.0.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.0.0 リリースノート
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.0.0 リリースノート {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-number-400}

Adobe Pass認証：JavaScript 4.0.0

リリース日：**07/05/2018**

## リリースの概要 {#release-overview-400}

* プラットフォーム間の統合セキュリティメカニズムである、動的クライアント登録メカニズムのサポートを実装しました。 プラットフォーム間のセキュリティ アクセスの管理は、プログラマ自身が TVE ダッシュボードを介して実行できます。
* すべてのサードパーティ cookie の使用を排除し、すべての機能に対するほとんどすべてのファーストパーティ cookie の使用を排除します（ただし、ファーストパーティ cookie が引き続き使用される場合は個別化を除き、今後削除される予定です）。これにより、サードパーティ cookie、または cookie 全般（例： Safari の ITP）。 以前の 3.x リリースは、ファーストパーティ cookie のみを使用することでハッピーフローで期待どおりに動作しますが、これは新しいブラウザーバージョンのリリースで変更される場合があることに注意してください。
* プリフライト認証チェックのキャッシュ無効化のサポート。
* このバージョンは後方互換性がなく、新しいパス https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsの下でホストされています。 この新しい JS SDKのメリットを活用するには、プログラマーが新しい動的クライアント登録メカニズムを使用して web アプリケーションを登録し、新しい JS SDK URL を指定するための移行プロセスが必要です。

## リリースパッケージ {#release-package-400}

実稼動 URL はhttps://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL はhttps://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.jsです。
