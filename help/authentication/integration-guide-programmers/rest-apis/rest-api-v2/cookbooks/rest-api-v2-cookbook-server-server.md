---
title: REST API V2 クックブック（サーバー間）
description: REST API V2 クックブック（サーバー間）
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2510'
ht-degree: 0%

---

# REST API V2 クックブック（サーバー間） {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

このドキュメントは、[Adobe Pass認証 REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) を、サーバー間（S2S）アーキテクチャを持つストリーミングアプリケーションに統合する開発者向けです。

## 前提条件 {#prerequisites}

用語と定義については、[REST API V2 用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) ドキュメントを参照してください。

必須の要件と推奨事項については、[REST API V2 チェックリスト ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) ドキュメントを参照してください。

よくある質問については、[REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) ドキュメントを参照してください。

### コンポーネント {#components}

開始する前に、ワークフローで使用される次のコンポーネントと用語について理解していることを確認してください。

| タイプ | コンポーネント | 説明 |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe インフラストラクチャ | Adobe Pass サービス | はMVPD IdP および AuthZ サービスと統合され、認証と承認の決定を行います。 |
| プログラマ基盤 | プログラマーサービス | は、ストリーミングデバイスをAdobe Pass サービスに接続して、認証済みのプロファイルと承認の決定を取得します。 |
| MVPD インフラストラクチャ | MVPD IdP サービス | 認証情報ベースの認証を担当するMVPD エンドポイントは、ユーザーの ID を検証します。 |
|                           | MVPD AuthZ サービス | ユーザーのサブスクリプション、保護者による制限、その他の使用権限規則に基づいて認証の決定を行うMVPD エンドポイント。 |
| ストリーミングデバイス | ストリーミングアプリ | ユーザーのストリーミングデバイス上で実行され、認証済みのビデオコンテンツを再生するプログラマーのアプリケーションです。 |
|                           | （オプション） AuthN モジュール | ストリーミングデバイスに user-agent （ブラウザーなど）が含まれている場合、AuthN モジュールはMVPD IdP でユーザー認証を処理します。 |
| （オプション） AuthN デバイス | AuthN アプリ | ストリーミングデバイスにユーザーエージェント（ブラウザーなど）がない場合、AuthN アプリケーションは、web ブラウザーを介して別のデバイスからアクセスするプログラマー Web アプリです。 |

### 要件 {#requirements}

サーバー間（S2S）実装では、ストリーミングアプリとプログラマーサービスは、プログラマーサービスが次の操作を行えるようにするプロトコルを確立する必要があります。

* ストリーミングアプリに代わってAdobe Pass サービスと通信します。

* [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ヘッダーで要求されるストリーミングデバイスの一意のデバイス識別子を収集して渡します。

* [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ヘッダーの要求に応じて、ソースポートやデバイス固有の詳細など、正確なストリーミングデバイス情報を収集して渡します。

* X-Forwarded-For ヘッダーで必要とされるストリーミングデバイスの IP アドレスを収集して渡します。

* デバイス ID、クライアント ID、クライアントシークレットなどのパラメーターは、ストリーミングアプリまたはプログラマーサービスのいずれかに安全に保存されます。

* デバイス IP、ソース ポート、デバイス固有情報、MRSS、ECID などのオプション識別子など、MVPD および統合アプリケーションに準拠した形式でデータを送信します。

* 暗号化されたユーザーメタデータの送信のために、Adobeで共有される証明書を維持し、安全に管理します。

* キャッシュ時に認証プロファイルと承認決定の有効性を考慮し、通知時に認証状態と承認状態が無効化されるようにします。

* 認証の決定と関連する指示をストリーミングアプリに返します。

### 環境 {#environments}

ワークフローに進む前に、少なくとも 2 つの環境（実稼動環境とステージング環境）を維持していることを確認してください。

**実稼動**

実稼動環境は、ライブスポーツイベントやニュース速報などによる大きなトラフィックスパイクや予期しないトラフィックスパイクに対応するために、高可用性と適切なスケーリングを備える必要があります。

* Adobe Pass サービスは、米国全土に分散した複数のデータセンターで動作し、パフォーマンスを最適化し、待ち時間を最小限に抑えます。

   * プログラマーサービスは、Adobe Passから低遅延の応答時間を確保するために、同様のインフラストラクチャ戦略を採用する必要があります。

* プログラマーは、実稼動環境の公開 IP 範囲を指定する必要があります。

   * これらの IP は、Adobe Pass インフラストラクチャ内の許可リストに追加されます。

* プログラマーサービスは、データセンターが利用できなくなったことにより、Adobeでトラフィックのリダイレクトが必要になった場合に、動的な再ルーティングを可能にするために、DNS キャッシングを最大 30 秒に制限する必要があります。

**ステージング**

ステージング環境は最小限に抑えることができますが、すべての重要なシステムコンポーネントとビジネスロジックを含めることで、実稼動環境をミラーリングする必要があります。

* 実稼動環境にデプロイする前に、リリースのテストを許可する必要があります。

* 実際のテストが可能な限り、実稼動環境と同様の操作性を維持する必要があります。

* ステージング環境は、次の目的でAdobe Pass テスト環境に接続するのが理想的です。

   * プログラマーがAdobe インフラストラクチャに対してテストできるようにします。

   * 必要に応じて、Adobeを有効にして、テストとトラブルシューティングを支援します。

## ワークフロー {#workflow}

次の図に示す手順を実行します。

![REST API V2 クックブック（サーバー間） ](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*REST API V2 クックブック（サーバー間）*

## A.登録フェーズ {#registration-phase}

登録フェーズの目的は、Dynamic Client Registration （DCR）プロセスを通じて、ストリーミングアプリケーションをAdobe Pass認証に登録することです。

動的クライアント登録（DCR）プロセスでは、登録フェーズの最終目標として、クライアント資格情報のペアを取得し、アクセストークンを取得するために、ストリーミングアプリケーションが必要です。

登録フェーズは必須ですが、クライアント資格情報とアクセストークンのペアがキャッシュされていて、まだ有効な場合、ストリーミングアプリケーションはこのフェーズをスキップできます。

+++関連記事

API:

* [クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

フロー：

* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

よくある質問：

* [登録フェーズに関するよくある質問](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 手順 1：アプリケーションを登録 {#step-1-register-your-application}

* クライアント資格情報の取得：プログラマーサービスは、[**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) エンドポイントを呼び出して、クライアント資格情報を取得します。

   * プログラマーサービスまたはプログラマーアプリは、アクセストークンを取得する必要がある場合、クライアント資格情報を保存し、無期限に使用する必要があります。


* アクセストークンの取得：プログラマーサービスは、[**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) エンドポイントを呼び出して、アクセストークンを取得します。

   * プログラマーサービスまたはプログラマーアプリは、アクセストークンを期限切れになるまで保存して使用し、破棄して新しいアクセストークンを取得する必要があります。

## B.認証フェーズ {#authentication-phase}

認証フェーズの目的は、ストリーミングアプリケーションにユーザーの ID を検証し、ユーザーメタデータ情報を取得する機能を提供することです。

認証フェーズは、ストリーミングアプリケーションでコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件となる手順となります。

+++関連記事

API

* [認証セッションの作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッションの再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [認証セッションの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [ユーザーエージェントでの認証の実行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の mvpd のプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

フロー

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリ・アプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

よくある質問

* [認証フェーズの FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### 手順 2：既存の認証済みプロファイルを確認する {#step-2-check-for-existing-authenticated-profiles}

* **プロファイルの取得：** プログラマーサービスは、[**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) エンドポイントを呼び出すことにより、ストリーミングアプリの代わりに既存のプロファイルを確認します。


* **シナリオ 1:** 既存のプロファイルがあり、プログラマーサービスが [ 事前認証フェーズ ](#preauthorization-phase) または [ 認証フェーズ ](#authorization-phase) に進む場合があります。


* **シナリオ 2:** 既存のプロファイルがありません。プログラマーサービスは、次の手順に進んで [ ユーザーの認証 ](#step-3-authenticate-the-user) を行う場合があります。


* **シナリオ 3:** 既存のプロファイルがありません。プログラマーサービスは、[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 機能を通じてユーザーに一時的なアクセスを提供する場合があります。

   * このシナリオはこのドキュメントの範囲外です。詳しくは、[ 一時的なアクセスフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) のドキュメントを参照してください。

### 手順 3：ユーザーの認証 {#step-3-authenticate-the-user}

* **設定の取得：** プログラマーサービスは、[**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) エンドポイントを呼び出して、使用可能な MVPD のリストを取得します。

   * プログラマーサービスは、カスタムフィルタリングメカニズムを実装して、設定応答から MVPD のリストを絞り込むことができます。これにより、ストリーミングアプリは対象のプロバイダーのみを表示し、それ以外のプロバイダー（開発中の MVPD、テスト MVPD、TempPass など）は非表示にします。 これにより、テレビプロバイダーを選択する際に、厳選された選択肢がユーザーに表示されます。


* **認証セッションの作成：** プログラマーサービスは、[**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントを呼び出して認証セッションを開始します。

   * プログラマーサービスは、`code` と `url` をストリーミングアプリに返す必要があります。


* **シナリオ 1:** ストリーミングアプリは、ブラウザーまたは web ビューを開くことができるので、認証 `url` ールを読み込む必要があります。

   * ユーザーは、MVPDのログインページ内でユーザー名とパスワードを送信します。 認証が成功すると、最終的なリダイレクトに成功ページが表示されます。


* **シナリオ 2:** ストリーミングアプリはブラウザーを開くことができないので、認証 `code` ールを表示する必要があります。 ユーザーに `code` を入力し、認証 `url` を作成して [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) を開くよう促すには、別の web アプリケーションが必要です。

   * ユーザーは、MVPDのログインページ内でユーザー名とパスワードを送信します。 認証が成功すると、最終的なリダイレクトに成功ページが表示されます。

### 手順 4：認証済みプロファイルを確認 {#step-4-check-for-authenticated-profiles}

* **特定のコードのプロファイルを取得：** プログラマーサービスは、`code` を使用してポーリングメカニズムを実装し、[**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) エンドポイントを呼び出して、プロファイルが正常に生成され保存されたかどうかを確認する必要があります。

   * プログラマーサービスは、次の条件下で **ポーリングを開始** する必要があります。

      * **プライマリ（画面）アプリケーション内で実行される認証：** ブラウザーコンポーネントが [ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントリクエストの `redirectUrl` パラメーターに指定された URL を読み込んだ後、ユーザーが最終的な宛先ページに到達すると、プログラマーサービスはポーリングを開始する必要があります。

      * **セカンダリ（画面）アプリケーション内で実行される認証：** プログラマーサービスアプリケーションは、ユーザーが認証プロセスを開始するとすぐに（[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答を受信し、認証コードをユーザーに表示した直後に）、ポーリングを開始する必要があります。

   * プログラマーサービスは、次の条件下で **ポーリングを停止** する必要があります。

      * **認証の成功：** ユーザーのプロファイル情報は正常に取得され、認証ステータスが確認されます。 この時点で、ポーリングは不要になります。

      * **認証セッションとコードの有効期限：** 認証セッションとコードの有効期限が切れます。これは、[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答の `notAfter` タイムスタンプ （例：30 分）に示されています。 この場合、ユーザーは認証プロセスを再起動する必要があり、以前の認証コードを使用したポーリングは直ちに停止する必要があります。

      * **生成された新しい認証コード：** ユーザーがプライマリ（画面）デバイスで新しい認証コードを要求すると、既存のセッションは無効になり、以前の認証コードを使用したポーリングを直ちに停止する必要があります。

   * プログラマーサービスは、次の条件下で **ポーリングを設定** メカニズムの頻度を設定する必要があります。

      * **プライマリ（画面）アプリケーション内で実行される認証：** プログラマーサービスは、3 ～ 5 秒ごとにポーリングする必要があります。

      * **セカンダリ（画面）アプリケーション内で実行される認証：** プログラマーサービスは、3 ～ 5 秒ごとにポーリングする必要があります。

   * プログラマーサービスは、不要なリクエストを避け、ユーザーエクスペリエンスを向上させるために、ユーザーのプロファイル情報の一部を永続的なストレージにキャッシュする必要があります。

## C. （オプション）事前認証フェーズ {#preauthorization-phase}

事前認証フェーズの目的は、ユーザーがアクセスする権利を持つカタログからリソースのサブセットを提示できるストリーミングアプリケーションを提供することです。

事前認証フェーズでは、ユーザーが初めてストリーミングアプリケーションを開いた場合や、新しいセクションに移動した場合のユーザーエクスペリエンスが向上します。

事前認証フェーズは必須ではありません。ストリーミングアプリケーションは、ユーザーの使用権限に基づいて最初にリソースのカタログをフィルタリングせずに提示する場合、このフェーズをスキップできます。

+++関連記事

API

* [事前認証の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

フロー

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

よくある質問

* [事前認証フェーズの FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 手順 5：事前承認済みリソースの確認 {#step-5-check-for-preauthorized-resources}

* **事前認証決定の取得：** プログラマーサービスは、[**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) エンドポイントを呼び出して、リソースのリストの事前認証決定を取得します。

   * プログラマーサービスは、許可および拒否の事前承認決定のリストをストリーミングアプリに渡す必要があります。

   * プログラマーサービスは、永続的なストレージに事前認証の決定を保存する必要はありません。 ただし、ユーザーエクスペリエンスを向上させるために、許可決定をメモリにキャッシュすることをお勧めします。 これにより、事前に承認されたリソースに対する不要な呼び出しを回避し、待ち時間を短縮し、パフォーマンスを向上させることができます。

   * プログラマーサービスは、決定の事前承認エンドポイントからの応答に含まれる [ エラーコードとメッセージ ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を調べることで、拒否された事前承認の決定の理由を判断できます。 これらの詳細は、事前認証リクエストが拒否された具体的な理由に関するインサイトを提供し、アプリケーションでの必要な処理をユーザーエクスペリエンスやトリガーに知らせるのに役立ちます。 事前認証の決定を取得するために実装された再試行メカニズムが、事前認証の決定が拒否された場合に無限ループが発生しないことを確認します。 再試行を適切な数に制限し、ユーザーに明確なフィードバックを表示して、拒否を適切に処理することを検討してください。

   * プログラマーサービスは、MVPD によって課された条件により、1 回の API リクエストで、通常は 5 回まで、限られた数のリソースに対して事前認証の決定を取得できます。 この最大数のリソースは、組織の管理者の 1 人がAdobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を通じて、またはお客様に代わってAdobe Pass認証担当者が MVPD に同意した後に表示および変更できます。

## D.承認フェーズ {#authorization-phase}

認証フェーズの目的は、ユーザーがMVPDで権限を検証した後でリクエストしたリソースをストリーミングアプリケーションで再生できるようにすることです。

認証フェーズは必須です。ストリーミングアプリケーションは、ユーザーがリクエストするリソースを再生する場合、このフェーズをスキップできません。ストリームをリリースする前に、ユーザーが権限を持っていることをMVPDで確認する必要があるからです。

+++関連記事

API

* [承認の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

フロー

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

よくある質問

* [承認フェーズに関するよくある質問](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 手順 6：許可されたリソースの確認 {#step-6-check-for-authorized-resources}

* **認証決定の取得：** プログラマーサービスは、[**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) エンドポイントを呼び出して、ストリーミングアプリによって渡される特定のリソースの認証決定を取得します。

   * プログラマーサービスは、認証決定を永続的なストレージに保存する必要はありません。

   * プログラマーサービスは、決定を承認エンドポイントからの応答に含まれている [ エラーコードとメッセージ ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を調べることで、拒否された承認決定の理由を判断できます。 これらの詳細は、認証リクエストが拒否された具体的な理由をinsightに提供し、ストリーミングアプリでの必要な処理をユーザーエクスペリエンスやトリガーに伝えるのに役立ちます。 認証決定を取得するために実装された再試行メカニズムが、認証決定が拒否された場合に無限ループが発生しないことを確認します。 再試行を適切な数に制限し、ユーザーに明確なフィードバックを表示して、拒否を適切に処理することを検討してください。

   * プログラマーサービスは、他のビジネスルールを評価し、ストリーミングアプリに適切な承認決定を返す場合があります。

   * ストリームのアクティブ再生中に期限切れのメディアトークンを更新する場合、プログラマーサービスは必要ありません。 再生中にメディアトークンの有効期限が切れた場合、ストリームが中断されることなく続行されるようにしてください。 ただし、次回ユーザーがリソースを再生しようとすると、クライアントは新しい認証決定をリクエストし、新しいメディアトークンを取得する必要があります。

   * プログラマーサービスは、MVPD によって課された条件により、1 回の API リクエストで限られた数のリソースに対して、通常は 1 つまでの承認決定を取得できます。

## E. ログアウトフェーズ {#logout-phase}

ログアウトフェーズの目的は、ユーザーのリクエストに応じてAdobe Pass Authentication 内でユーザーの認証済みプロファイルを終了できるストリーミングアプリケーションを提供することです。

ログアウトフェーズは必須です。ストリーミングアプリケーションは、ログアウト機能をユーザーに提供する必要があります。

+++関連記事

API

* [特定の mvpd に対するログアウトの開始](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

フロー

* [プライマリアプリケーション内で実行される基本的なログアウトフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

よくある質問

* [ログアウトフェーズに関するよくある質問](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 手順 7：ログアウト {#step-7-logout}

* Adobe Pass ログアウトの開始：プログラマーサービスは、[/api/v2/{serviceProvider}/logout/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) エンドポイントを呼び出して、ストリーミングアプリからリクエストされたログアウトフローを開始します。

   * プログラマーサービスは、認証済みユーザーに関する情報を保存している場合、その情報をクリーンアップすることができます。

   * プログラマーサービスは、ログアウトプロセスが正しく完了するように、ログアウトエンドポイント応答の `actionName` 属性と `actionType` 属性に記載されている手順に従う必要があります。

      * 応答の `actionType` 属性が「interactive」に設定されている場合、プログラマーサービスは `url` 属性値をストリーミングアプリに返す必要があります。

         * **シナリオ 1:** ストリーミングアプリは、ブラウザーまたは web ビューを開くことができるので、ログアウト `url` を読み込む必要があります。

         * **シナリオ 2:** ストリーミングアプリでブラウザーを開くことができないので、MVPD セッションがストリーミングデバイスブラウザーのキャッシュに保持されていなかったので、ログアウトプロセスを停止できます。
