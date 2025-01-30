---
title: REST API V2 の用語集
description: REST API V2 の用語集
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# REST API V2 の用語集 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass認証 REST API V2 を統合する際に使用する用語の定義について説明します。

>[!MORELIKETHIS]
>
> * [Dynamic Client Registration （DCR）の用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## 用語集の用語 {#glossary-terms}

### {#a}

#### 認証 {#authentication}

認証とは、[MVPD[ でユーザーサブスクリプションを検証した後、ユーザーが ](#programmer) プログラマー）に ID を証明し、保護されたコンテンツ（[resource](#resource)）にアクセスできるようにするプロセス ](#mvpd) す。

#### 認証コード {#code}

認証コードは、ユーザーが [authentication](#authentication) プロセスを開始する際に生成される一意の値を格納し、認証プロセスが完了するまでユーザーの [authentication session](#session) を一意に識別するAdobe Pass Authentication 概念です。

認証コードは、[プライマリ（プログラマー）アプリケーションまたは [セカンダリ（プログラマー）アプリケーションの両方で使用でき ](#primary-application)[ 認証 ](#authentication) プロセスを完了したり ](#secondary-application)[ 認証セッション ](#session) に関する情報を取得したり、ユーザー [ プロファイル ](#profile) にアクセスしたりできます。

以前の用語と同義で、登録コードを使用します。

#### 認証セッション {#session}

認証セッションは、[ プログラマ ](#programmer) アプリケーションから開始（または継続）されたユーザーの認証プロセスに関する情報を格納するAdobe Pass認証コンセプトであり、[ 認証コード ](#code) によって一意に識別されます。

また、ユーザーがすでに認証されている場合に備えて、認証セッションでは、[ 権限 ](#programmer) フローの次のフェーズとして [ 承認 ](#authorization) プロセスを続行するように [ プログラマー ](#entitlement) アプリケーションを指定することもできます。

#### 認証 {#authorization}

承認とは、[MVPD[ でユーザー権限を検証した後、所有している [4}MVPD](#programmer) サブスクリプションに基づいて、ユーザーが ](#mvpd) プログラマー ](#mvpd) カタログから保護されたコンテンツ（リソース ](#resource)）にアクセスできるようにするプロセスです。[

### C {#c}

#### 設定 {#configuration}

設定は、[ プログラマー ](#programmer) と [MVPD](#mvpd) の統合設定に関する情報を保存するAdobe Pass認証の概念であり、[ 認証 ](#authentication) プロセス中に、アクティブな統合のリストから [TV プロバイダー ](#tv-provider) を選択するようにユーザーに求める際に使用できます。

### D {#d}

#### 決定 {#decision}

この判断は、Adobe Pass認証の概念です。この概念には、保護されたコンテンツ ](#mvpd) プログラマー [ へのアクセスを許可または拒否するための [MVPD](#authorization)[ 認証 ](#preauthorization) または [ 事前認証 ](#programmer) のプロセス照会に関する情報が格納されます。

#### 分解 {#degradation}

この低下は、[MVPD](#mvpd) でサービスが中断された場合でも、ユーザーが保護されたコンテンツにアクセスできるようにするAdobe Pass認証機能です。

詳細については、「[ 機能の低下 ](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)」を参照してください。

#### デバイス ID {#device-id}

デバイス ID は、ユーザーのデバイスにバインドされる一意の識別子で、[ 使用権限 ](#programmer) フローのすべてのフェーズで [ プログラマー ](#entitlement) アプリケーションから提供される必要があります。

### E {#e}

#### 使用権限 {#entitlement}

使用権限は、Adobe Pass認証の概念です。使用可能なフローと機能を組み込むことで、ユーザーが様々なフェーズを経て、[ 認証 ](#authentication)、[ 事前認証 ](#preauthorization)、[ 認証 ](#authorization)、最終的には [ ログアウト ](#logout) に至るまで、保護されたコンテンツにアクセスできるようにします。

#### エラーコードの強化 {#enhanced-error-code}

拡張エラーコードは、リクエストの処理中に発生したエラーに関する追加情報を提供する、Adobe Pass認証の概念です。

詳しくは、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントを参照してください。

### 時間 {#h}

#### HBA {#hba}

ホームベースの認証（HBA）とは、消費者が、購読の契約における場所の一部である、ホームネットワークに接続された特定のデバイス上の [TV Everywhere （TVE） ](#tve) コンテンツへのアクセスを自動的に許可されるプロセスです。

### I {#i}

#### ID プロバイダー {#identity-provider}

ID プロバイダーは、[TV Everywhere （TVE） ](#tve) のコンテキストで、ケーブル、衛星、またはインターネットベースのサービスをまたいで消費者に ID サービスを提供する会社です。

[MVPD](#mvpd) および [TV プロバイダー ](#tv-provider) と同義。

### L {#l}

#### ログアウト {#logout}

ログアウトとは、Adobe Pass Authentication 内でユーザーが認証済みの [ プロファイル ](#profile) を終了し、ユーザーのステータスを反映するように [ プログラマー ](#programmer) アプリケーションを更新できるプロセスです。

### M {#m}

#### メディアトークン {#media-token}

メディアトークンは、保護されたコンテンツへのアクセスを提供することを目的とした認証 [ 決定 ](#decision) の結果としてAdobe Pass Authentication によって生成されるトークンです。

メディアトークンは [ プログラマー ](#programmer) に渡され、次にその [ リソース ](#resource) のアクセスのセキュリティを確保するために検証されます。

以前の用語と同義で、短い認証トークンを使用します。

#### メディアトークン検証子 {#media-token-verifier}

メディアトークンベリファイアは、Adobe Pass認証によって配布されるライブラリで、[ メディアトークン ](#media-token) の信頼性の検証を担当します。

詳しくは、[ メディアトークンベリファイア ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) ドキュメントを参照してください。

#### MVPD {#mvpd}

MVPD（multichannel video programming distributor）は、ケーブル、衛星、またはインターネットを利用したサービスを提供している企業です。

MVPDは、MVPDとAdobeの間のオンボーディングプロセス中に定義された一意の値によって識別されます。

[TV プロバイダー ](#tv-provider) および [ID プロバイダー ](#identity-provider) と同義。

### P {#p}

#### パートナー {#partner}

パートナーは、シングルサインオンユーザーエクスペリエンスを有効にするためのサービスまたはフレームワークを [ プログラマー ](#programmer) に提供する会社です。

パートナーは、パートナーとAdobeの間のオンボーディングプロセス中に定義される一意の値（「apple」など）によって識別されます。

#### 事前認証 {#preauthorization}

事前認証とは、[MVPD[ でユーザー権限を検証した後、ユーザーがアクセス権を持つ ](#resource) プログラマー ](#programmer) カタログから [ リソース ](#mvpd) のサブセットをプレビューできるプロセスです。

[ プリフライト ](#preflight) と同義。

#### Preflight {#preflight}

プリフライトは、[MVPD[ でユーザー権限を検証した後、ユーザーがアクセスする資格のある [ プログラマー ](#programmer) カタログから ](#resource) リソース ](#mvpd) のサブセットをプレビューできるプロセスです。

[ 事前認証 ](#preauthorization) と同義。

#### プライマリ（プログラマ）アプリケーション {#primary-application}

プライマリアプリケーションとは、[ 認証 ](#authentication) を開始する [ プログラマー ](#programmer) アプリケーションを指します。ただし、[ ユーザーエージェント ](#user-agent) を使用して [MVPD](#mvpd) のログインページに移動すると、このプロセスを完了できない可能性があります。

#### Profile {#profile}

プロファイルは、ユーザーの認証開始日と終了日、[ ユーザーのメタデータ ](#user-metadata) および認証の取得方法を示すその他のフィールド（「通常」、「機能縮退」、「一時」、「シングルサインオン」など）に関する情報を保存するAdobe Pass認証コンセプトです。

以前の用語と同義で、認証トークンを使用します。

#### プログラマ {#programmer}

プログラマーは、様々なプラットフォームをまたいで所有チャネル（ブランド）を通じて消費者にコンテンツを提供する会社です。

プログラマーは、Adobe Pass Authentication との統合において、複数の所有チャネル（ブランド）を [ サービスプロバイダー ](#service-provider) としてグループ化します。

#### プロキシMVPD {#proxy-mvpd}

プロキシ MVPDは、他の MVPD に ID サービスを提供する会社で、Adobe Pass Authentication と直接統合されています。

#### プロキシ化されたMVPD {#proxied-mvpd}

プロキシを使用したMVPDは、Adobe Pass Authentication と直接統合されておらず、[ プロキシ MVPD](#proxy-mvpd) を通じて統合される会社です。

#### プラットフォーム ID {#platform-identity}

プラットフォーム ID は、ユーザーのデバイスにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成される一意のプラットフォーム識別子ペイロードで、シングルサインオンユーザーエクスペリエンスを有効にするために [ プログラマー ](#programmer) に提供されます。

詳しくは、[ プラットフォーム ID フローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) ドキュメントを参照してください。

### R {#r}

#### Resource {#resource}

リソースは、ユーザーが [ プログラマー ](#programmer) カタログからアクセスしようとしている、保護されたコンテンツです。

リソースは、プログラマと MVPD の間で合意された一意の値によって識別されます。

詳しくは、[ 保護されたリソース ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) ドキュメントを参照してください。

### S {#s}

#### SAML {#saml}

SAML （Security Assertion Markup Language）は、当事者間、特に [ID プロバイダ）と [ サービス・プロバイダ ](#sp) との間で認証および認可データを交換するためのオープン ](#identity-provider) 標準です。

#### セカンダリ（プログラマ）アプリケーション {#secondary-application}

セカンダリアプリケーションとは、[ ユーザーエージェント ](#programmer) を使用して [MVPD[ のログインページに移動して ](#authentication) 認証 [ プロセスを完了できる ](#user-agent) プログラマー ](#mvpd) アプリケーションを指します。

セカンダリアプリケーションは、プライマリアプリケーションと同じデバイス上または別の（セカンダリ）デバイス上で実行される場合があります。この場合、ログインエクスペリエンスは、「2 番目の画面認証」ユーザーエクスペリエンスと呼ばれます。

#### サービストークン {#service-token}

サービストークンは、ユーザーにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成される一意のユーザー識別子で、シングルサインオンユーザーエクスペリエンスを有効にするために [ プログラマー ](#programmer) に提供されます。

詳しくは、[ サービストークンフローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) ドキュメントを参照してください。

#### サービスプロバイダー {#service-provider}

サービスプロバイダーとは、[ プログラマー）が所有するチャネル（ブランド ](#programmer) です。

サービスプロバイダーは、プログラマーとAdobeの間のオンボーディングプロセス中に定義された一意の値によって識別されます。

以前の用語で使用された要求者 ID と同義です。

#### SLO {#slo}

シングルログアウト（SLO）は、[ シングルサインオン（SSO） ](#sso) に含まれるすべてのアプリケーションからユーザーがログアウトできるプロセスです。

#### SP {#sp}

サービスプロバイダー（SP）とは、[MVPD](#mvpd) との統合において、[ プログラマー ](#programmer) に代わってAdobe Pass認証が果たす役割を指します。

#### SSO {#sso}

シングルサインオン（SSO）は、ユーザーが 1 回認証すると、複数の [ プログラマー ](#programmer) アプリケーションのそれぞれに対して認証を行う必要なく、それらのアプリケーションにアクセスできるプロセスです。

### T {#t}

#### TempPass Basic {#temp-pass-basic}

基本的な TempPass は、[MVPD](#mvpd) による認証の必要なしに、ユーザーが保護されたコンテンツに限られた時間だけアクセスできるようにするAdobe Pass認証機能です。

詳しくは、[ 基本 TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass) ドキュメントを参照してください。

#### TempPass プロモーション {#temp-pass-promotional}

プロモーション TempPass は、Adobe Pass認証機能の 1 つです。これにより、[MVPD](#mvpd) による認証を必要とせずに、保護されたコンテンツにアクセスできるリソースの最大数と時間を制限できます。

詳しくは、[ プロモーション TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass) ドキュメントを参照してください。

#### TTL {#ttl}

有効期間（TTL）は、基になるエンティティが有効な時間を示す値です。

TTL は、[ アクセストークン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token)、[ プロファイル ](#profile)、認証 [ 決定 ](#decision) または [ メディアトークン ](#media-token) について言及できます。

#### TVE {#tve}

TV Everywhere （TVE）は、消費者がスマートフォン、タブレット、ノートパソコンなどの複数のデバイスで、お気に入りのテレビ番組、映画、その他のコンテンツにアクセスできるようにする業界ニッチです。

#### TVE ダッシュボード {#tve-dashboard}

TV Everywhere （TVE）ダッシュボードは、設定とデータを管理するために [ プログラマー ](#programmer) に提供されるAdobe Pass認証ツールです。

詳しくは、[TVE ダッシュボードユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) ドキュメントを参照してください。

#### TV プロバイダ {#tv-provider}

TV プロバイダーは、ケーブル、衛星、またはインターネットベースのサービスを通じて消費者にテレビサービスを提供する会社です。

TV プロバイダーは、TV プロバイダーとAdobeのオンボーディングプロセス中に定義された一意の値によって識別されます。

[MVPD](#mvpd) および [ID プロバイダー ](#identity-provider) と同義です。

### U {#u}

#### ユーザーエージェント {#user-agent}

ユーザーエージェントとは、Web 上を移動して [MVPD](#mvpd) のログインページをレンダリングできるブラウザーまたは同様のコンポーネント（プラットフォーム固有）を指します。

#### ユーザー ID {#user-id}

ユーザー ID は、ユーザーにバインドされる一意の ID で、[MVPD](#mvpd) 認証プロセスから生成されます。

#### ユーザーメタデータ {#user-metadata}

ユーザーメタデータとは、ユーザー固有の属性（郵便番号、保護者の制限、ユーザー ID など）を指し、[MVPDによって維持管理され ](#mvpd)Adobe Pass認証が [ プロファイル ](#profile) の一部として提供します。

詳しくは、[ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) ドキュメントを参照してください。

### V {#v}

#### VSA {#vsa}

ビデオ購読者アカウント（VSA）は、シングルサインオンユーザーエクスペリエンスを可能にするために [ プログラマー ](#programmer) に提供される、Appleが開発したフレームワークです。

詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) および [ パートナーフローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) ドキュメントを参照してください。
