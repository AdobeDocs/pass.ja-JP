---
title: Adobe Pass Authentication 3.7.0 リリースノート
description: Adobe Pass Authentication 3.7.0 リリースノート
source-git-commit: 89b5fbd8e8510cbf84ce7908e8cf86551e7a0cb9
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication 3.7.0 リリースノート {#authn-370-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-370}

* [ビルド番号](#build-number-370)
* [リリースの概要](#release-overview-370)

### ビルド番号 {#build-number-370}

Adobe Pass認証：adobe-pass-**3.7.0.2**\
リリース日：**05/12/2026 - 05/14/2026**

### リリースの概要 {#release-overview-370}

このリリースでは、MVPD統合のアップグレード、バグ修正、TVE ダッシュボードの機能強化に焦点を当てています。

#### MVPDとの連携

* OAuth2 ベースのMVPD認証のPKCE サポートを追加しました。

#### 機能強化

* TVE Dashboard バージョン 1.5.1をリリースしました。

#### バグ修正

* Apple SSOが特定のMVPD設定の不一致を見落とす問題を修正しました。
* 応答ヘッダーの文字が無効なため、承認拒否でHTTP 500 エラーが発生する問題を修正しました。
