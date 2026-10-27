---
title: Adobe Pass Authentication 3.10.0 リリースノート
description: Adobe Pass Authentication 3.10.0 リリースノート
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: d60b5256f5809b52122d70d04eb873018707fcaf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Adobe Pass Authentication 3.10.0 リリースノート {#authn-3100-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-3100}

* [ビルド番号](#build-number-3100)
* [リリースの概要](#release-overview-3100)

### ビルド番号 {#build-number-3100}

Adobe Pass認証：adobe-pass-**3.10.0**\
リリース日：**10/27/2026 - 10/29/2026**

### リリースの概要 {#release-overview-3100}

このリリースでは、シングルサインオンの制御、ユーザーメタデータの機能強化、REST API V2およびMVPDの構成管理の信頼性の向上に焦点を当てています。

#### 機能強化

* サービストークンのシングルサインオン（SSO）の統合レベルの制御を追加しました。 サービストークン SSOは、統合に対して明示的に有効になっている場合にのみ使用されます。
* 認証、認証、およびプリフライト中に、カスタムユーザーメタデータ（例：`packageType`）のサポートを追加しました。 REST API V2の認証とプリフライトの決定に、メタデータへのアクセスと暗号化設定を尊重しながら、キャッシュされた応答を含む、設定されたユーザーメタデータが含まれるようになりました。

#### バグ修正

* SAML経由でOAuth2 MVPDを使用する統合で、REST API V2 Apple SSO プロファイルリクエストが無効なSAML応答エラーで失敗する問題を修正しました。
* 同じリソースのプリフライトと認証の決定が、互いのキャッシュされた有効期限とユーザーメタデータを上書きする可能性がある問題を修正しました。
* プロキシ化されたMVPDを削除すると、後続の設定の保存を妨げる参照されていないプラットフォーム設定が残る問題を修正しました。
