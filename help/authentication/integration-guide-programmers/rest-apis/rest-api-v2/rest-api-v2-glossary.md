---
title: REST API V2 の用語集
description: REST API V2 の用語集
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1964'
ht-degree: 0%

---

# REST API V2 の用語集 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass認証 REST API V2 ドキュメントを統合する際に使用する用語の定義を示し、従来の [ 用語集 ](/help/authentication/kickstart/glossary.md) を上書きします。

## 用語集の用語 {#glossary-terms}

### {#a}

#### アクセストークン {#access-token}

アクセストークンは、保護された API へのアクセスを確保するための [Dynamic Client Registration （DCR） ](#dcr) プロセスの結果として、Adobe Pass Authentication によって生成されるトークンです。

#### 認証 {#authentication}

認証とは、[MVPD](#mvpd) でユーザーサブスクリプションを検証した後、ユーザーが自分の ID を [ プログラマー ](#programmer) に証明し、保護されたコンテンツ（[ リソース ](#resource)）にアクセスできるようにするプロセスです。

#### 認証コード {#code}

認証コードは、ユーザーが [authentication](#authentication) プロセスを開始する際に生成される一意の値を格納し、認証プロセスが完了するまでユーザーの [authentication session](#session) を一意に識別するAdobe Pass Authentication 概念です。

認証コードは、[プライマリ（プログラマー）アプリケーションまたは [セカンダリ（プログラマー）アプリケーションの両方で使用でき ](#primary-application)[ 認証 ](#authentication) プロセスを完了したり ](#secondary-application)[ 認証セッション ](#session) に関する情報を取得したり、ユーザー [ プロファイル ](#profile) にアクセスしたりできます。

以前の用語と同義で、登録コードを使用します。

#### 認証セッション {#session}

認証セッションは、[ プログラマ ](#programmer) アプリケーションから開始（または継続）されたユーザーの認証プロセスに関する情報を格納するAdobe Pass認証コンセプトであり、[ 認証コード ](#code) によって一意に識別されます。

また、ユーザーがすでに認証されている場合に備えて、認証セッションでは、[ 権限 ](#programmer) フローの次のフェーズとして [ 承認 ](#authorization) プロセスを続行するように [ プログラマー ](#entitlement) アプリケーションを指定することもできます。

#### 認証 {#authorization}

承認とは、[MVPD](#resource) でユーザー権限を検証した後、所有している [MVPD](#mvpd) サブスクリプションに基づいて、ユーザーが [Programmer](#programmer) カタログから保護されたコンテンツ（[resource](#mvpd)）にアクセスできるようにするプロセスです。

### C {#c}

#### クライアント資格情報 {#client-credentials}

クライアント資格情報は、[Dynamic Client Registration （DCR） ](#dcr) プロセス中に生成される一意の値のセットであり、[ アクセストークン ](#access-token) の取得に使用されることを目的としています。

#### 設定 {#configuration}

この設定は、[Programmer](#programmer) と [MVPD](#mvpd) の統合設定に関する情報を保存するAdobe Pass Authentication の概念であり、[authentication](#authentication) プロセス中に、アクティブな統合のリストから [TV Provider](#tv-provider) を選択するようにユーザーに求める際に使用できます。

#### カスタムスキーム {#custom-scheme}

カスタムスキームは、Adobe Pass[TVE Dashboard](#programmer) から生成およびダウンロードできる [Programmer](#tve-dashboard) アプリケーションを参照する一意の値で、iOS デバイスで動作するアプリケーションでの最終リダイレクトとして使用されます。

### D {#d}

#### DCR {#dcr}

Dynamic Client Registration （DCR）は、[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) で定義されている認証メカニズムであり、[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) で説明されている OAuth 2.0 認証フレームワークに基づいています。

DCR は、保護された API へのアクセスをさらに可能にするAdobe Pass認証サービスとして [ プログラマー ](#programmer) に配信されます。

詳しくは、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

#### 決定 {#decision}

この判断は、保護されたコンテンツ [ プログラマー ](#mvpd) に対するユーザーアクセスを許可または拒否するための [MVPD[ 承認 ](#authorization) または [ 事前承認 ](#preauthorization) プロセスの問い合わせに関する情報を保存するAdobe Pass認証の概念 ](#programmer) す。

#### 分解 {#degradation}

この劣化は、[MVPD](#mvpd) のサービスが中断された場合でも、保護されたコンテンツにユーザーがアクセスできるようにするAdobe Pass認証機能です。

詳しくは、[Degradation API の概要 ](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md) ドキュメントを参照してください。

### E {#e}

#### 使用権限 {#entitlement}

使用権限は、Adobe Pass認証の概念です。使用可能なフローと機能を組み込むことで、ユーザーが [ 認証 ](#authentication)、[ 事前認証 ](#preauthorization)、[ 認証 ](#authorization)、最終的には [ ログアウト ](#logout) に至る、保護されたコンテンツにアクセスするための様々なフェーズを進むことができます。

#### エラーコードの強化 {#enhanced-error-code}

拡張エラーコードは、リクエストの処理中に発生したエラーに関する追加情報を提供する、Adobe Pass認証の概念です。

詳しくは、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントを参照してください。

### 時間 {#h}

#### HBA {#hba}

ホームベースの認証（HBA）は、消費者が自動的に [TV Everywhere （TVE） ](#tve) 契約の場所の一部であるホームネットワークに接続された選択したデバイスのコンテンツへのアクセスを許可されるプロセスです。

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

詳しくは、[ メディアトークンベリファイアの統合 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md) ドキュメントを参照してください。

#### MVPD {#mvpd}

MVPD （Multichannel Video Programming Distributor）は、ケーブル、衛星、またはインターネットを利用したサービスを通じて、消費者にテレビサービスを提供する企業です。

MVPD は、MVPD とAdobeの間のオンボーディングプロセス中に定義された一意の値によって識別されます。

[TV プロバイダー ](#tv-provider) および [ID プロバイダー ](#identity-provider) と同義。

### P {#p}

#### パートナー {#partner}

パートナーは、シングルサインオンユーザーエクスペリエンスを有効にするために、[ プログラマー ](#programmer) にサービスまたはフレームワークを提供する会社です。

パートナーは、パートナーとAdobeの間のオンボーディングプロセス中に定義される一意の値（「apple」など）によって識別されます。

#### 事前認証 {#preauthorization}

事前認証とは、ユーザーが [MVPD](#mvpd) でユーザー権限を検証した後、アクセス権を持つ ](#resource) プログラマー ](#programmer) カタログから [ リソース [ のサブセットをプレビューできるようにするプロセスです。

[ プリフライト ](#preflight) と同義。

#### Preflight {#preflight}

プリフライトは、[MVPD](#mvpd) でユーザー権限を検証した後、ユーザーがアクセスする資格のある [ プログラマー ](#programmer) カタログから [ リソース ](#resource) のサブセットをプレビューできるプロセスです。

[ 事前認証 ](#preauthorization) と同義。

#### プライマリ（プログラマ）アプリケーション {#primary-application}

プライマリ アプリケーションとは、[ 認証 ](#authentication) を開始する [ プログラマ ](#programmer) アプリケーションを指しますが、[ ユーザーエージェント ](#user-agent) を使用して [MVPD](#mvpd) ログイン ページに移動することによってプロセスを完了することはできません。

#### Profile {#profile}

プロファイルは、ユーザーの認証開始日と終了日、[ ユーザーのメタデータ ](#user-metadata) および認証の取得方法を示すその他のフィールド（「通常」、「機能縮退」、「一時」、「シングルサインオン」など）に関する情報を保存するAdobe Pass認証コンセプトです。

以前の用語と同義で、認証トークンを使用します。

#### プログラマ {#programmer}

プログラマーは、様々なプラットフォームをまたいで所有チャネル（ブランド）を通じて消費者にコンテンツを提供する会社です。

プログラマーは、Adobe Pass Authentication との統合において、複数の所有チャネル（ブランド）を [ サービスプロバイダー ](#service-provider) としてグループ化します。

#### プロキシ MVPD {#proxy-mvpd}

プロキシ MVPD は、他の MVPD に ID サービスを提供する会社で、Adobe Pass Authentication と直接統合されています。

#### プロキシ化された MVPD {#proxied-mvpd}

プロキシされた MVPD は、Adobe Pass Authentication と直接統合されておらず、[ プロキシ MVPD](#proxy-mvpd) を介して統合される会社です。

#### プラットフォーム ID {#platform-identity}

プラットフォーム ID は、一意のプラットフォーム識別子ペイロードで、ユーザーのデバイスにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成され、シングルサインオンユーザーエクスペリエンスを有効にするために [ プログラマー ](#programmer) に提供されます。

詳しくは、[ プラットフォーム ID フローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) ドキュメントを参照してください。

### R {#r}

#### 登録アプリケーション {#registered-application}

登録済みアプリケーションは、[Dynamic Client Registration （DCR](#programmer) プロセスを続行する必要がある [ プログラマー ](#dcr) アプリケーションに関する情報を格納する、Adobe Pass認証コンセプトです。

#### Resource {#resource}

リソースは、ユーザーが [ プログラマー ](#programmer) カタログからアクセスしようとしている、保護されたコンテンツです。

リソースは、プログラマと MVPD の間で合意された一意の値によって識別されます。

詳しくは、[ 保護されたリソースの識別 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md) ドキュメントを参照してください。

### S {#s}

#### SAML {#saml}

SAML （Security Assertion Markup Language）は、当事者間、特に [ID プロバイダ）と [ サービス・プロバイダ ](#sp) との間で認証および認可データを交換するためのオープン ](#identity-provider) 標準です。

#### セカンダリ（プログラマ）アプリケーション {#secondary-application}

セカンダリアプリケーションとは、[ ユーザーエージェント ](#programmer) を使用して [MVPD[ ログインページに移動する ](#user-agent) 認証 ](#authentication) プロセスを完了できる [ プログラマー ](#mvpd) アプリケーションを指します。

セカンダリアプリケーションは、プライマリアプリケーションと同じデバイス上または別の（セカンダリ）デバイス上で実行される場合があります。この場合、ログインエクスペリエンスは、「2 番目の画面認証」ユーザーエクスペリエンスと呼ばれます。

#### サービストークン {#service-token}

サービストークンは、ユーザーにバインドされたサービスまたはフレームワーク（ライブラリ）によって生成される一意のユーザー識別子で、シングルサインオンユーザーエクスペリエンスを有効にするために [ プログラマー ](#programmer) に提供されます。

詳しくは、[ サービストークンフローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) ドキュメントを参照してください。

#### サービスプロバイダー {#service-provider}

サービスプロバイダーとは、[ プログラマー）が所有するチャネル（ブランド ](#programmer) です。

サービスプロバイダーは、プログラマーとAdobeの間のオンボーディングプロセス中に定義される一意の値によって識別されます。

以前に使用された用語 [ 要求者 ID](/help/authentication/kickstart/glossary.md#requestor-id) と同義です。

#### ソフトウェア明細書 {#software-statement}

このソフトウェアステートメントは、Adobe Pass[TVE Dashboard](#tve-dashboard) からダウンロードできる JSON web トークン（JWT）であり、[Dynamic Client Registration （DCR） ](#dcr) プロセスの一部として使用することを目的としています。

#### SLO {#slo}

シングルログアウト（SLO）は、[ シングルサインオン（SSO） ](#sso) に含まれるすべてのアプリケーションからユーザーがログアウトできるプロセスです。

#### SP {#sp}

サービスプロバイダー（SP）とは、[MVPD](#mvpd) との統合において、[ プログラマー ](#programmer) に代わってAdobe Pass認証が果たす役割を指します。

#### SSO {#sso}

シングルサインオン（SSO）は、ユーザーが 1 回認証すると、複数の [ プログラマー ](#programmer) アプリケーションのそれぞれに対して認証を行う必要なく、それらのアプリケーションにアクセスできるプロセスです。

### T {#t}

#### TempPass Basic {#temp-pass-basic}

基本的な TempPass は、[MVPD](#mvpd) による認証を必要とせずに、ユーザーが保護されたコンテンツに限られた時間アクセスできるようにするAdobe Pass認証機能です。

詳しくは、[Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) ドキュメントを参照してください。

#### TempPass プロモーション {#temp-pass-promotional}

プロモーション用 TempPass は、Adobe Pass認証機能であり、[MVPD](#mvpd) を使用して認証する必要なく、保護されたコンテンツにアクセスできるリソースの最大数と時間を制限できます。

詳しくは、[ プロモーション一時パス ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md) ドキュメントを参照してください。

#### TTL {#ttl}

有効期間（TTL）は、基になるエンティティが有効な時間を示す値です。

TTL は、[ アクセストークン ](#access-token)、[ プロファイル ](#profile)、認証 [ 決定 ](#decision) または [ メディアトークン ](#media-token) について言及できます。

#### TVE {#tve}

TV Everywhere （TVE）は、消費者がスマートフォン、タブレット、ノートパソコンなどの複数のデバイスで、お気に入りのテレビ番組、映画、その他のコンテンツにアクセスできるようにする業界ニッチです。

#### TVE ダッシュボード {#tve-dashboard}

TV Everywhere （TVE）ダッシュボードは、Adobe Passの設定とデータを管理するために [ プログラマー ](#programmer) に提供される TV Authentication Tool です。

詳しくは、[TVE ダッシュボードユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) ドキュメントを参照してください。

#### TV プロバイダ {#tv-provider}

TV プロバイダーは、ケーブル、衛星、またはインターネットベースのサービスを通じて消費者にテレビサービスを提供する会社です。

TV プロバイダーは、TV プロバイダーとAdobeの間のオンボーディングプロセス中に定義された一意の値によって識別されます。

[MVPD](#mvpd) および [ID プロバイダー ](#identity-provider) と同義。

### U {#u}

#### ユーザーエージェント {#user-agent}

ユーザーエージェントは、Web 上を移動して（MVPD](#mvpd) ログインページをレンダリングできる、ブラウザーまたは類似のコンポーネント [ プラットフォーム固有）を参照します。

#### ユーザーメタデータ {#user-metadata}

ユーザーメタデータとは、ユーザー固有の属性（郵便番号、保護者の制限、ユーザー ID など）を指し、[MVPD](#mvpd) によって維持管理され、Adobe Pass認証が [ プロファイル ](#profile) の一部として提供します。

詳しくは、[ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md) ドキュメントを参照してください。

### V {#v}

#### VSA {#vsa}

ビデオ購読者アカウント（VSA）は、シングルサインオンユーザーエクスペリエンスを可能にするために [ プログラマー ](#programmer) に提供される、Appleが開発したフレームワークです。

詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) および [ パートナーフローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) ドキュメントを参照してください。
