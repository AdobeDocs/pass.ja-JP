---
title: プログラマー統合ガイド
description: プログラマー統合ガイド
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# プログラマー統合ガイド {#programmer-integration-guide}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

この統合ガイドは、Adobe® パス認証との統合を計画しているコンテンツプロバイダー（プログラマー）を対象としています。

今日のデジタル環境では、視聴者は、いつでもどこでもインターネットにアクセスし、保護されたコンテンツへのアクセスをリクエストできます。 彼らは、1 回限りのイベントを見たり、放送しているテレビシリーズ全体をストリーミングする権利を求めたりする可能性があります。

保護されたコンテンツへのアクセスを許可する前に、閲覧者がそのコンテンツへの権限を持っているかどうかを判断する必要があります。 主な質問は次のとおりです。

* **ビューアには、マルチチャネルビデオプログラミング配信者（MVPD）とのアクティブなサブスクリプションがありますか？**
* **このサブスクリプションには、プログラミングが含まれていますか？**

## あらゆる場所での TV 用Adobe Pass認証 {#adobe-pass-authentication-for-tv-everywhere}

プログラマーにとって、使用権限の決定は必ずしも簡単ではありません。 MVPD は、顧客の識別データとアクセス権限の管理者です。 さらに複雑なことに、プログラマーの視聴者は多種多様な MVPD を購読することができ、それぞれが独自のシステムで動作します。 これらの複雑さにより、資格の検証が技術的に難しくなり、リソースを大量に消費することになります。

![ プログラマーが直接決定するユーザー使用権限 ](../assets/user-ent-by-progr.png){align="center"}

*プログラマーが直接決定するユーザー使用権限*

Adobe Pass Authentication を使用すると、プログラマーと MVPD 間の権利付与トランザクションを安全かつ迅速に実行でき、保護されたコンテンツを対象となるビューアに安全かつ簡単に提供できます。

![Adobe Pass Authentication を介したユーザーエンタイトルメント ](../assets/user-ent-mediatedby-authn.png){align="center"}

*Adobe Pass Authentication を介したユーザーエンタイトルメント*

Adobe Pass認証はプロキシとして機能し、プログラマーと MVPD の両方に安全で一貫性のあるインターフェイスを提供することで、両者の間の使用権限のフローを容易にします。

プログラマーの場合、Adobe Pass認証では、次のような API を **Standard** 層または **Premium** 層の一部として提供しています。

* 標準Adobe Pass認証 API:
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass認証 API:
   * [Temp Pass API をリセット](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass フィーチャ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [API の低下](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [縮退機能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [使用権限サービスモニタリング API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### ユースケース {#use-cases}

この節では、Adobe Pass認証でサポートされるプログラマー統合の使用例の概要を説明します。

* 単一チャネルネットワークを持つプログラマー（TVE）アプリケーション

  これにより、プログラマーは、TVE アプリケーション内の単一ブランドのチャネルネットワークからコンテンツへのアクセスを視聴者に提供できます。

* 複数のチャネルネットワークを持つプログラマー（TVE）アプリケーション

  これにより、プログラマーは、単一の TVE アプリケーション内の複数のチャネルネットワークからコンテンツへのアクセスを視聴者に提供できます。

* 特別なイベントのためのプログラマー（TVE）アプリケーション

  これにより、プログラマーは、通常のチャネルのように、MVPDのエンタイトルメントデータベース内のリソースではない可能性のある特別なイベントのコンテンツへのアクセスをビューアに提供できます。

| **フェーズ** | **優先度** | **ユースケース** | **書類** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **認証** | **高** | 認証 | 詳しくは、「認証フェーズ [ の節で集計したドキュメントを参照し ](#authentication-phase) ください。 |
|                      | **高** | ホームベースの認証（HBA） | 詳しくは、[ ホームベースの認証 ](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) を参照してください。 |
|                      | **高** | シングルサインオン（SSO） | 詳しくは、[ シングルサインオン（SSO） ](#sso) の節で集計したドキュメントを参照してください。 |
|                      | **高** | MVPDを選択 | 詳しくは、「設定フェーズ [ の節で集約されたドキュメントを参照し ](#configuration-phase) ください。 |
|                      | **Medium** | ブランド MVPDのログインページ | MVPD が、既定の言語設定のサポートなど、プログラマまたはサービス プロバイダに固有のブランドを使用してログイン ページを提供できるようにします。 |
|                      | **高** | プラットフォームごとの Time-To-Live （TTL）値の設定 | 詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) を参照してください。 |
| **事前認証** | **低** | 事前認証（プリフライト認証） | 詳しくは、「事前認証フェーズ [ の節で集約されたドキュメントを参照し ](#preauthorization-phase) ください。 |
|                      | **Medium** | 拡張エラーコード | 詳しくは、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を参照してください。 |
| **認可** | **高** | 認証 | 詳しくは、「承認フェーズ [ の節で集約されたドキュメントを参照し ](#authorization-phase) ください。 |
|                      | **高** | ユニークチャネル認証 | ユーザーが単一の TVE アプリケーション内の複数のチャネル ネットワークからコンテンツにアクセスできるようにします。 プログラマーは、チャネル固有の認証呼び出しを行って、使用権限を確認できます。 |
|                      | **低** | アセットレベルの認証 | MVPD が認証中に個々のコンテンツアセットの詳細な分析を収集できるようにします。 |
|                      | **Medium** | 拡張エラーコード | 詳しくは、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を参照してください。 |
|                      | **高** | プログラマー向けフェデレーテッド プレーヤー – ページレベルの認証を使用 | 詳しくは、[ メディアトークン ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) を参照してください。 |
|                      | **Medium** | プログラマー向けフェデレーテッド プレーヤー – 内部プレーヤー認証付き | 詳しくは、[ メディアトークン ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) を参照してください。 |
|                      | **高** | シンジケートプレーヤー – ページレベルの認証を使用してMVPD ポータルでホスト | 詳しくは、[ メディアトークン ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) を参照してください。 |
|                      | **低** | ペアレンタルコントロール – 認証リクエストのコンテンツ評価 | プログラマーが、アセットレベルの認証に役立つコンテンツ評価をMVPDへの認証リクエストの一部として含めることができるようにします。 |
|                      | **低** | ペアレンタルコントロール – ユーザー属性に基づくコンテンツのフィルタリング | プログラマーが、ユーザーに許可されているコンテンツの最大評価を確認し、それに応じて使用可能なコンテンツをフィルタリングできるようにします。 |
| **ログアウト** | **Medium** | ログアウト | 詳しくは、「ログアウトフェーズ [ の節で集計したドキュメントを参照し ](#logout-phase) ください。 |

## 使用権限フロー {#entitlement-flow}

使用権限フローは、保護されたコンテンツをストリーミングするために、プログラマー（TVE）アプリケーションが完了する必要がある一連の手順です。 このフローは、次のフェーズで構成されます。

* [登録フェーズ](#registration-phase)
* [設定フェーズ](#configuration-phase)
* [認証フェーズ](#authentication-phase)
* [（オプション）事前認証フェーズ](#preauthorization-phase)
* [承認フェーズ](#authorization-phase)
* [ログアウトフェーズ](#logout-phase)

ユーザーがプログラマー（TVE）アプリケーションに最初に訪問すると、使用権限フローは概要を説明したシーケンスに従います。 ただし、その後の訪問では、アプリケーションは、登録または認証のステータスと該当する閲覧ポリシーに基づいて、特定の手順を省略する場合があります。

使用権限のフローとそのフェーズについて詳しくは、引き続きこのドキュメントを参照し、付属のクックブックガイドを参照して追加のインサイトを確認します。

* [REST API V2 クックブック（クライアントからサーバー）](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [REST API V2 クックブック（サーバー間）](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> このドキュメントでは、Adobe Pass Authentication がサポートする各種プラットフォーム（ブラウザー、モバイルデバイス、TV に接続されたデバイスなど）で動作するアプリケーションの種類を総称してプログラマー（TVE）アプリケーションと呼びます。

### 登録フェーズ {#registration-phase}

登録フェーズの目的は、[Dynamic Client Registration （DCR） ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) プロセスを通じて、Adobe Pass Authentication に対してクライアントアプリケーションを登録することです。

動的クライアント登録（DCR）プロセスでは、クライアントアプリケーションがクライアント資格情報のペアを取得し、登録フェーズの最終目標としてアクセストークンを取得する必要があります。

**API**

* [クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**フロー**

* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**よくある質問**

* [ 登録フェーズに関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general)。

### 設定フェーズ {#configuration-phase}

設定フェーズの目的は、各MVPDに対してAdobe Pass Authentication によって保存された設定の詳細と共に、アクティブに統合される MVPD のリストをクライアントアプリケーションに提供することです。

設定フェーズは、クライアントアプリケーションがユーザーに TV プロバイダーの選択を依頼する必要がある場合に、認証フェーズの前提条件の手順として機能します。

**API**

* [特定のサービスプロバイダーの設定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**よくある質問**

* [ 設定フェーズに関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general)。

>[!TIP]
>
> TVE アプリには、MVPDのセレクションインターフェイスが含まれているので、テレビ事業者を簡単に特定して選択できます。

### 認証フェーズ {#authentication-phase}

認証フェーズの目的は、クライアントアプリケーションがMVPDを使用してユーザーの ID を検証し、ユーザーメタデータ情報を取得できるようにすることです。

認証フェーズは、クライアントアプリケーションでコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件の手順として機能します。

認証が成功すると、アプリケーション、デバイス、サービスプロバイダーに結び付けられたプロファイルが生成され、その中にはユーザーメタデータ情報も含まれています。

**手順の概要**

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

**API**

* [認証セッションの作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッションの再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [認証セッションの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [ユーザーエージェントでの認証の実行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の mvpd のプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**フロー**

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリ・アプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**よくある質問**

* [認証フェーズの FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

>[!TIP]
>
> 例えば、保護されたコンテンツのアクセシビリティを示す「ロック済み」アイコンや「ロック解除」アイコンと共にMVPD ロゴを表示するなどして、TVE アプリケーションでユーザーの認証ステータスを明確に伝える必要があります。

#### シングルサインオン（SSO） {#single-sign-on}

**API**

* [パートナー認証要求の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [パートナー認証応答を使用したプロファイルの作成と取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**フロー**

* [パートナーフローを使用したシングルサインオン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [プラットフォーム ID フローを使用したシングルサインオン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [サービストークンフローを使用したシングルサインオン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### （オプション）事前認証フェーズ {#preauthorization-phase}

事前認証フェーズの目的は、ユーザーがアクセスする権利を持つカタログからリソースのサブセットをクライアントアプリケーションに提示できるようにすることです。

事前認証フェーズでは、ユーザーが初めてクライアントアプリケーションを開いた場合や、新しいセクションに移動した場合のユーザーエクスペリエンスが向上します。

**API**

* [事前認証の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**よくある質問**

* [事前認証フェーズに関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

>[!TIP]
>
> TVE アプリケーションでは、制限付きコンテンツの「ロック」アイコンや承認済みコンテンツの「ロック解除」アイコンなどの視覚的なインジケーターを使用して、制限付きコンテンツと承認済みコンテンツを明確に区別する必要があります。

### 承認フェーズ {#authorization-phase}

認証フェーズの目的は、ユーザーがMVPDで権限を検証した後でリクエストしたリソースをクライアントアプリケーションで再生できるようにすることです。

認証が成功すると、決定が生成されます。この決定には、セキュリティ上の目的でプログラマー（TVE）アプリケーションに提供されるメディアトークンも含まれます。

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

**API**

* [承認の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**よくある質問**

* [承認フェーズに関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

>[!TIP]
>
> TVE アプリケーションでは、制限付きコンテンツの「ロック」アイコンや承認済みコンテンツの「ロック解除」アイコンなどの視覚的なインジケーターを使用して、制限付きコンテンツと承認済みコンテンツを明確に区別する必要があります。

### ログアウトフェーズ {#logout-phase}

ログアウトフェーズの目的は、クライアントアプリケーションが、ユーザーのリクエストに応じてAdobe Pass Authentication 内でユーザーの認証済みプロファイルを終了できるようにすることです。

**API**

* [特定の mvpd に対するログアウトの開始](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的なログアウトフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**よくある質問**

* [ログアウトフェーズの FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

#### シングル ログアウト （SLO） {#single-logout}

**フロー**

* [シングルログアウトフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

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

  認証に成功すると、Adobe Pass Authentication によって、成功した再生リクエストに関連付けられたメディアトークン（「短期間有効」）が作成され、不正を軽減するための業界のベストプラクティス（ストリームリッピングなど）のサポートが提供されます。

プロファイルと決定の有効期間（「TTL」）値は、関係するすべてのユーザーに最適な値に同意するプログラマーと有料テレビプロバイダーの契約に基づいて設定されます。
