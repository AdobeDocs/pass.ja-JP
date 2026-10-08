---
title: REST API V2用語集
description: REST API V2用語集
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1762'
ht-degree: 0%
---
# REST API V2用語集 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このドキュメントでは、Adobe Pass Authentication REST API V2を統合する際に使用される用語の定義について説明します。

>[!MORELIKETHIS]
>
> * [動的クライアント登録（DCR）用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## 用語集 {#glossary-terms}

### A {#a}

#### 認証 {#authentication}

認証とは、[MVPD](#mvpd)でユーザーサブスクリプションを検証した後、保護されたコンテンツ（[resource](#resource)）へのアクセスを取得するために、[&#x200B; プログラマー](#programmer)に対してIDを証明できるようにするプロセスです。

#### 認証コード {#code}

認証コードは、ユーザーが[認証](#authentication) プロセスを開始したときに生成された一意の値を格納し、認証プロセスが完了するまでユーザーの[認証セッション &#x200B;](#session)を一意に識別するAdobe Pass認証の概念です。

この認証コードは、[プライマリ（プログラマー）アプリケーション &#x200B;](#primary-application)または[セカンダリ（プログラマー）アプリケーション &#x200B;](#secondary-application)の両方で使用して、[認証](#authentication) プロセスを完了したり、[認証セッション &#x200B;](#session)に関する情報を取得したり、ユーザー[&#x200B; プロファイル &#x200B;](#profile)にアクセスしたりできます。

以前の用語の使用された登録コードと同義語。

#### 認証セッション {#session}

認証セッションは、[&#x200B; プログラマー](#programmer) アプリケーションから開始（または継続）されたユーザーの認証プロセスに関する情報を保存するAdobe Pass認証の概念であり、[認証コード &#x200B;](#code)によって一意に識別されます。

認証セッションは、ユーザーが既に認証されている場合に備えて、[使用権限](#entitlement) フローの次のフェーズとして[認証](#authorization) プロセスを進める[&#x200B; プログラマー](#programmer) アプリケーションを示すこともできます。

#### 認証 {#authorization}

認証とは、[MVPD](#mvpd)でユーザー権限を検証した後、所有している[MVPD](#mvpd) サブスクリプションに基づいて[&#x200B; プログラマー](#programmer) カタログから保護されたコンテンツ （[resource](#resource)）にアクセスできるようにするプロセスです。

### C {#c}

#### 設定 {#configuration}

この設定は、[Programmer](#programmer)および[Adobe Pass](#mvpd)の統合設定に関する情報を保存するMVPD認証の概念であり、アクティブな統合のリストから[TV プロバイダー](#tv-provider)を選択するようにユーザーに依頼する[認証](#authentication) プロセス中に使用できます。

### D {#d}

#### 決定 {#decision}

この決定は、[MVPD](#mvpd) [認証](#authorization)または[事前認証](#preauthorization) プロセスの問い合わせに関する情報を保存するAdobe Pass認証の概念であり、[&#x200B; プログラマー](#programmer)の保護されたコンテンツへのユーザーアクセスを許可または拒否します。

#### 劣化 {#degradation}

この低下は、[Adobe Pass](#mvpd)でサービスの中断が発生した場合でも、保護されたコンテンツにアクセスできるMVPD認証機能です。

詳しくは、[&#x200B; デグラデーション機能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) ドキュメントを参照してください。

#### デバイス ID {#device-id}

デバイス IDは、ユーザーのデバイスにバインドされた一意の識別子であり、[&#x200B; エンタイトルメント &#x200B;](#entitlement) フローのすべてのフェーズで[&#x200B; プログラマー](#programmer) アプリケーションから提供される必要があります。

### E {#e}

#### 使用権限 {#entitlement}

使用権限は、[認証](#authentication)、[事前認証](#preauthorization)、[認証](#authorization)、最後に[&#x200B; ログアウト &#x200B;](#logout)の範囲の保護されたコンテンツへのアクセスに役立つ、様々なフェーズを通過する利用可能なフローと機能を組み込んだAdobe Pass認証の概念です。

#### 拡張エラーコード {#enhanced-error-code}

拡張エラーコードは、リクエストの処理中に発生したエラーに関する詳細情報を提供するAdobe Pass認証の概念です。

詳しくは、[拡張エラーコード &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントを参照してください。

### H {#h}

#### HBA {#hba}

ホームベース認証（HBA）とは、サブスクリプション契約内の場所の一部である、ホームネットワークに接続されている一部のデバイス上の[TV Everywhere （TVE） &#x200B;](#tve) コンテンツへのアクセスが消費者に自動的に許可されるプロセスです。

### I {#i}

#### ID プロバイダー {#identity-provider}

ID プロバイダーは、[TV Everywhere （TVE） &#x200B;](#tve)のコンテキストで、ケーブル、衛星、またはインターネット ベースのサービスを通じて消費者にID サービスを提供する企業です。

[MVPD](#mvpd)および[TV Provider](#tv-provider)と同義語です。

### L {#l}

#### ログアウト {#logout}

ログアウトは、ユーザーがAdobe Pass Authentication内で認証済みの[&#x200B; プロファイル &#x200B;](#profile)を終了し、ユーザーのステータスを反映するために[Programmer](#programmer) アプリケーションを更新できるようにするプロセスです。

### M {#m}

#### メディアトークン {#media-token}

メディアトークンは、保護されたコンテンツへのアクセスを提供するための認証[decision](#decision)の結果としてAdobe Pass Authenticationによって生成されたトークンです。

メディアトークンは[&#x200B; プログラマー](#programmer)に渡され、その[&#x200B; リソース &#x200B;](#resource)に対するアクセスのセキュリティを確保するために検証されます。

以前の用語の使用された短期認証トークンと同義です。

#### Media Token Verifier {#media-token-verifier}

Media Token Verifierは、[Adobe Pass トークン &#x200B;](#media-token)の信頼性を検証する役割を担う、Media Authenticationによって配布されるライブラリです。

詳しくは、[Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)のドキュメントを参照してください。

#### MVPD {#mvpd}

マルチチャネルビデオプログラミングディストリビューター（MVPD）は、ケーブル、衛星、インターネットベースのサービスを通じてテレビサービスを提供する企業です。

MVPDは、MVPDとAdobe間のオンボーディングプロセスで定義された一意の値によって識別されます。

[TV Provider](#tv-provider)および[ID Provider](#identity-provider)と同義語です。

### P {#p}

#### パートナー {#partner}

パートナーは、[&#x200B; プログラマー](#programmer)にサービスまたはフレームワークを提供して、シングルサインオンユーザーエクスペリエンスを有効にする企業です。

パートナーは、パートナーとAdobe間のオンボーディングプロセス中に定義される一意の値（例：「apple」）によって識別されます。

#### 事前認証 {#preauthorization}

事前認証は、[MVPD](#mvpd)でユーザー権限を検証した後、アクセス権が付与される[&#x200B; プログラマー](#programmer) カタログの[resources](#resource)のサブセットをプレビューできるプロセスです。

[Preflight](#preflight)と同義です。

#### プリフライト {#preflight}

プリフライトは、[MVPD](#mvpd)でユーザー権限を検証した後、アクセス権が付与される[&#x200B; プログラマー](#programmer) カタログの[resources](#resource)のサブセットをプレビューできるプロセスです。

[事前認証](#preauthorization)と同義です。

#### プライマリ（プログラマ）アプリ {#primary-application}

プライマリアプリケーションとは、[認証](#authentication)を開始する[&#x200B; プログラマー](#programmer) アプリケーションを指しますが、[&#x200B; ユーザーエージェント &#x200B;](#user-agent)を使用して[MVPD](#mvpd) ログインページに移動すると、プロセスを完了できない可能性があります。

#### プロファイル {#profile}

プロファイルは、ユーザーの認証開始日と終了日、[&#x200B; ユーザーのメタデータ &#x200B;](#user-metadata)および認証の取得方法を示す他のフィールド（例：「標準」、「劣化」、「一時的」、「シングルサインオン」など）に関する情報を格納するAdobe Pass認証の概念です。

以前の使用済み認証トークンと同義です。

#### プログラマー {#programmer}

プログラマーは、さまざまなプラットフォームにわたるオウンドチャネル（ブランド）を通じて消費者にコンテンツを提供する企業です。

プログラマーは、Adobe Pass Authenticationとの統合で、複数のオウンドチャネル（ブランド）を[&#x200B; サービスプロバイダー](#service-provider)としてグループ化します。

#### Proxy MVPD {#proxy-mvpd}

プロキシ MVPDは、他のMVPDのID サービスを提供し、Adobe Pass認証と直接統合する企業です。

#### Proxied MVPD {#proxied-mvpd}

プロキシ化されたMVPDは、Adobe Pass Authenticationと直接統合されていませんが、[&#x200B; プロキシ MVPD](#proxy-mvpd)を通じて統合される企業です。

#### Platform ID {#platform-identity}

プラットフォーム IDは、ユーザーのデバイスにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成された一意のプラットフォーム ID ペイロードで、[&#x200B; プログラマー](#programmer)に提供され、シングルサインオン ユーザーエクスペリエンスを有効にします。

詳しくは、[&#x200B; プラットフォーム ID フローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)のドキュメントを参照してください。

### R {#r}

#### リソース {#resource}

リソースは、ユーザーが[&#x200B; プログラマー](#programmer) カタログからアクセスしようとしている保護されたコンテンツです。

リソースは、プログラマとMVPDの間で合意された一意の値によって識別されます。

詳しくは、[保護リソース &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)のドキュメントを参照してください。

### S {#s}

#### SAML {#saml}

Security Assertion Markup Language （SAML）は、当事者間、特に[ID プロバイダー](#identity-provider)と[&#x200B; サービスプロバイダー](#sp)の間で認証データと認証データを交換するためのオープンスタンダードです。

#### セカンダリ（プログラマ）アプリ {#secondary-application}

セカンダリアプリケーションとは、[&#x200B; ユーザーエージェント &#x200B;](#user-agent)を使用して[MVPD](#mvpd) ログインページに移動し、[認証](#authentication) プロセスを完了できる[&#x200B; プログラマー](#programmer) アプリケーションを指します。

セカンダリ アプリケーションは、プライマリ アプリケーションと同じデバイスまたは別の（セカンダリ） デバイスで実行される場合があります。この場合、ログイン体験は「セカンドスクリーン認証」ユーザーエクスペリエンスと呼ばれます。

#### サービストークン {#service-token}

サービストークンは、ユーザーにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成された一意のユーザーIDで、[&#x200B; プログラマー](#programmer)に提供され、シングルサインオンユーザーエクスペリエンスを有効にします。

詳しくは、[&#x200B; サービストークンのフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)のドキュメントを参照してください。

#### サービスプロバイダー {#service-provider}

サービスプロバイダーは、[&#x200B; プログラマー](#programmer)が所有するチャネル（ブランド）です。

サービスプロバイダーは、プログラマーとAdobe間のオンボーディングプロセス中に定義された一意の値によって識別されます。

以前の用語が使用した依頼者IDと同義です。

#### SLO {#slo}

シングルログアウト （SLO）とは、[&#x200B; シングルサインオン （SSO） &#x200B;](#sso)に含まれるすべてのアプリケーションからユーザーがログアウトできるようにするプロセスです。

#### SP {#sp}

サービスプロバイダー（SP）とは、[MVPD](#mvpd)との統合で[&#x200B; プログラマー](#programmer)の代理としてAdobe Pass認証が果たす役割を指します。

#### SSO {#sso}

シングルサインオン （SSO）とは、ユーザーが1回認証を行い、それぞれに対する認証を行うことなく、複数の[&#x200B; プログラマー](#programmer) アプリケーションにアクセスできるようにするプロセスです。

### T {#t}

#### TempPass Basic {#temp-pass-basic}

基本的なTempPassは、[Adobe Pass](#mvpd)で認証を行うことなく、保護されたコンテンツに期間限定でアクセスできるMVPD認証機能です。

詳しくは、[Basic TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass) ドキュメントを参照してください。

#### TempPass プロモーション {#temp-pass-promotional}

プロモーション用のTempPassは、Adobe Pass認証機能です。この機能を使用すると、ユーザーは[MVPD](#mvpd)で認証を行うことなく、保護されたコンテンツに最大数のリソースと期間限定でアクセスできます。

詳しくは、[&#x200B; プロモーションテンプパス &#x200B;](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)のドキュメントを参照してください。

#### TTL {#ttl}

TTL （Time to Live）は、基になるエンティティが有効な時間を示す値です。

TTLは、[&#x200B; アクセストークン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token)、[&#x200B; プロファイル &#x200B;](#profile)、認証[決定](#decision)、または[&#x200B; メディアトークン &#x200B;](#media-token)に記載できます。

#### TVE {#tve}

TV Everywhere （TVE）は、消費者が、スマートフォンやタブレット、ノートパソコンなど、複数のデバイスからお気に入りのテレビ番組や映画などのコンテンツにアクセスできるニッチな業界です。

#### TVE ダッシュボード {#tve-dashboard}

TV Everywhere （TVE）ダッシュボードは、[&#x200B; プログラマー](#programmer)に提供されるAdobe Pass認証ツールで、設定とデータを管理します。

詳しくは、[TVE ダッシュボード ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)のドキュメントを参照してください。

#### TV プロバイダー {#tv-provider}

テレビ事業者は、ケーブル、衛星、またはインターネットベースのサービスを通じて消費者にテレビサービスを提供する企業です。

TV プロバイダーは、TV プロバイダーとAdobe間のオンボーディングプロセス中に定義された一意の値によって識別されます。

[MVPD](#mvpd)および[ID プロバイダー](#identity-provider)と同義語です。

### U {#u}

#### ユーザーエージェント {#user-agent}

ユーザーエージェントは、Webをナビゲートし、[MVPD](#mvpd) ログインページをレンダリングできるブラウザーまたは同様のコンポーネント（プラットフォーム固有）を指します。

#### ユーザー ID {#user-id}

ユーザーIDは、ユーザーにバインドされた一意のIDで、[MVPD](#mvpd)認証プロセスから送信されます。

#### ユーザーメタデータ {#user-metadata}

ユーザーメタデータは、ユーザー固有の属性（郵便番号、保護者の評価、ユーザーIDなど）を指します。 [MVPD](#mvpd)によって管理され、[&#x200B; プロファイル &#x200B;](#profile)の一部としてAdobe Pass認証によって提供されるサービスです。

詳しくは、[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) ドキュメントを参照してください。

### V {#v}

#### VSA {#vsa}

ビデオ購読者アカウント （VSA）は、[&#x200B; プログラマー](#programmer)に提供されるApple開発のフレームワークで、シングルサインオンユーザーエクスペリエンスを有効にします。

詳しくは、[&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount)および[&#x200B; パートナーフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)のドキュメントを参照してください。
