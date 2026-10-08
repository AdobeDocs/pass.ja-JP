---
title: MVPD統合ガイド
description: MVPD統合ガイド
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1326'
ht-degree: 0%
---
# MVPD統合ガイド {#mvpd-integration-guide}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

この統合ガイドは、Adobe® Pass Authenticationとの統合を計画しているマルチチャネルビデオプログラミングディストリビューター（MVPD）を対象としています。

TV Everywhere （TVE）は、有料テレビ業界における革新的な取り組みであり、加入者が自宅でも外出先でも、複数のデバイスをまたいで既に支払っているコンテンツにアクセスすることを可能にします。 有料テレビ事業者にとって、TVEは既存の顧客関係を強化し、新たな顧客関係を開く大きなチャンスを提供します。 しかし、これらのチャンスには課題があります。

TVE エコシステムでは、**プログラマー**&#x200B;がコンテンツを提供し、**MVPD** （マルチチャネル ビデオ プログラミング ディストリビューター）が、視聴者が適格なサブスクライバーであるかどうかを確認するために必要な顧客データを管理します。 1人のプログラマーで認証と認証を調整することは管理可能かもしれませんが、何十人、あるいは何百人ものプログラマーで管理することは、かなり複雑になります。

ここでは、**Adobe® Pass Authentication**&#x200B;がプロセスを簡素化します。 MVPDは、Adobe Passとの統合を合理化するだけで、TVEのエコシステム全体にアクセスできるようになります。 提供される統合フレームワークは、市場投入までの時間を短縮し、不正行為を軽減する安全な環境を提供します。また、複数のプラットフォームをまたいでより多くのテレビコンテンツを配信することで、顧客体験を向上させます。

## TV Everywhere向けAdobe Pass認証 {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass Authenticationは、バックエンド（サーバー間）の迅速な統合を可能にするために設計されたSaaS （Software as a Service）ソリューションとして動作し、マルチチャネルビデオプログラミングディストリビューター（MVPD）とプログラマーの両方のビジネスルールを遵守します。

### 基準とプロトコル {#standards-protocols}

Adobe Pass Authenticationは、TV Everywhere （TVE）の新しい標準とプロトコルに完全に適合しており、エコシステム全体でシームレスな統合とコンプライアンスをサポートします。

* **CableLabs OLCA （オンラインコンテンツアクセス）仕様**\
  Adobe Pass Authenticationは、CableLabs OLCA仕様に準拠しています。この仕様は、オンライン ソースから有料テレビのお客様にビデオ コンテンツを配信するための技術要件とアーキテクチャを定義しています。 Adobeは、2011年6月にCableLabs相互運用性テストプロジェクトに積極的に参加し、サービスプロバイダー実装のテストプロセスに成功しました。

Adobe Pass Authenticationは、複数のプロトコル（SAML、OAuth 2.0など）をサポートするように設計されており、進化するニーズに基づいて、カスタムプロトコルを含む将来の拡張を可能にします。

ほとんどの統合では、認証の主要な標準であるSAML （Security Assertion Markup Language）プロトコルを使用しています。 Adobe Pass認証は、SAML フレームワーク内のプロキシサービスプロバイダーとして機能し、SAML認証応答をAdobe共通ドメインのセキュアトークンとして保持します。

結論として、Adobe Pass Authenticationはプロトコルに依存せず、OLCA標準と密接に連携するように設計されています。 これらの標準は、プログラマー、MVPD、およびサービスプロバイダーに対して共通のフレームワークを確立し、TVE機能を実装するための一貫したアプローチを確保します。

### 統合とサポート {#integration-support}

Adobe Pass Authenticationは、MVPDの技術チームと連携して、次のように、それぞれの要件に合わせて統合機能を設定します。

* **標準統合**\
  ドキュメントや基本的なメールサポートなど、無料で利用できます。

* **強化サポート**\
  大規模なカスタマイズや迅速なスケジュールには、サポート料金が適用される場合があります。

Adobe Pass Authenticationは、次のように、MVPD ビジネスロジックの効率的な処理をサポートします。

* **自己完結型ビジネス ロジック**\
  承認要求中にMVPDによって完全に適用されるビジネスロジックについては、Adobeが必要なデータを提供します。 このデータには、一意のデバイス IDと、リクエストを行うユーザーのデバイスのIP アドレスが含まれますが、これらに限定されません。

* **カスタムプロパティのサポート**\
  ユーザーの介入やAdobe固有の処理を必要とするビジネスロジックの場合、Adobeは各MVPDのカスタム設定を管理できます。 これらのポリシーにより、使用権限ワークフローの特定のポイントでトリガーできる、事前定義済みのワークフローが有効になります。 詳しくは、Adobe担当者にお問い合わせください。

## 使用権限のフロー {#entitlement-flow}

エンタイトルメントフローは、保護されたコンテンツをストリーミングするためにプログラマー（TVE）アプリケーションが完了する必要がある一連の手順です。 このフローには、MVPDとのやり取りを含む複数のフェーズが含まれます。

* [認証フェーズ](#authentication-phase)
* （オプション）事前認証フェーズ
* [承認フェーズ](#authorization-phase)
* ログアウトフェーズ

ユーザーがプログラマー（TVE）アプリケーションに初めてアクセスすると、エンタイトルメントフローは概説されたシーケンスに従います。 ただし、その後の訪問では、認証のステータスと該当する視聴ポリシーに基づいて、アプリケーションが特定の手順をバイパスする場合があります。

>[!NOTE]
>
> このドキュメントでは、プログラマ（TVE）アプリケーションを使用して、様々なプラットフォーム（ブラウザー、モバイルデバイス、テレビ接続デバイスなど）で実行されるアプリケーションの種類をまとめて示します。 Adobe Pass Authenticationによるサポート。

### 認証フェーズ {#authentication-phase}

次の手順は、SAML統合の場合の大まかな手順の概要です。

1. **プログラマーのアプリケーション （Web サイト）の読み込み**\
   Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)を統合するプログラマーのアプリケーション（web サイト）に移動します。

1. **保護されたコンテンツ要求**\
   ユーザーが保護されたコンテンツにアクセスしようとすると、プログラマーのアプリケーションに、ユーザーが選択できるMVPDのリストが表示されます。

1. **認証要求の初期化**\
   MVPDを選択すると、ユーザーはAdobe Pass認証サーバーにリダイレクトされます。 ここでは、SAML統合の場合、選択したMVPDに対する暗号化されたSAML認証リクエストが生成されます。 このリクエストは、プログラマの代理としてMVPDに送信されます。 MVPDのシステムによっては、ユーザーのブラウザーがMVPDのログインページにリダイレクトされるか、ログイン iFrameがプログラマーのアプリケーション内に埋め込まれます。

1. **MVPD ログイン**\
   MVPDはリクエストを受け入れ、リダイレクトまたはiFrameを使用してログインインターフェイスを表示します。

1. **ユーザーのログインと検証**\
   ユーザーはMVPDの資格情報でログインします。 MVPDは、ユーザーのサブスクリプションステータスを検証し、独自のHTTP セッションを確立します。

1. **Adobe Pass認証に対するMVPDの応答**\
   検証が完了すると、MVPDはSAML応答（暗号化）を生成し、Adobe Pass Authenticationに返します。

1. **プロファイル生成**\
   Adobe Pass認証は、SAML応答を検証し、キャッシュされるユーザープロファイルを生成し、ユーザーをプログラマーアプリケーション（web サイト）にリダイレクトします。

### 承認フェーズ {#authorization-phase}

**高度な手順**

次の手順は、大まかな手順の概要を示しています。

1. **リソース Idの処理**\
   保護されたコンテンツは、[ リソース識別子](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)によって識別されます。これは、単純な文字列または複雑な構造である可能性があります。 このIDは、プログラマとMVPDによって事前に定義され、合意されています。 プログラマーのアプリケーションは、リソース IDをAdobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)に送信します。

1. **MVPD認証チェック**\
   Adobe Pass Authentication Serverは、標準化されたプロトコルを使用してMVPDの認証エンドポイントと通信します。

1. **Adobe Pass認証に対するMVPDの応答**\
   検証が完了すると、MVPDは、ユーザーがコンテンツにアクセスする権限を持っているかどうかを確認し、Adobe Pass Authenticationに返信を送信します。

1. **決定およびメディアトークンの生成**\
   Adobe Pass認証は、応答を検証し、キャッシュされる[decision](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)を生成し、メディアトークンを含む決定をプログラマーのアプリケーション（web サイト）に返します。

1. **コンテンツアクセスの確認**\
   プログラマーのアプリケーションは、[Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)を使用して、正しいユーザーが正しいコンテンツにアクセスしていることを確認します。 検証が完了すると、保護されたコンテンツを表示するためのアクセス権がユーザーに付与されます。

## 使用権限について {#understanding-entitlements}

Adobe Passの認証ソリューションでは、認証ワークフローと承認ワークフローが正常に完了したときに生成される、特定のデータであるエンタイトルメントの作成を中心に展開します。 これらの使用権限は、保護されたコンテンツへのアクセスを許可しますが、有効期間は限られています。 使用権限が期限切れになると、認証または承認プロセスを再度開始して更新する必要があります。

使用権限について詳しくは、次のドキュメントを参照してください。

* **プロファイル**

  認証に成功すると、Adobe Pass Authenticationは、リクエスト側のアプリケーション、デバイス、サービスプロバイダーの識別子（依頼者識別子）に関連付けられた認証済みプロファイル（「長期間有効」）を作成します。

* **[ユーザーメタデータ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  認証が成功すると（場合によっては認証後も）、Adobe Pass AuthenticationはMVPDからユーザーメタデータを受け取り、リクエスト側のアプリケーションに公開できます。

* **[決定](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  認証が成功すると、Adobe Pass Authenticationは、リクエスト側のアプリケーション、デバイス、サービスプロバイダー識別子（依頼者識別子）および特定の保護されたリソース（リソース識別子）に関連付けられた認証決定（「長期間有効」）を作成します。

* **[メディアトークン](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  認証が成功すると、Adobe Pass Authenticationは、成功した再生リクエストに関連付けられたメディアトークン（「短期間有効」）を作成し、不正を軽減するための業界のベストプラクティス（ストリームリッピングなど）をサポートします。

プロファイルと決定の有効期間（「TTL」）値は、プログラマーと有料テレビ事業者の間の合意に基づいて設定され、関係者全員に最もサービスを提供する価値に同意します。
