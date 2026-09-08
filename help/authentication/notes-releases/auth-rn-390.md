---
title: Adobe Pass Authentication 3.9.0 リリースノート
description: Adobe Pass Authentication 3.9.0 リリースノート
source-git-commit: 7ec140485418d07e16a181d43b651ea6de331477
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Adobe Pass Authentication 3.9.0 リリースノート {#authn-390-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-390}

* [ビルド番号](#build-number-390)
* [リリースの概要](#release-overview-390)

### ビルド番号 {#build-number-390}

Adobe Pass認証：adobe-pass-**3.9.0.1**\
リリース日：**09/08/2026 - 09/10/2026**

### リリースの概要 {#release-overview-390}

このリリースでは、REST API V2とESM指標の改善に焦点を当てています。

#### 機能強化

* OAuth2で設定されたMVPDに対して有効な認証要求が返されるように、REST API V2 パートナーのシングルサインオンを改善しました。
* 認証が失敗した場合に、空の応答ではなく明確なエラー応答を返すようにREST API V2決定を改善しました。
* 登録コードの生成を改善し、文字が視覚的に曖昧になるのを防ぎ、コードの読み取りと入力を正しくしました。
* プリフライト AuthZ メトリクスのサポートによるESM ダッシュボードの機能強化。
