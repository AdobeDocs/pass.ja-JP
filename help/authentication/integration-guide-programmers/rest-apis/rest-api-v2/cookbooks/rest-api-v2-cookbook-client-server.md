---
title: REST API V2 クックブック （クライアント間）
description: REST API V2 クックブック （クライアント間）
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1842'
ht-degree: 0%
---
# REST API V2 クックブック （クライアント間） {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> REST API V2の実装は、[ スロットル メカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)のドキュメントによって制限されています。

このドキュメントは、[Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)を、クライアント間（C2S）アーキテクチャを持つストリーミングアプリケーションに統合する開発者向けです。

## 前提条件 {#prerequisites}

用語と定義については、[REST API V2 Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)のドキュメントを参照してください。

必須の要件と推奨されるプラクティスについては、[REST API V2 チェックリスト ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md)のドキュメントを参照してください。

よくある質問については、[REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)のドキュメントを参照してください。

## イ。登録段階 {#registration-phase}

登録フェーズの目的は、Dynamic Client Registration （DCR）プロセスを通じて、Adobe Pass Authenticationに対してストリーミングアプリケーションを登録することです。

動的クライアント登録（DCR）プロセスでは、ストリーミングアプリケーションが登録フェーズの最終目標として、1組のクライアント資格情報を取得し、アクセストークンを取得する必要があります。

登録フェーズは必須ですが、ストリーミングアプリケーションがキャッシュされたクライアント資格情報のペアと有効なアクセストークンを持っている場合、このフェーズをスキップできます。

+++関連する記事

**API:**

* [クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**フロー：**

* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**FAQ:**

* [登録フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 手順1：アプリケーションの登録 {#step-1-register-your-application}

* **クライアント資格情報の取得：** ストリーミングアプリケーションは、[**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) エンドポイントを呼び出して、クライアント資格情報を取得します。

  * ストリーミングアプリケーションは、クライアント資格情報を保存し、アクセストークンを取得する必要がある場合に無期限に使用する必要があります。


* **アクセストークンの取得：** ストリーミングアプリケーションは、[**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) エンドポイントを呼び出してアクセストークンを取得します。

  * ストリーミングアプリケーションは、有効期限が切れるまでアクセストークンを保存して使用し、それを破棄して新しいトークンを取得する必要があります。

## B.認証フェーズ {#authentication-phase}

認証フェーズの目的は、ストリーミングアプリケーションにユーザーのIDを検証し、ユーザーメタデータ情報を取得する機能を提供することです。

認証フェーズは、ストリーミングアプリケーションがコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件ステップとして機能します。

+++関連する記事

**API**

* [認証セッションの作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッションを再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [認証セッションの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [ユーザーエージェントで認証を実行する](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のmvpdのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**フロー**

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本的なプロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**FAQ**

* [認証フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### 手順2：既存の認証プロファイルの確認 {#step-2-check-for-existing-authenticated-profiles}

* **プロファイルの取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) エンドポイントを呼び出して、既存のプロファイルをチェックします。


* **シナリオ 1:**&#x200B;既存のプロファイルがあります。ストリーミングアプリケーションは、[事前承認フェーズ ](#preauthorization-phase)または[承認フェーズ ](#authorization-phase)に進むことができます。


* **シナリオ 2:**&#x200B;既存のプロファイルがありません。ストリーミングアプリケーションは、次の手順に進んで[ ユーザーの認証](#step-3-authenticate-the-user)を行うことができます。


* **シナリオ 3:**&#x200B;既存のプロファイルがありません。ストリーミングアプリケーションは、[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)機能を通じて、ユーザーに一時的なアクセスを提供するために続行できます。

  * このシナリオは、このドキュメントの範囲外です。詳しくは、[一時アクセスフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) ドキュメントを参照してください。

### 手順3：ユーザーの認証 {#step-3-authenticate-the-user}

* **設定の取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) エンドポイントを呼び出して、使用可能なMVPDのリストを取得します。

  * ストリーミングアプリケーションは、カスタムフィルタリングメカニズムを実装して、設定応答からMVPDのリストを絞り込み、目的のプロバイダーのみを表示しながら、他のプロバイダー（開発中のMVPD、テスト MVPD、TempPassなど）を非表示にすることができます。 これにより、テレビ事業者を選択する際に、オーディエンスに厳選された選択肢が提示されます。


* **認証セッションの作成：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントを呼び出して、認証セッションを開始します。


* **シナリオ 1:** ストリーミング アプリケーションはブラウザーまたはweb ビューを開くことができるため、認証`url`を読み込む必要があります。

  * ユーザーは、MVPD ログインページ内でユーザー名とパスワードを送信します。 認証が成功すると、最終的なリダイレクトに成功ページが表示されます。


* **シナリオ 2:** ストリーミング アプリケーションはブラウザーを開くことができないため、認証`code`を表示する必要があります。 ユーザーに`code`への入力を促し、認証`url`を作成して開くように求めるには、別のweb アプリケーションが必要です：[**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)。

  * ユーザーは、MVPD ログインページ内でユーザー名とパスワードを送信します。 認証が成功すると、最終的なリダイレクトに成功ページが表示されます。

### 手順4：認証済みプロファイルの確認 {#step-4-check-for-authenticated-profiles}

* **特定のコードのプロファイルを取得：** ストリーミングアプリケーションは、`code`を使用してポーリングメカニズムを実装し、[**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) エンドポイントを呼び出して、プロファイルが正常に生成および保存されたかどうかを確認する必要があります。

  * ストリーミングアプリケーションは、次の条件で&#x200B;**ポーリング** メカニズムを開始する必要があります。

    * **プライマリ（画面）アプリケーション内で実行された認証：** ブラウザーコンポーネントが[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント要求で`redirectUrl` パラメーターに指定されたURLを読み込んだ後、ユーザーが最終宛先ページに到達すると、プライマリ（ストリーミング）アプリケーションはポーリングを開始する必要があります。

    * **セカンダリ （画面） アプリケーション内で実行された認証：** プライマリ （ストリーミング） アプリケーションは、ユーザーが認証プロセスを開始するとすぐにポーリングを開始する必要があります。これは、[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答を受信し、ユーザーに認証コードを表示した直後です。

  * ストリーミングアプリケーションは、次の条件で&#x200B;**ポーリング** メカニズムを停止する必要があります。

    * **認証に成功しました：** ユーザーのプロファイル情報が正常に取得され、認証状態が確認されました。 この時点では、投票はもう必要ありません。

    * **認証セッションとコードの有効期限：**&#x200B;認証セッションとコードは、[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答の`notAfter` タイムスタンプ（30分など）で示されているように、有効期限が切れます。 この場合、ユーザーは認証プロセスを再起動する必要があり、以前の認証コードを使用したポーリングはすぐに停止する必要があります。

    * **新しい認証コードが生成されました：** ユーザーがプライマリ（画面）デバイスで新しい認証コードを要求した場合、既存のセッションは無効になり、以前の認証コードを使用したポーリングはすぐに停止する必要があります。

  * ストリーミングアプリケーションは、次の条件で&#x200B;**ポーリング** メカニズム頻度を設定する必要があります。

    * **プライマリ （画面） アプリケーション内で実行された認証：** プライマリ （ストリーミング） アプリケーションは、3 ～ 5秒以上ごとにポーリングする必要があります。

    * **セカンダリ （画面） アプリケーション内で実行された認証：** プライマリ （ストリーミング） アプリケーションは、3 ～ 5秒以上ごとにポーリングする必要があります。

  * ストリーミングアプリケーションは、ユーザーのプロファイル情報の一部を永続的なストレージにキャッシュし、不要な要求を回避してユーザーエクスペリエンスを向上させる必要があります。

## C. （オプション）事前認証フェーズ {#preauthorization-phase}

事前認証フェーズの目的は、ストリーミングアプリケーションに、ユーザーがアクセスする権限を持つリソースのサブセットをカタログから提示する機能を提供することです。

事前認証フェーズでは、ユーザーが初めてストリーミングアプリケーションを開いたり、新しいセクションに移動したりすると、ユーザーエクスペリエンスが向上します。

事前認証フェーズは必須ではありません。ユーザーの使用権限に基づいて最初にリソースをフィルタリングせずにリソースのカタログを表示する場合、ストリーミングアプリケーションはこのフェーズをスキップできます。

+++関連する記事

**API**

* [事前認証の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**FAQ**

* [事前認証フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 手順5：事前に承認されたリソースの確認 {#step-5-check-for-preauthorized-resources}

* **事前承認決定の取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) エンドポイントを呼び出すことにより、リソースのリストに対する事前承認決定を取得します。

  * ストリーミングアプリケーションは、事前認証の決定を永続ストレージに保存する必要はありません。 ただし、ユーザーエクスペリエンスを向上させるために、メモリ内で許可の決定をキャッシュすることをお勧めします。 これにより、既に承認済みのリソースに対する不要な呼び出しを回避し、遅延を低減してパフォーマンスを向上させることができます。

  * ストリーミングアプリケーションは、決定事前認証エンドポイントから応答に含まれる[ エラーコードとメッセージ ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)を調べることで、拒否された事前認証決定の理由を判断できます。 これらの詳細は、事前認証リクエストが拒否された具体的な理由をinsightに提供し、ユーザーエクスペリエンスまたはトリガーにアプリケーションで必要な処理を通知するのに役立ちます。 事前認証の決定を取得するために実装された再試行メカニズムが、事前認証の決定が拒否された場合に無限ループが発生しないようにします。 利用者に明確なフィードバックを提供することで、再試行を合理的な数に制限し、拒否を適切に処理することを検討してください。

  * ストリーミングアプリケーションは、MVPDによって課される条件により、1つのAPI リクエストで限られた数のリソースに対して事前承認決定を取得できます（通常は5件まで）。 このリソースの最大数は、Adobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じてMVPDと契約した後、組織の管理者またはAdobe Pass認証担当者が表示および変更できます。


## D.認証フェーズ {#authorization-phase}

認証フェーズの目的は、MVPDでユーザーの権利を検証した後、ユーザーがリクエストしたリソースをストリーミングアプリケーションで再生できるようにすることです。

認証フェーズは必須です。ストリーミングアプリケーションは、ユーザーがリクエストしたリソースを再生する場合、このフェーズをスキップできません。ストリーミングをリリースする前に、ユーザーが資格を持っていることをMVPDで確認する必要があるからです。

+++関連する記事

**API**

* [認証の決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的な認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**FAQ**

* [承認フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 手順6：承認済みリソースの確認 {#step-6-check-for-authorized-resources}

* **承認決定の取得：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) エンドポイントを呼び出すことにより、特定のリソースの承認決定を取得します。

  * ストリーミングアプリケーションは、永続ストレージに認証の決定を保存する必要はありません。

  * ストリーミングアプリケーションは、「決定の承認」エンドポイントからの応答に含まれる[ エラーコードとメッセージ ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)を調べることで、拒否された承認決定の理由を判断できます。 これらの詳細は、insightに認証リクエストが拒否された具体的な理由を示し、ユーザーエクスペリエンスまたはトリガーにアプリケーションで必要な処理を通知するのに役立ちます。 承認決定を取得するために実装された再試行メカニズムが、承認決定が拒否された場合にエンドレスループにならないようにします。 利用者に明確なフィードバックを提供することで、再試行を合理的な数に制限し、拒否を適切に処理することを検討してください。

  * ストリーミングアプリケーションは、ストリームがアクティブに再生されている間に、期限切れのメディアトークンを更新する必要はありません。 再生中にメディアトークンが期限切れになった場合は、ストリームを中断せずに続行できるようにする必要があります。 ただし、クライアントは、ユーザーがリソースの再生を試みたときに、新しい承認決定をリクエストし、新しいメディアトークンを取得する必要があります。

  * ストリーミングアプリケーションは、MVPDによって課される条件により、通常は1つのAPI リクエストで限られた数のリソースに対して承認決定を取得できます（1件まで）。

## E. ログアウトフェーズ {#logout-phase}

ログアウトフェーズの目的は、ストリーミングアプリケーションに、ユーザーリクエストに応じてAdobe Pass Authentication内でユーザーの認証プロファイルを終了する機能を提供することです。

ログアウトフェーズは必須です。ストリーミングアプリケーションは、ユーザーにログアウト機能を提供する必要があります。

+++関連する記事

**API**

* [特定のmvpdのログアウトを開始](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**フロー**

* [プライマリアプリケーション内で実行される基本的なログアウトフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**FAQ**

* [ログアウトフェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 手順7：ログアウト {#step-7-logout}

* **Adobe Passのログアウトを開始：** ストリーミングアプリケーションは、[**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) エンドポイントを呼び出して、ログアウトフローを開始します。

  * ストリーミングアプリケーションは、ログアウトプロセスが正しく完了するように、ログアウトエンドポイント応答の`actionName`属性と`actionType`属性に記載されている手順に従う必要があります。

    * 応答の`actionType`属性が「インタラクティブ」に設定されている場合：

      * **シナリオ 1:** ストリーミング アプリケーションはブラウザーまたはweb ビューを開くことができるため、ログアウト `url`を読み込む必要があります。

      * **シナリオ 2:** ストリーミング アプリケーションはブラウザーを開くことができないため、MVPD セッションがストリーミング デバイス ブラウザーのキャッシュに保持されていないため、ログアウト プロセスを停止できます。
