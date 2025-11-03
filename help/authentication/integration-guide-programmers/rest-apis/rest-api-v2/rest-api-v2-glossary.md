---
title: REST API V2 の用語集
description: REST API V2 の用語集
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# REST API V2 の用語集 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass認証 REST API V2 を統合する際に使用する用語の定義について説明します。

>[!MORELIKETHIS]
>
> * [Dynamic Client Registration （DCR）の用語集 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## 用語集の用語 {#glossary-terms}

### A {#a}

#### 認証 {#authentication}

認証とは、[MVPD](#programmer) でユーザーサブスクリプションを検証した後、ユーザーが [&#x200B; プログラマー）に ID を証明し、保護されたコンテンツ（](#resource)resource[）にアクセスできるようにするプロセス &#x200B;](#mvpd) す。

#### 認証コード {#code}

認証コードは、ユーザーが [authentication](#authentication) プロセスを開始する際に生成される一意の値を格納し、認証プロセスが完了するまでユーザーの [authentication session](#session) を一意に識別するAdobe Pass Authentication 概念です。

認証コードは、[プライマリ（プログラマー）アプリケーションまたは &#x200B;](#primary-application)セカンダリ（プログラマー）アプリケーションの両方で使用でき [&#128279;](#secondary-application) 認証 [&#x200B; プロセスを完了したり &#x200B;](#authentication) [&#x200B; 認証セッション &#x200B;](#session) に関する情報を取得したり、ユーザー [&#x200B; プロファイル &#x200B;](#profile) にアクセスしたりできます。

以前の用語と同義で、登録コードを使用します。

#### 認証セッション {#session}

認証セッションは、[&#x200B; プログラマ &#x200B;](#programmer) アプリケーションから開始（または継続）されたユーザーの認証プロセスに関する情報を格納するAdobe Pass認証コンセプトであり、[&#x200B; 認証コード &#x200B;](#code) によって一意に識別されます。

また、ユーザーがすでに認証されている場合に備えて、認証セッションでは、[&#x200B; 権限 &#x200B;](#programmer) フローの次のフェーズとして [&#x200B; 承認 &#x200B;](#authorization) プロセスを続行するように [&#x200B; プログラマー &#x200B;](#entitlement) アプリケーションを指定することもできます。

#### 認証 {#authorization}

承認とは、[MVPD](#resource) でユーザー権限を検証した後、所有している [4&rbrace;MVPD](#programmer) サブスクリプションに基づいて、ユーザーが [&#x200B; プログラマー &#x200B;](#mvpd) カタログから保護されたコンテンツ（リソース [）にアクセスできるようにするプロセスです。](#mvpd)

### C {#c}

#### 設定 {#configuration}

設定は、[&#x200B; プログラマー &#x200B;](#programmer) と [MVPD](#mvpd) の統合設定に関する情報を保存するAdobe Pass認証の概念であり、[&#x200B; 認証 &#x200B;](#authentication) プロセス中に、アクティブな統合のリストから [TV プロバイダー &#x200B;](#tv-provider) を選択するようにユーザーに求める際に使用できます。

### 日 {#d}

#### 決定 {#decision}

この判断は、Adobe Pass認証の概念です。この概念には、保護されたコンテンツ [&#x200B; プログラマー &#x200B;](#mvpd) へのアクセスを許可または拒否するための [MVPD](#authorization) [&#x200B; 認証 &#x200B;](#preauthorization) または [&#x200B; 事前認証 &#x200B;](#programmer) のプロセス照会に関する情報が格納されます。

#### 分解 {#degradation}

この低下は、[MVPD](#mvpd) でサービスが中断された場合でも、ユーザーが保護されたコンテンツにアクセスできるようにするAdobe Pass認証機能です。

詳細については、「[&#x200B; 機能の低下 &#x200B;](/help/premium-workflow/degraded-access/degradation-feature.md)」を参照してください。

#### デバイス ID {#device-id}

デバイス ID は、ユーザーのデバイスにバインドされる一意の識別子で、[&#x200B; 使用権限 &#x200B;](#programmer) フローのすべてのフェーズで [&#x200B; プログラマー &#x200B;](#entitlement) アプリケーションから提供される必要があります。

### E {#e}

#### 使用権限 {#entitlement}

使用権限は、Adobe Pass認証の概念です。使用可能なフローと機能を組み込むことで、ユーザーが様々なフェーズを経て、[&#x200B; 認証 &#x200B;](#authentication)、[&#x200B; 事前認証 &#x200B;](#preauthorization)、[&#x200B; 認証 &#x200B;](#authorization)、最終的には [&#x200B; ログアウト &#x200B;](#logout) に至るまで、保護されたコンテンツにアクセスできるようにします。

#### エラーコードの強化 {#enhanced-error-code}

拡張エラーコードは、リクエストの処理中に発生したエラーに関する追加情報を提供する、Adobe Pass認証の概念です。

詳しくは、[&#x200B; 拡張エラーコード &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントを参照してください。

### H {#h}

#### HBA {#hba}

ホームベースの認証（HBA）とは、消費者が、購読の契約における場所の一部である、ホームネットワークに接続された特定のデバイス上の [TV Everywhere （TVE） &#x200B;](#tve) コンテンツへのアクセスを自動的に許可されるプロセスです。

### I {#i}

#### ID プロバイダー {#identity-provider}

ID プロバイダーは、[TV Everywhere （TVE） &#x200B;](#tve) のコンテキストで、ケーブル、衛星、またはインターネットベースのサービスをまたいで消費者に ID サービスを提供する会社です。

[MVPD](#mvpd) および [TV プロバイダー &#x200B;](#tv-provider) と同義。

### L {#l}

#### ログアウト {#logout}

ログアウトとは、Adobe Pass Authentication 内でユーザーが認証済みの [&#x200B; プロファイル &#x200B;](#profile) を終了し、ユーザーのステータスを反映するように [&#x200B; プログラマー &#x200B;](#programmer) アプリケーションを更新できるプロセスです。

### M {#m}

#### メディアトークン {#media-token}

メディアトークンは、保護されたコンテンツへのアクセスを提供することを目的とした認証 [&#x200B; 決定 &#x200B;](#decision) の結果としてAdobe Pass Authentication によって生成されるトークンです。

メディアトークンは [&#x200B; プログラマー &#x200B;](#programmer) に渡され、次にその [&#x200B; リソース &#x200B;](#resource) のアクセスのセキュリティを確保するために検証されます。

以前の用語と同義で、短い認証トークンを使用します。

#### メディアトークン検証子 {#media-token-verifier}

メディアトークンベリファイアは、Adobe Pass認証によって配布されるライブラリで、[&#x200B; メディアトークン &#x200B;](#media-token) の信頼性の検証を担当します。

詳しくは、[&#x200B; メディアトークンベリファイア &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) ドキュメントを参照してください。

#### MVPD {#mvpd}

MVPD（multichannel video programming distributor）は、ケーブル、衛星、またはインターネットを利用したサービスを提供している企業です。

MVPDは、MVPDとAdobe間のオンボーディングプロセス中に定義された一意の値によって識別されます。

[TV プロバイダー &#x200B;](#tv-provider) および [ID プロバイダー &#x200B;](#identity-provider) と同義。

### P {#p}

#### パートナー {#partner}

パートナーは、シングルサインオンユーザーエクスペリエンスを有効にするためのサービスまたはフレームワークを [&#x200B; プログラマー &#x200B;](#programmer) に提供する会社です。

パートナーは、パートナーとAdobe間のオンボーディングプロセス中に定義される一意の値（「apple」など）で識別されます。

#### 事前認証 {#preauthorization}

事前認証とは、[MVPD](#resource) でユーザー権限を検証した後、ユーザーがアクセス権を持つ [&#x200B; プログラマー &#x200B;](#programmer) カタログから [&#x200B; リソース &#x200B;](#mvpd) のサブセットをプレビューできるプロセスです。

[&#x200B; プリフライト &#x200B;](#preflight) と同義。

#### Preflight {#preflight}

プリフライトは、[MVPD](#resource) でユーザー権限を検証した後、ユーザーがアクセスする資格のある [&#x200B; プログラマー &#x200B;](#programmer) カタログから [&#x200B; リソース &#x200B;](#mvpd) のサブセットをプレビューできるプロセスです。

[&#x200B; 事前認証 &#x200B;](#preauthorization) と同義。

#### プライマリ（プログラマ）アプリケーション {#primary-application}

プライマリアプリケーションとは、[&#x200B; 認証 &#x200B;](#programmer) を開始する [&#x200B; プログラマー &#x200B;](#authentication) アプリケーションを指します。ただし、[&#x200B; ユーザーエージェント &#x200B;](#user-agent) を使用して [MVPD](#mvpd) のログインページに移動すると、このプロセスを完了できない可能性があります。

#### Profile {#profile}

プロファイルは、ユーザーの認証開始日と終了日、[&#x200B; ユーザーのメタデータ &#x200B;](#user-metadata) および認証の取得方法を示すその他のフィールド（「通常」、「機能縮退」、「一時」、「シングルサインオン」など）に関する情報を保存するAdobe Pass認証コンセプトです。

以前の用語と同義で、認証トークンを使用します。

#### プログラマ {#programmer}

プログラマーは、様々なプラットフォームをまたいで所有チャネル（ブランド）を通じて消費者にコンテンツを提供する会社です。

プログラマーは、Adobe Pass Authentication との統合において、複数の所有チャネル（ブランド）を [&#x200B; サービスプロバイダー &#x200B;](#service-provider) としてグループ化します。

#### プロキシMVPD {#proxy-mvpd}

プロキシ MVPDは、他の MVPD に ID サービスを提供する会社で、Adobe Pass Authentication と直接統合されています。

#### プロキシ化されたMVPD {#proxied-mvpd}

プロキシを使用したMVPDは、Adobe Pass Authentication と直接統合されておらず、[&#x200B; プロキシ MVPD](#proxy-mvpd) を通じて統合される会社です。

#### プラットフォーム ID {#platform-identity}

プラットフォーム ID は、ユーザーのデバイスにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成される一意のプラットフォーム識別子ペイロードで、シングルサインオンユーザーエクスペリエンスを有効にするために [&#x200B; プログラマー &#x200B;](#programmer) に提供されます。

詳しくは、[&#x200B; プラットフォーム ID フローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) ドキュメントを参照してください。

### R {#r}

#### Resource {#resource}

リソースは、ユーザーが [&#x200B; プログラマー &#x200B;](#programmer) カタログからアクセスしようとしている、保護されたコンテンツです。

リソースは、プログラマと MVPD の間で合意された一意の値によって識別されます。

詳しくは、[&#x200B; 保護されたリソース &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) ドキュメントを参照してください。

### S {#s}

#### SAML {#saml}

SAML （Security Assertion Markup Language）は、当事者間、特に [ID プロバイダ）と &#x200B;](#identity-provider) サービス・プロバイダ [&#x200B; との間で認証および認可データを交換するためのオープン &#x200B;](#sp) 標準です。

#### セカンダリ（プログラマ）アプリケーション {#secondary-application}

セカンダリアプリケーションとは、[&#x200B; ユーザーエージェント &#x200B;](#programmer) を使用して [MVPD](#authentication) のログインページに移動して [&#x200B; 認証 &#x200B;](#user-agent) プロセスを完了できる [&#x200B; プログラマー &#x200B;](#mvpd) アプリケーションを指します。

セカンダリアプリケーションは、プライマリアプリケーションと同じデバイス上または別の（セカンダリ）デバイス上で実行される場合があります。この場合、ログインエクスペリエンスは、「2 番目の画面認証」ユーザーエクスペリエンスと呼ばれます。

#### サービストークン {#service-token}

サービストークンは、ユーザーにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成される一意のユーザー識別子で、シングルサインオンユーザーエクスペリエンスを有効にするために [&#x200B; プログラマー &#x200B;](#programmer) に提供されます。

詳しくは、[&#x200B; サービストークンフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) ドキュメントを参照してください。

#### サービスプロバイダー {#service-provider}

サービスプロバイダーとは、[&#x200B; プログラマー）が所有するチャネル（ブランド &#x200B;](#programmer) です。

サービスプロバイダーは、プログラマーとAdobe間のオンボーディングプロセス中に定義された一意の値によって識別されます。

以前の用語で使用された要求者 ID と同義です。

#### SLO {#slo}

シングルログアウト（SLO）は、[&#x200B; シングルサインオン（SSO） &#x200B;](#sso) に含まれるすべてのアプリケーションからユーザーがログアウトできるプロセスです。

#### SP {#sp}

サービスプロバイダー（SP）とは、[MVPD](#programmer) との統合において、[&#x200B; プログラマー &#x200B;](#mvpd) に代わってAdobe Pass認証が果たす役割を指します。

#### SSO {#sso}

シングルサインオン（SSO）は、ユーザーが 1 回認証すると、複数の [&#x200B; プログラマー &#x200B;](#programmer) アプリケーションのそれぞれに対して認証を行う必要なく、それらのアプリケーションにアクセスできるプロセスです。

### T {#t}

#### TempPass Basic {#temp-pass-basic}

基本的な TempPass は、[MVPD](#mvpd) による認証の必要なしに、ユーザーが保護されたコンテンツに限られた時間だけアクセスできるようにするAdobe Pass認証機能です。

詳しくは、[&#x200B; 基本 TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md#basic-temp-pass) ドキュメントを参照してください。

#### TempPass プロモーション {#temp-pass-promotional}

プロモーション TempPass は、Adobe Pass認証機能の 1 つです。これにより、[MVPD](#mvpd) による認証を必要とせずに、保護されたコンテンツにアクセスできるリソースの最大数と時間を制限できます。

詳しくは、[&#x200B; プロモーション TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md#promotional-temp-pass) ドキュメントを参照してください。

#### TTL {#ttl}

有効期間（TTL）は、基になるエンティティが有効な時間を示す値です。

TTL は、[&#x200B; アクセストークン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token)、[&#x200B; プロファイル &#x200B;](#profile)、認証 [&#x200B; 決定 &#x200B;](#decision) または [&#x200B; メディアトークン &#x200B;](#media-token) について言及できます。

#### TVE {#tve}

TV Everywhere （TVE）は、消費者がスマートフォン、タブレット、ノートパソコンなどの複数のデバイスで、お気に入りのテレビ番組、映画、その他のコンテンツにアクセスできるようにする業界ニッチです。

#### TVE ダッシュボード {#tve-dashboard}

TV Everywhere （TVE）ダッシュボードは、設定とデータを管理するために [&#x200B; プログラマー &#x200B;](#programmer) に提供されるAdobe Pass認証ツールです。

詳しくは、[TVE ダッシュボードユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) ドキュメントを参照してください。

#### TV プロバイダ {#tv-provider}

TV プロバイダーは、ケーブル、衛星、またはインターネットベースのサービスを通じて消費者にテレビサービスを提供する会社です。

TV プロバイダーは、TV プロバイダーとAdobeの間のオンボーディングプロセス中に定義された一意の値によって識別されます。

[MVPD](#mvpd) および [ID プロバイダー &#x200B;](#identity-provider) と同義です。

### U {#u}

#### ユーザーエージェント {#user-agent}

ユーザーエージェントとは、Web 上を移動して [MVPD](#mvpd) のログインページをレンダリングできるブラウザーまたは同様のコンポーネント（プラットフォーム固有）を指します。

#### ユーザー ID {#user-id}

ユーザー ID は、ユーザーにバインドされる一意の ID で、[MVPD](#mvpd) 認証プロセスから生成されます。

#### ユーザーメタデータ {#user-metadata}

ユーザーメタデータとは、ユーザー固有の属性（郵便番号、保護者の制限、ユーザー ID など）を指し、[MVPDによって維持管理され &#x200B;](#mvpd)Adobe Pass認証が [&#x200B; プロファイル &#x200B;](#profile) の一部として提供します。

詳しくは、[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) ドキュメントを参照してください。

### V {#v}

#### VSA {#vsa}

ビデオ購読者アカウント（VSA）は、シングルサインオンユーザーエクスペリエンスを可能にするために [&#x200B; プログラマー &#x200B;](#programmer) に提供される、Appleが開発したフレームワークです。

詳しくは、[&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) および [&#x200B; パートナーフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) ドキュメントを参照してください。
