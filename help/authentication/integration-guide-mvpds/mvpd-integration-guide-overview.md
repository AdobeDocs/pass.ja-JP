---
title: MVPD統合ガイド
description: MVPD統合ガイド
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '1265'
ht-degree: 0%

---

# MVPD統合ガイド {#mvpd-integration-guide}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

この統合ガイドは、Adobe® パス認証との統合を計画しているマルチチャンネル・ビデオ・プログラミング・ディストリビューター（MVPD）向けです。

TV Everywhere （TVE）は、有料テレビ業界における革新的な取り組みです。これにより、加入者は、自宅や外出先を問わず、複数のデバイスをまたいで既に支払っているコンテンツにアクセスできます。 TVE は、有料テレビ事業者に対して、既存の顧客関係を強化し、新しい顧客への扉を開く重要な機会を提供しています。 しかし、こうした機会には課題も伴います。

TVE エコシステムでは、**プログラマー** がコンテンツを提供し、**MVPD** （マルチチャネルビデオプログラミングディストリビューター）が、ビューアが適格なサブスクライバーかどうかを確認するために必要な顧客データを管理します。 1 人のプログラマーで認証と承認を調整するのは管理可能な場合もありますが、数十人または数百人のプログラマーで調整すると、かなり複雑になります。

ここで、**認証® パスAdobe** によりプロセスが簡略化されます。 MVPD は、TVE エコシステム全体へのアクセスを取得するために、Adobe Passとの単一の合理化された統合を実装するだけで済みます。 提供される統合フレームワークは、市場投入までの時間を短縮し、不正を軽減するための安全な環境を提供します。また、複数のプラットフォームにわたってより多くの TV コンテンツを配信することで、顧客体験を向上させます。

## あらゆる場所での TV 用Adobe Pass認証 {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass Authentication は、マルチチャネルビデオプログラミングディストリビューター（MVPD）とプログラマーのビジネスルールを遵守し、バックエンド（サーバー間）の迅速な統合を可能にする SaaS （Software as a Service）ソリューションとして動作します。

### 標準とプロトコル {#standards-protocols}

Adobe Pass Authentication は、TV Everywhere （TVE）の新しい標準およびプロトコルと完全に連携しており、エコシステム全体でのシームレスな統合とコンプライアンスをサポートします。

* **CableLabs OLCA （Online Content Access）仕様**\
  Adobe Pass Authentication は CableLabs OLCA 仕様に準拠しており、オンラインソースから有料テレビのお客様にビデオコンテンツを配信するための技術要件とアーキテクチャを定義しています。 Adobeは、2011 年 6 月に CableLabs の相互運用テスト プロジェクトに積極的に参加し、サービス プロバイダの実装に関するテスト プロセスを成功に導きました。

Adobe Pass認証は複数のプロトコル（SAML、OAuth 2.0 など）をサポートするように設計されており、この柔軟性により、進化するニーズに基づいて、カスタムプロトコルを含む将来の拡張にも対応できます。

ほとんどの統合では、認証の主要な標準である SAML （Security Assertion Markup Language）プロトコルが使用されています。 Adobe Pass Authentication は、SAML フレームワーク内のプロキシサービスプロバイダーとして機能し、SAML 認証応答をAdobe共通ドメインのセキュアトークンとして保持します。

つまり、Adobe Pass認証は、プロトコルに依存せず、OLCA 標準に密接に合致するように設計されています。 これらの規格は、プログラマ、MVPD、およびサービスプロバイダに共通のフレームワークを確立し、TVE 機能を実装するための統一されたアプローチを保証します。

### の統合とサポート {#integration-support}

Adobe Pass Authentication は、MVPD テクニカルチームと協力して、次のように特定の要件に合わせて統合を設定します。

* **標準の統合**\
  ドキュメントや基本的なメールサポートなど、無料で提供されます。

* **サポートの強化**\
  重要なカスタマイズまたは迅速なタイムラインの場合、サポート料金が適用される場合があります。

Adobe Pass認証では、次のようなMVPD ビジネスロジックの効率的な処理をサポートしています。

* **自己完結型ビジネスロジック**\
  認証リクエスト時にMVPDによって完全に適用されるビジネスロジックの場合、Adobeは必要なデータを提供します。 このデータには、一意のデバイス ID と、リクエストを送信するユーザーのデバイスの IP アドレスを含めることができますが、これらに限定されません。

* **カスタムプロパティのサポート**\
  Adobeの操作やユーザー固有の処理が必要なビジネスロジックの場合、Adobeでは、各MVPDのカスタム設定を管理できます。 これらのポリシーにより、使用権限ワークフローの特定の時点でトリガーできる事前定義されたワークフローが有効になります。 詳しくは、Adobe担当者にお問い合わせください。

## 使用権限フロー {#entitlement-flow}

使用権限フローは、保護されたコンテンツをストリーミングするために、プログラマー（TVE）アプリケーションが完了する必要がある一連の手順です。 このフローには、MVPDとのインタラクションに関連するいくつかのフェーズが含まれています。

* [認証フェーズ](#authentication-phase)
* （オプション）事前認証フェーズ
* [承認フェーズ](#authorization-phase)
* ログアウトフェーズ

ユーザーがプログラマー（TVE）アプリケーションに最初に訪問すると、使用権限フローは概要を説明したシーケンスに従います。 ただし、その後の訪問では、アプリケーションは、認証のステータスと該当する表示ポリシーに基づいて、特定の手順を省略する場合があります。

>[!NOTE]
>
> このドキュメントでは、Adobe Pass Authentication がサポートする各種プラットフォーム（ブラウザー、モバイルデバイス、TV に接続されたデバイスなど）で動作するアプリケーションの種類を総称してプログラマー（TVE）アプリケーションと呼びます。

### 認証フェーズ {#authentication-phase}

次の手順は、SAML 統合が発生した場合の高レベルの手順の概要を示しています。

1. **プログラマーのアプリケーション（Web サイト）の読み込み**\
   ユーザーがプログラマーのアプリケーション（web サイト）に移動すると、Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) が統合されます。

1. **保護されたコンテンツの要求**\
   ユーザーが保護されたコンテンツにアクセスしようとすると、プログラマーのアプリケーションはユーザーが選択できる MVPD のリストを表示します。

1. **認証リクエストの初期化**\
   MVPDを選択すると、ユーザーはAdobe Pass Authentication Server にリダイレクトされます。 ここでは、SAML 統合の場合、選択したMVPDに対して、暗号化された SAML 認証リクエストが生成されます。 このリクエストは、プログラマーの代わりにMVPDに送信されます。 MVPDのシステムに応じて、ユーザーのブラウザーがMVPDのログインページにリダイレクトされるか、ログイン iFrame がプログラマーのアプリケーション内に埋め込まれます。

1. **MVPD ログイン**\
   MVPDはリクエストを受け入れ、リダイレクトまたは iFrame 経由で、ログインインターフェイスを表示します。

1. **ユーザーのログインと検証**\
   ユーザーは、MVPD資格情報を使用してログインします。 MVPDは、ユーザーの購読ステータスを検証し、独自の HTTP セッションを確立します。

1. **Adobe Pass認証に対するMVPDの応答**\
   検証が完了すると、MVPDは SAML 応答（暗号化）を生成し、Adobe Pass Authentication に返します。

1. **プロファイルの生成**\
   Adobe Pass Authentication は、SAML 応答を検証し、キャッシュされるユーザープロファイルを生成して、プログラマーのアプリケーション（web サイト）にユーザーをリダイレクトします。

### 承認フェーズ {#authorization-phase}

**手順の概要**

次の手順は、大まかな手順の概要を示しています。

1. **リソース識別子の処理**\
   保護されたコンテンツは、単純な文字列またはより複雑な構造の [ リソース識別子 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) によって識別されます。 この ID は、プログラマーとMVPDによって事前に定義され、同意されています。 プログラマーのアプリケーションがリソース ID をAdobe Pass認証 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) に送信します。

1. **MVPD認証チェック**\
   Adobe Pass Authentication Server は、標準化されたプロトコルを使用してMVPD認証エンドポイントと通信します。

1. **Adobe Pass認証に対するMVPDの応答**\
   検証が完了すると、MVPDはユーザーにコンテンツにアクセスする権限があることを確認し、応答をAdobe Pass Authentication に返します。

1. **決定およびメディアトークンの生成**\
   Adobe Pass Authentication は、応答を確認し、キャッシュされる [ 決定 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) を生成し、メディアトークンを含む決定をプログラマーのアプリケーション（web サイト）に返します。

1. **コンテンツアクセスの検証**\
   プログラマーのアプリケーションは、[ メディアトークン検証子 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) を使用して、正しいユーザーが正しいコンテンツにアクセスしていることを確認します。 検証が完了すると、保護されたコンテンツを表示するアクセス権がユーザーに付与されます。

## 使用権限について {#understanding-entitlements}

Adobe Pass認証ソリューションでは、使用権限（認証および承認ワークフローが正常に完了すると生成される特定のデータ）の作成を中心に取り組んでいます。 これらの権限は、保護されたコンテンツへのアクセス権を付与しますが、存続期間が限られています。 使用権限の有効期限が切れたら、認証または承認プロセスを再開して、使用権限を更新する必要があります。

使用権限について詳しくは、次のドキュメントを参照してください。

* **プロファイル**

  認証に成功すると、Adobe Pass Authentication は、要求元のアプリケーション、デバイス、サービスプロバイダーの識別子（要求者識別子）に関連付けられた認証済みプロファイル（「長期間有効」）を作成します。

* **[ユーザーメタデータ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Adobe Pass認証は、認証に成功すると（場合によっては認証後も）、MVPDからユーザーメタデータを受け取り、要求元のアプリケーションに公開できます。

* **[決定](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Adobe Pass Authentication は、認証に成功すると、要求元のアプリケーション、デバイス、サービスプロバイダーの識別子（要求者識別子）および保護された特定のリソース（リソース識別子）に関連付けられた認証決定（「長期間有効」）を作成します。

* **[メディアトークン](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  認証に成功すると、Adobe Pass認証は、成功した再生リクエストに関連付けられたメディアトークン（「短期間有効」）を作成します。
