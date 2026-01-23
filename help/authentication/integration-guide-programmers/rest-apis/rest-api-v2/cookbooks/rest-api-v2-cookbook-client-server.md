---
title: REST API V2 クックブック（クライアントからサーバー）
description: REST API V2 クックブック（クライアントからサーバー）
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1842'
ht-degree: 0%

---

# REST API V2 クックブック（クライアントからサーバー） {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

このドキュメントは、[Adobe Pass認証 REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) を、クライアントからサーバー（C2S）アーキテクチャを持つストリーミングアプリケーションに統合する開発者向けです。

## 前提条件 {#prerequisites}

用語と定義については、[REST API V2 用語集 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) ドキュメントを参照してください。

必須の要件と推奨事項については、[REST API V2 チェックリスト &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md) ドキュメントを参照してください。

よくある質問については、[REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md) ドキュメントを参照してください。

## A.登録フェーズ {#registration-phase}

登録フェーズの目的は、Dynamic Client Registration （DCR）プロセスを通じて、ストリーミングアプリケーションをAdobe Pass認証に登録することです。

動的クライアント登録（DCR）プロセスでは、登録フェーズの最終目標として、クライアント資格情報のペアを取得し、アクセストークンを取得するために、ストリーミングアプリケーションが必要です。

登録フェーズは必須ですが、クライアント資格情報とアクセストークンのペアがキャッシュされていて、まだ有効な場合、ストリーミングアプリケーションはこのフェーズをスキップできます。

+++関連記事

**API:**

* [クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**フロー：**

* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQ:**

* [登録フェーズに関するよくある質問](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 手順 1：アプリケーションを登録 {#step-1-register-your-application}

* **クライアント資格情報の取得：** ストリーミングアプリケーションは、[**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) エンドポイントを呼び出して、クライアント資格情報を取得します。

   * ストリーミングアプリケーションは、アクセストークンを取得する必要がある場合、クライアント資格情報を保存して無期限に使用する必要があります。


* **アクセストークンの取得：** ストリーミングアプリケーションは、[**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) エンドポイントを呼び出してアクセストークンを取得します。

   * ストリーミングアプリケーションは、有効期限が切れるまでアクセストークンを保存して使用し、その後、破棄して新しいアクセストークンを取得する必要があります。

## B.認証フェーズ {#authentication-phase}

認証フェーズの目的は、ストリーミングアプリケーションにユーザーの ID を検証し、ユーザーメタデータ情報を取得する機能を提供することです。

認証フェーズは、ストリーミングアプリケーションでコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件となる手順となります。

+++関連記事

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

+++

### 手順 2：既存の認証済みプロファイルを確認する {#step-2-check-for-existing-authenticated-profiles}

* **プロファイルの取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) エンドポイントを呼び出して、既存のプロファイルを確認します。


* **シナリオ 1:** 既存のプロファイルがある場合、ストリーミングアプリケーションは [&#x200B; 事前認証フェーズ &#x200B;](#preauthorization-phase) または [&#x200B; 認証フェーズ &#x200B;](#authorization-phase) に進みます。


* **シナリオ 2:** 既存のプロファイルがありません。ストリーミングアプリケーションは次の手順に進んで [&#x200B; ユーザーの認証 &#x200B;](#step-3-authenticate-the-user) を行う場合があります。


* **シナリオ 3:** 既存のプロファイルがありません。ストリーミングアプリケーションは、[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 機能を通じてユーザーに一時的なアクセスを提供する場合があります。

   * このシナリオはこのドキュメントの範囲外です。詳しくは、[&#x200B; 一時的なアクセスフロー &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) のドキュメントを参照してください。

### 手順 3：ユーザーの認証 {#step-3-authenticate-the-user}

* **設定の取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) エンドポイントを呼び出して、使用可能な MVPD のリストを取得します。

   * ストリーミングアプリケーションは、カスタムフィルタリングメカニズムを実装して、設定応答から MVPD のリストを絞り込み、対象のプロバイダーのみを表示し、その他のプロバイダー（開発中の MVPD、テスト MVPD、TempPass など）は非表示にできます。 これにより、テレビプロバイダーを選択する際に、厳選された選択肢がユーザーに表示されます。


* **認証セッションの作成：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントを呼び出して認証セッションを開始します。


* **シナリオ 1:** ストリーミングアプリケーションは、ブラウザーまたは web ビューを開くことができるので、認証 `url` ールを読み込む必要があります。

   * ユーザーは、MVPDのログインページ内でユーザー名とパスワードを送信します。 認証が成功すると、最終的なリダイレクトに成功ページが表示されます。


* **シナリオ 2:** ストリーミングアプリケーションはブラウザーを開くことができないので、認証 `code` を表示する必要があります。 ユーザーに `code` を入力し、認証 `url` を作成して [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) を開くよう促すには、別の web アプリケーションが必要です。

   * ユーザーは、MVPDのログインページ内でユーザー名とパスワードを送信します。 認証が成功すると、最終的なリダイレクトに成功ページが表示されます。

### 手順 4：認証済みプロファイルを確認 {#step-4-check-for-authenticated-profiles}

* **特定のコードのプロファイルを取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) エンドポイントを呼び出してプロファイルが正常に生成され保存されたかどうかを確認するために、`code` を使用してポーリングメカニズムを実装する必要があります。

   * ストリーミングアプリケーションは、次の条件下で **ポーリングを開始** メカニズムを使用する必要があります。

      * **プライマリ（画面）アプリケーション内で実行される認証：** プライマリ（ストリーミング）アプリケーションは、ユーザーが最終的な宛先ページに到達したときに、ブラウザーコンポーネントが [&#x200B; セッション &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントリクエストの `redirectUrl` パラメーターに指定された URL を読み込んだ後、ポーリングを開始する必要があります。

      * **セカンダリ（画面）アプリケーション内で実行される認証：** プライマリ（ストリーミング）アプリケーションは、ユーザーが認証プロセスを開始するとすぐに（[&#x200B; セッション &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答を受信し、認証コードをユーザーに表示した直後に）、ポーリングを開始する必要があります。

   * ストリーミングアプリケーションは、次の条件下で **ポーリングを停止** する必要があります。

      * **認証の成功：** ユーザーのプロファイル情報は正常に取得され、認証ステータスが確認されます。 この時点で、ポーリングは不要になります。

      * **認証セッションとコードの有効期限：** 認証セッションとコードの有効期限が切れます。これは、[&#x200B; セッション &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答の `notAfter` タイムスタンプ （例：30 分）に示されています。 この場合、ユーザーは認証プロセスを再起動する必要があり、以前の認証コードを使用したポーリングは直ちに停止する必要があります。

      * **生成された新しい認証コード：** ユーザーがプライマリ（画面）デバイスで新しい認証コードを要求すると、既存のセッションは無効になり、以前の認証コードを使用したポーリングを直ちに停止する必要があります。

   * ストリーミングアプリケーションは、次の条件下で **ポーリングを設定** メカニズムの頻度を設定する必要があります。

      * **プライマリ（画面）アプリケーション内で実行される認証：** プライマリ（ストリーミング）アプリケーションは、3～5 秒以上ごとにポーリングする必要があります。

      * **セカンダリ（画面）アプリケーション内で実行される認証：** プライマリ（ストリーミング）アプリケーションは、3～5 秒以上ごとにポーリングする必要があります。

   * ストリーミングアプリケーションは、不要なリクエストを回避してユーザーエクスペリエンスを向上させるために、ユーザーのプロファイル情報の一部を永続的なストレージにキャッシュする必要があります。

## C. （オプション）事前認証フェーズ {#preauthorization-phase}

事前認証フェーズの目的は、ユーザーがアクセスする権利を持つカタログからリソースのサブセットを提示できるストリーミングアプリケーションを提供することです。

事前認証フェーズでは、ユーザーが初めてストリーミングアプリケーションを開いた場合や、新しいセクションに移動した場合のユーザーエクスペリエンスが向上します。

事前認証フェーズは必須ではありません。ストリーミングアプリケーションは、ユーザーの使用権限に基づいて最初にリソースのカタログをフィルタリングせずに提示する場合、このフェーズをスキップできます。

+++関連記事

**API**

* [事前認証の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**よくある質問**

* [事前認証フェーズの FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 手順 5：事前承認済みリソースの確認 {#step-5-check-for-preauthorized-resources}

* **事前認証決定の取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) エンドポイントを呼び出して、リソースのリストの事前認証決定を取得します。

   * ストリーミングアプリケーションは、永続的なストレージに事前認証の決定を保存する必要はありません。 ただし、ユーザーエクスペリエンスを向上させるために、許可決定をメモリにキャッシュすることをお勧めします。 これにより、事前に承認されたリソースに対する不要な呼び出しを回避し、待ち時間を短縮し、パフォーマンスを向上させることができます。

   * ストリーミングアプリケーションは、決定の事前承認エンドポイントからの応答に含まれる [&#x200B; エラーコードとメッセージ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を調べることで、拒否された事前承認の決定の理由を判断できます。 これらの詳細により、事前認証リクエストが拒否された具体的な理由がinsightに提供され、アプリケーションでの必要な処理をユーザーエクスペリエンスやトリガーに伝えるのに役立ちます。 事前認証の決定を取得するために実装された再試行メカニズムが、事前認証の決定が拒否された場合に無限ループが発生しないことを確認します。 再試行を適切な数に制限し、ユーザーに明確なフィードバックを表示して、拒否を適切に処理することを検討してください。

   * ストリーミングアプリケーションは、MVPD によって課せられる条件により、1 回の API リクエストで限られた数のリソースに対して、通常は最大 5 の事前認証決定を取得できます。 この最大数のリソースは、組織の管理者の 1 人がAdobe Pass [TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を通じて、またはお客様に代わってAdobe Pass認証担当者が MVPD に同意した後に表示および変更できます。


## D.承認フェーズ {#authorization-phase}

認証フェーズの目的は、ユーザーがMVPDで権限を検証した後でリクエストしたリソースをストリーミングアプリケーションで再生できるようにすることです。

認証フェーズは必須です。ストリーミングアプリケーションは、ユーザーがリクエストするリソースを再生する場合、このフェーズをスキップできません。ストリームをリリースする前に、ユーザーが権限を持っていることをMVPDで確認する必要があるからです。

+++関連記事

**API**

* [承認の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**よくある質問**

* [承認フェーズに関するよくある質問](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 手順 6：許可されたリソースの確認 {#step-6-check-for-authorized-resources}

* **認証決定の取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) エンドポイントを呼び出して、特定のリソースの認証決定を取得します。

   * ストリーミングアプリケーションは、認証決定を永続ストレージに保存するために必要ではありません。

   * ストリーミングアプリケーションは、決定を承認エンドポイントからの応答に含まれる [&#x200B; エラーコードとメッセージ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を調べることで、拒否された承認決定の理由を判断できます。 これらの詳細により、認証リクエストが拒否された具体的な理由がinsightに提供され、アプリケーションでの必要な処理をユーザーエクスペリエンスやトリガーに伝えるのに役立ちます。 認証決定を取得するために実装された再試行メカニズムが、認証決定が拒否された場合に無限ループが発生しないことを確認します。 再試行を適切な数に制限し、ユーザーに明確なフィードバックを表示して、拒否を適切に処理することを検討してください。

   * ストリーミング アプリケーションは、ストリームのアクティブな再生中に期限切れのメディア トークンを更新する必要はありません。 再生中にメディアトークンの有効期限が切れた場合、ストリームが中断されることなく続行されるようにしてください。 ただし、次回ユーザーがリソースを再生しようとすると、クライアントは新しい認証決定をリクエストし、新しいメディアトークンを取得する必要があります。

   * ストリーミングアプリケーションは、MVPD によって課せられる条件により、1 回の API リクエストで限られた数のリソースに対する認証決定を、通常は 1 つまで取得できます。

## E. ログアウトフェーズ {#logout-phase}

ログアウトフェーズの目的は、ユーザーのリクエストに応じてAdobe Pass Authentication 内でユーザーの認証済みプロファイルを終了できるストリーミングアプリケーションを提供することです。

ログアウトフェーズは必須です。ストリーミングアプリケーションは、ログアウト機能をユーザーに提供する必要があります。

+++関連記事

**API**

* [特定の mvpd に対するログアウトの開始](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的なログアウトフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**よくある質問**

* [ログアウトフェーズに関するよくある質問](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 手順 7：ログアウト {#step-7-logout}

* **Adobe Pass ログアウトの開始：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) エンドポイントを呼び出して、ログアウトフローを開始します。

   * ストリーミングアプリケーションは、ログアウトプロセスが正しく完了するように、ログアウトエンドポイント応答の `actionName` 属性と `actionType` 属性に記載されている指示に従う必要があります。

      * 応答の `actionType` 属性が「interactive」に設定されている場合：

         * **シナリオ 1:** ストリーミングアプリケーションは、ブラウザーまたは web ビューを開くことができるので、ログアウト `url` を読み込む必要があります。

         * **シナリオ 2:** ストリーミングアプリケーションでブラウザーを開くことができないので、MVPD セッションがストリーミングデバイスブラウザーのキャッシュに保持されていなかったので、ログアウトプロセスを停止できます。
