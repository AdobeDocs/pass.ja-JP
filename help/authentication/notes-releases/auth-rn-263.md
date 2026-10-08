---
title: Adobe Pass Authentication 2.63 リリースノート
description: Adobe Pass Authentication 2.63 リリースノート
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%
---
# Adobe Pass Authentication 2.63 リリースノート {#authn-263-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-263}

* [ビルド番号](#build-number-263)
* [リリースの概要](#release-overview-263)

### ビルド番号 {#build-number-263}

Adobe Pass認証：adobe-pass-**2.63**

リリース日：**09/20/20 - 09/22/2022**

### リリースの概要 {#release-overview-263}

#### プラットフォーム識別メカニズムの改善

* このリリース以降、デバイスの識別に使用するメカニズムが改善され、クライアントサイドの実装に依存しなくなりました。 これにより、プラットフォームレベルでビジネスルールを適用する際の精度が向上し、ESM レポートのトラフィック値をより深く理解できるようになります。

* 新しいESM リリースは近日中にリリースされ、プラットフォーム関連のフィールドを公開する新しい改善されたレポートが追加されます。

* 予定されている変更の詳細については、TAMにお問い合わせください。

#### MVPDの自己劣化

この機能により、MVPDは、各エンドポイントの負荷が高すぎると、高トラフィックシナリオに対して独自の認証エンドポイントと承認エンドポイントを一時的にバイパスする機能を提供します。

#### 認証呼び出しのヘッダーにプロキシ IDを追加します

この機能は、認証呼び出しのヘッダーにSynacor プロキシ MVPDのIDを追加します。 これにより、Synacorはプロキシ化された個別のビジネスルールを設定できます（例： プロキシ化されたMVPDごとに異なるドメインにルーティングします）。

#### TVE ダッシュボード

このリリースでは、MVPD レベルで設定されたauthNまたはauthZ TTLがコンフィギュレーションレポートで正しく計算されない問題を修正しました。

#### JavaScript SDK 4.6.0

* `eval`関数の使用を削除し、SDKをコンテンツセキュリティポリシーに準拠させました。
* パートナーアプリケーションによってブラウザーのローカルストレージが明示的にクリアされたときに、認証フローが正常に終了しない問題を修正しました。
