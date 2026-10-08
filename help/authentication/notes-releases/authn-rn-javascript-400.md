---
title: Adobe Pass Authentication JavaScript 4.0.0 リリースノート
description: Adobe Pass Authentication JavaScript 4.0.0 リリースノート
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 0%
---
# Adobe Pass Authentication JavaScript 4.0.0 リリースノート {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## ビルド番号 {#build-number-400}

Adobe Pass認証：JavaScript 4.0.0

リリース日：**07/05/2018**

## リリースの概要 {#release-overview-400}

* プラットフォーム間で統合されたセキュリティメカニズムである動的なクライアント登録メカニズムのサポートを実装。 プラットフォーム間のセキュリティアクセスの管理は、TVE ダッシュボードを介して、プログラマ自身によって行うことができます。
* すべての機能に対するすべての3rd パーティ Cookieとほぼすべての1st パーティ Cookieの使用を排除します（1st パーティ Cookieがまだ使用されている個人化を除く – 将来的に削除されます）。これにより、3rd パーティ Cookie、またはCookie全般に関する新しいブラウザーポリシーとの互換性と将来性が向上します。 SafariのITP）。 以前の3.x リリースでは、ハッピーフローでは1st パーティ Cookieのみを使用して正常に動作しますが、新しいブラウザーバージョンのリリースで変更される場合があります。
* プリフライト認証チェックのキャッシュを無効にするサポート。
* このバージョンは後方互換性がなく、https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsという新しいパスでホストされます。 この新しいJS SDKのメリットを享受するには、新しい動的クライアント登録メカニズムを使用してweb アプリケーションを登録し、新しいJS SDK URLを指すため、プログラマーから移行プロセスが必要です。

## リリースパッケージ {#release-package-400}

実稼動URLはhttps://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.jsです。

ステージング URL: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
