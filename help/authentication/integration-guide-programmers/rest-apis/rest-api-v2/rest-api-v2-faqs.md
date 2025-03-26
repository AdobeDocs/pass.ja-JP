---
title: REST API V2 の FAQ
description: REST API V2 の FAQ
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '9113'
ht-degree: 0%

---

# REST API V2 の FAQ {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass認証 REST API V2 の導入に関するよくある質問に対する概要の回答を示します。

REST API V2 全体について詳しくは、[REST API V2 の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) ドキュメントを参照してください。

## 一般的な FAQ {#general-faqs}

[REST API V1} または [SDK](#migration-sdk-to-rest-api-v2) から移行する新規または既存のアプリケーションにかかわらず、REST API V2 を統合する必要があるアプリケーションを使用している場合は、この節から開始し ](#migration-rest-api-v1-to-rest-api-v2) ください。

移行の詳細と手順については、次の節も参照してください。

### 登録フェーズに関するよくある質問 {#registration-phase-faqs-general}

+++登録フェーズに関するよくある質問

[Dynamic Client Registration （DCR）に関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs) ドキュメントを参照してください。

+++

### 設定フェーズの FAQ {#configuration-phase-faqs-general}

+++設定フェーズに関するよくある質問

#### 1.設定フェーズの目的は何ですか？ {#configuration-phase-faq1}

設定フェーズの目的は、MVPDごとにAdobe Pass Authentication によって保存された設定の詳細（`id`、`displayName`、`logoUrl` など）と、アクティブに統合される MVPD のリストをクライアントアプリケーションに提供することです。

設定フェーズは、クライアントアプリケーションがユーザーに TV プロバイダーの選択を依頼する必要がある場合に、認証フェーズの前提条件の手順として機能します。

#### 2.設定フェーズは必須ですか？ {#configuration-phase-faq2}

設定フェーズは必須ではありません。クライアントアプリケーションは、認証または再認証のためにMVPDを選択する必要がある場合にのみ、設定を取得する必要があります。

クライアントアプリケーションは、次のシナリオでは、このフェーズをスキップできます。

* ユーザーは既に認証済みです。
* ユーザーは、基本的またはプロモーションの [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 機能を通じて一時的なアクセスが提供されます。
* ユーザー認証の有効期限が切れているが、クライアントアプリケーションは、以前に選択したMVPDをユーザーエクスペリエンスの動機に基づく選択としてキャッシュし、まだMVPDの購読者であることを確認するように求めるだけです。

#### 3.設定とは何で、どれくらい有効ですか？ {#configuration-phase-faq3}

設定とは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration) ドキュメントで定義されている用語です。

設定には、[Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) エンドポイントから取得できる、次の属性 `id`、`displayName`、`logoUrl` などで定義された MVPD のリストが含まれています。

クライアントアプリケーションは、ユーザーが認証または再認証のためにMVPDを選択する必要がある場合にのみ、設定を取得する必要があります。

クライアントアプリケーションは、設定応答を使用して、ユーザーがテレビプロバイダーを選択する必要があるときに、使用可能なMVPD オプションを UI ピッカーに表示できます。

クライアントアプリケーションは、認証、事前認証、認証、ログアウトの各フェーズに進むために、MVPDの configuration-level `id` 属性で指定された、ユーザーの選択したMVPD ID を保存する必要があります。

詳しくは、[ 設定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) ドキュメントを参照してください。

#### 4. クライアントアプリケーションは、永続的なストレージに設定応答情報をキャッシュする必要がありますか？ {#configuration-phase-faq4}

クライアントアプリケーションは、ユーザーが認証または再認証のためにMVPDを選択する必要がある場合にのみ、設定を取得する必要があります。

次の場合に、クライアントアプリケーションは設定応答情報をメモリストレージにキャッシュして、不要なリクエストを回避し、ユーザーエクスペリエンスを向上させる必要があります。

* ユーザーは既に認証済みです。
* ユーザーは、基本的またはプロモーションの [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) 機能を通じて一時的なアクセスが提供されます。
* ユーザー認証の有効期限が切れているが、クライアントアプリケーションは、以前に選択したMVPDをユーザーエクスペリエンスの動機に基づく選択としてキャッシュし、まだMVPDの購読者であることを確認するように求めるだけです。

#### 5. クライアントアプリケーションは、独自の MVPD リストを管理できますか。 {#configuration-phase-faq5}

クライアントアプリケーションは MVPD の独自のリストを管理できますが、MVPD ID とAdobe Pass Authentication を同期させる必要があります。 そのため、Adobe Pass Authentication から提供される設定を使用して、リストを最新かつ正確にすることをお勧めします。

指定されたAdobe Pass ID が無効な場合や、指定された ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) サービスプロバイダー ](rest-api-v2-glossary.md#service-provider) とのアクティブな統合がない場合、クライアントアプリケーションはMVPD認証 REST API V2 から [ エラー [ を受け取ります。

#### 6. クライアントアプリケーションは MVPD のリストをフィルタリングできますか。 {#configuration-phase-faq6}

クライアントアプリケーションは、独自のビジネスロジックおよび要件（以前に選択したユーザーの場所やユーザーの履歴など）に基づいてカスタムメカニズムを実装することで、設定応答で提供される MVPD のリストをフィルタリングできます。

クライアントアプリケーションは、開発中またはテスト中の統合を持つ [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPD または MVPD のリストをフィルタリングできます。

#### 7. MVPDとの統合が無効になり、非アクティブとしてマークされた場合、どうなりますか？ {#configuration-phase-faq7}

MVPDとの統合が無効で非アクティブとしてマークされている場合、MVPDは、以降の設定応答で提供される MVPD のリストから削除されます。考慮すべき重要な結果が 2 つあります。

* そのMVPDの認証されていないユーザーは、そのMVPDを使用して認証フェーズを完了できなくなります。
* そのMVPDの認証済みユーザーは、そのMVPDを使用して事前認証、認証、ログアウトの各フェーズを実行できなくなります。

選択したMVPDが指定の [ サービスプロバイダー ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) とのアクティブな統合を持たなくなった場合、クライアントアプリケーションはAdobe Pass認証 REST API V2 から [ エラー ](rest-api-v2-glossary.md#service-provider) を受け取ります。

#### 8. MVPDとの統合が有効になり、アクティブとしてマークされた場合、どうなりますか？ {#configuration-phase-faq8}

MVPDとの統合が有効になり、アクティブとしてマークされると、MVPDは、以降の設定応答で提供される MVPD のリストに戻されます。考慮すべき重要な結果は 2 つあります。

* そのMVPDの認証されていないユーザーは、そのMVPDを使用して認証フェーズを再び完了できます。
* そのMVPDの認証済みユーザーは、そのMVPDを使用して、事前認証、認証、ログアウトの各フェーズを再び完了できます。

#### 9. MVPDとの統合を有効または無効にする方法 {#configuration-phase-faq9}

この操作は、組織管理者の 1 人がAdobe Pass[TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を使用して、またはお客様に代わってAdobe Pass認証担当者が実行できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration) ドキュメントを参照してください。

+++

### 認証フェーズの FAQ {#authentication-phase-faqs-general}

+++認証フェーズに関するよくある質問

#### 1.認証段階の目的は何ですか。 {#authentication-phase-faq1}

認証フェーズの目的は、クライアントアプリケーションにユーザーの ID を検証し、ユーザーメタデータ情報を取得する機能を提供することです。

認証フェーズは、クライアントアプリケーションでコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件の手順として機能します。

#### 2.認証セッションとは何で、どれくらい有効ですか？ {#authentication-phase-faq2}

認証セッションとは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session) ドキュメントで定義されている用語です。

認証セッションには、セッション エンドポイントから取得できる、開始された認証プロセスに関する情報が格納されます。

認証セッションは、発行の時点で `notAfter` タイムスタンプによって指定された、限られた短い期間にわたって有効です。これは、ユーザーが認証プロセスを完了してからフローを再開する必要が生じるまでの時間を示します。

クライアントアプリケーションは、認証セッション応答を使用して、認証プロセスの進め方を知ることができます。 一時的なアクセスの提供、アクセスの低下、ユーザーが既に認証されている場合など、ユーザーの認証が不要な場合があることに注意してください。

詳しくは、次のドキュメントを参照してください。

* [認証セッション API の作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッション API の再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリ・アプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 3.認証コードとは何ですか？また、認証コードの有効期間はどのくらいですか？ {#authentication-phase-faq3}

認証コードとは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code) ドキュメントで定義されている用語です。

認証コードは、ユーザが認証プロセスを開始する際に生成される一意の値を格納し、プロセスが完了するまで、または認証セッションが期限切れになるまで、ユーザの認証セッションを一意に識別する。

認証コードは、`notAfter` タイムスタンプによって認証セッションを開始する時点で指定された、制限された短い期間にわたって有効です。これは、ユーザーが認証プロセスを完了してからフローを再開する必要が生じるまでの時間を示します。

クライアントアプリケーションは認証コードを使用して、ユーザーが正常に認証を完了したかどうかを確認し、通常はポーリングメカニズムを介してユーザーのプロファイル情報を取得できます。

また、クライアントアプリケーションは認証コードを使用して、ユーザーが認証セッションの有効期限が切れていないことを考慮して、同じデバイスまたは 2 番目のデバイス（画面）で認証プロセスを完了または再開できるようにします。

詳しくは、次のドキュメントを参照してください。

* [認証セッション API の作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッション API の再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリ・アプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 4. ユーザーが有効な認証コードを入力し、認証セッションがまだ期限切れになっていないことをクライアントアプリケーションが認識するにはどうすればよいですか？ {#authentication-phase-faq4}

クライアントアプリケーションは、認証セッションの再開または認証コードに関連する認証セッション情報の取得を担当するセッションエンドポイントの 1 つにリクエストを送信することにより、セカンダリ（画面）アプリケーションにおいてユーザーによって入力された認証コードを検証できる。

指定された認証コードが正しく入力されなかった場合、または認証セッションが期限切れになった場合、クライアントアプリケーションは [ エラー ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) を受け取ります。

詳しくは、[ 認証セッションの再開 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) および [ 認証セッションの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) ドキュメントを参照してください。

#### 5. ユーザーが既に認証されているかどうかをクライアントアプリケーションが認識するにはどうすればよいですか？ {#authentication-phase-faq5}

クライアントアプリケーションは、ユーザーが既に認証されているかどうかを確認できる次のエンドポイントのいずれかをクエリし、プロファイル情報を返すことができます。

* [プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のMVPD API のプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（認証）コード API 用プロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 6. プロファイルとは何で、どれくらい有効ですか？ {#authentication-phase-faq6}

プロファイルとは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile) ドキュメントで定義されている用語です。

プロファイルには、ユーザーの認証の有効性、メタデータ情報など、プロファイルエンドポイントから取得できる多くの情報が保存されます。

クライアントアプリケーションはプロファイルを使用して、ユーザーの認証ステータスの把握、ユーザーメタデータ情報へのアクセス、認証に使用する方法の検索、ID の提供に使用するエンティティの検索を行うことができます。

詳しくは、次のドキュメントを参照してください。

* [プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のMVPD API のプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（認証）コード API 用プロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

プロファイルは、`notAfter` タイムスタンプでクエリされる際に指定された限られた期間にわたって有効です。これは、ユーザーが認証フェーズを再度通過する必要があるまでに有効な認証を受けた時間を示します。

この制限された期間は、認証（authN） [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) と呼ばれ、組織管理者の 1 人またはユーザーに代わってAdobe Pass認証担当者が、Adobe Pass[TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を通じて表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) ドキュメントを参照してください。

#### 7. クライアントアプリケーションは、ユーザーのプロファイル情報を永続的なストレージにキャッシュする必要がありますか？ {#authentication-phase-faq7}

クライアントアプリケーションは、次の側面を考慮して、不要なリクエストを回避しユーザーエクスペリエンスを向上させるために、ユーザーのプロファイル情報を永続的なストレージにキャッシュする必要があります。

| 属性 | ユーザーエクスペリエンス |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `attributes` | クライアントアプリケーションはこれを使用して、様々な [ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) キー（`zip`、`maxRating` など）に基づいてユーザーエクスペリエンスをパーソナライズできます。 |
| `mvpd` | クライアントアプリケーションはこれを使用して、ユーザーが選択した TV プロバイダーを追跡できます。<br/><br/> 現在のユーザープロファイルの有効期限が切れると、クライアントアプリケーションは記憶されているMVPDの選択内容を使用し、ユーザーに確認を求めることができます。 |
| `notAfter` | クライアントアプリケーションはこれを使用して、ユーザープロファイルの有効期限を追跡し、有効期限が切れた場合の再認証プロセスをトリガーし、事前認証または認証フェーズ中のエラーを避けることができます。<br/><br/> クライアントアプリケーションのエラー処理では、[authenticated_profile_expired](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) エラーコードを処理できる必要があります。このエラーコードは、クライアントアプリケーションがユーザーの再認証を必要とすることを示します。 |

#### 8. クライアントアプリケーションは、再認証を必要とせずにユーザープロファイルを拡張できますか。 {#authentication-phase-faq8}

いいえ。

ユーザープロファイルの有効期限は、MVPD で確立された認証 TTL によって決定されるので、ユーザーの操作なしにユーザープロファイルを有効期限を超えて拡張することはできません。

そのため、クライアントアプリケーションは、MVPD ログインページを再認証および操作して、システム上のプロファイルを更新するようにユーザーに求める必要があります。

ただし、[HBA （ホーム・ベースの認証 ](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) をサポートする MVPD の場合、ユーザーは認証情報を入力する必要はありません。

#### 9.使用可能な各プロファイルエンドポイントのユースケースは何ですか？ {#authentication-phase-faq9}

基本プロファイルエンドポイントは、ユーザーの認証ステータスの把握、ユーザーメタデータ情報へのアクセス、認証に使用される方法の検索、ID の提供に使用されるエンティティの検索をおこなえる機能をクライアントアプリケーションに提供するように設計されています。

各エンドポイントは、次のように特定のユースケースに適しています。

| API | 説明 | ユースケース |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [ プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | すべてのユーザープロファイルを取得します。 | **クライアントアプリケーションを初めて開く**<br/><br/> このシナリオでは、クライアントアプリケーションには、選択したMVPD識別子が永続ストレージにキャッシュされていません。<br/><br/> その結果、使用可能なすべてのユーザープロファイルを取得するリクエストが 1 回送信されます。 |
| [ 特定のMVPD API のプロファイルエンドポイント ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | 特定のMVPDに関連付けられているユーザープロファイルを取得します。 | **前回の訪問で認証した後、クライアントアプリケーションに戻る。**<br/><br/> この場合、クライアントアプリケーションには、ユーザーが以前選択したMVPD識別子が、永続的なストレージにキャッシュされている必要があります。<br/><br/> その結果、その特定のMVPDのユーザーのプロファイルを取得するリクエストが 1 回送信されます。 |
| [ 特定（認証）コード API 用プロファイルエンドポイント ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 特定の認証コードに関連付けられているユーザープロファイルを取得します。 | **ユーザーが認証プロセスを開始**<br/><br/> このシナリオでは、クライアントアプリケーションは、ユーザーが正常に認証を完了したかどうかを判断し、ユーザーのプロファイル情報を取得する必要があります。<br/><br/> その結果、ポーリングメカニズムが開始され、認証コードに関連付けられたユーザーのプロファイルを取得します。 |

詳しくは、[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) および [ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md) ドキュメントを参照してください。

プロファイル SSO エンドポイントは、様々な目的を果たします。パートナー認証応答を使用してユーザープロファイルを作成し、1 回の操作で取得する機能をクライアントアプリケーションに提供します。

| API | 説明 | ユースケース |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [ プロファイル SSO エンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | パートナー認証応答を使用してユーザープロファイルを作成および取得します。 | **ユーザーが、認証にパートナーのシングルサインオンを使用することをアプリケーションに許可する**<br/><br/> このシナリオでは、クライアントアプリケーションはパートナー認証応答を受信した後にユーザープロファイルを作成し、1 回の操作で取得する必要があります。 |

以降のクエリでは、基本プロファイルエンドポイントを使用して、ユーザーの認証ステータスの判断、ユーザーメタデータ情報へのアクセス、認証に使用する方法の検索、ID の提供に使用するエンティティの検索を行う必要があります。

詳しくは、[ パートナーフローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) および [Apple SSO クックブック（REST API V2） ](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md) ドキュメントを参照してください。

#### 10. ユーザーに複数のMVPD プロファイルがある場合、クライアントアプリケーションは何を行いますか？ {#authentication-phase-faq10}

ユーザーが複数のMVPD プロファイルを持っている場合、クライアントアプリケーションは、このシナリオを処理する最適なアプローチを決定する責任を負います。

クライアントアプリケーションは、応答から最初のユーザープロファイルを選択するか、有効期間が最も長いプロファイルを選択するかのように、目的のMVPD プロファイルを選択するようにユーザーに求めるか、自動的に選択するかを選択できます。

#### 11. ユーザープロファイルが期限切れになるとどうなりますか。 {#authentication-phase-faq11}

ユーザープロファイルの有効期限が切れると、プロファイルエンドポイントからの応答に含まれなくなります。

プロファイルエンドポイントが空のプロファイルマップ応答を返した場合、クライアントアプリケーションは新しい認証セッションを作成し、ユーザーに再認証を促す必要があります。

詳しくは、[ 認証セッション API の作成 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) ドキュメントを参照してください。

#### 12. ユーザープロファイルが無効になるのはいつですか？ {#authentication-phase-faq12}

ユーザープロファイルが無効になるのは、次の場合です。

* プロファイルエンドポイント応答の `notAfter` タイムスタンプで示されるように、認証 TTL の有効期限が切れるタイミング。
* クライアントアプリケーションが [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ヘッダー値を変更する場合。
* クライアントアプリケーションが [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) ヘッダー値の取得に使用されるクライアント資格情報を更新する場合。
* クライアント アプリケーションが、クライアント資格情報の取得に使用するソフトウェア ステートメントを失効または更新したとき。

#### 13. クライアントアプリケーションは、いつポーリングメカニズムを起動する必要がありますか？ {#authentication-phase-faq13}

効率を確保し、不要なリクエストを避けるために、クライアントアプリケーションは次の条件下でポーリングメカニズムを開始する必要があります。

**プライマリ（画面）アプリケーション内で実行される認証**

プライマリ（ストリーミング）アプリケーションでは、ユーザーが最終的な宛先ページに到達すると、ブラウザーコンポーネントが、[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントリクエストの `redirectUrl` パラメーターに指定された URL を読み込んだ後に、ポーリングを開始する必要があります。

**セカンダリ（画面）アプリケーション内で実行される認証**

プライマリ（ストリーミング）アプリケーションでは、ユーザーが認証プロセスを開始するとすぐに、つまり、[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントの応答を受信して認証コードをユーザーに表示した直後に、ポーリングを開始する必要があります。

#### 14. クライアントアプリケーションは、いつポーリングメカニズムを停止する必要がありますか。 {#authentication-phase-faq14}

効率を確保し、不要なリクエストを避けるために、クライアントアプリケーションは次の条件でポーリングメカニズムを停止する必要があります。

**認証に成功**

ユーザーのプロファイル情報は正常に取得され、ユーザーの認証ステータスが確認されます。 この時点で、ポーリングは不要になります。

**認証セッションとコードの有効期限**

[ セッション ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答の `notAfter` タイムスタンプで示されるように、認証セッションとコードの有効期限が切れます。 この場合、ユーザーは認証プロセスを再起動する必要があり、以前の認証コードを使用したポーリングは直ちに停止する必要があります。

**新しい認証コードが生成されました**

ユーザーがプライマリ（画面）デバイスで新しい認証コードを要求すると、既存のセッションは無効になり、以前の認証コードを使用したポーリングを直ちに停止する必要があります。

#### 15. クライアントアプリケーションは、呼び出しの間隔にどの程度ポーリングメカニズムを使用する必要がありますか？ {#authentication-phase-faq15}

効率を確保し、不要なリクエストを避けるために、クライアントアプリケーションは次の条件下でポーリングメカニズムの頻度を設定する必要があります。

| **プライマリ（画面）アプリケーション内で実行される認証** | **セカンダリ（画面）アプリケーション内で実行される認証** |
|----------------------------------------------------------------------|----------------------------------------------------------------------|
| プライマリ（ストリーミング）アプリケーションは、3～5 秒ごとにポーリングする必要があります。 | プライマリ（ストリーミング）アプリケーションは、3～5 秒ごとにポーリングする必要があります。 |

#### 16. クライアントアプリケーションが送信できるポーリングリクエストの最大数はいくつですか。 {#authentication-phase-faq16}

クライアントアプリケーションは、Adobe Pass認証 [ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits) で定義された現在の制限に準拠する必要があります。

クライアントアプリケーションエラー処理では、クライアントアプリケーションが許可されているリクエストの最大数を超えたことを示す [429 Too Many Requests](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) エラーコードを処理できる必要があります。

詳しくは、「スロットルメカニズム [ のドキュメントを参照し ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) ください。

#### 17. クライアントアプリケーションは、ユーザーのメタデータ情報をどのように取得できますか。 {#authentication-phase-faq17}

クライアントアプリケーションは、プロファイル情報の一部として [ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 情報を返すことができる次のエンドポイントのいずれかをクエリできます。

* [プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のMVPD API のプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（認証）コード API 用プロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

ユーザーメタデータは認証フローが完了すると使用できるようになります。したがって、[ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 情報は既にプロファイル情報に含まれているため、クライアントアプリケーションで個別のエンドポイントをクエリして取得する必要はありません。

詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

MVPDと特定のメタデータ属性に応じて、特定のメタデータ属性を認証フロー中に更新できます。 その結果、最新のユーザーメタデータを取得するには、クライアントアプリケーションが上記の API を再度クエリする必要が生じる場合があります。

#### 18. クライアント・アプリケーションは、縮退アクセスをどのように管理する必要がありますか。 {#authentication-phase-faq18}

組織が [degradation](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) 機能を使用する予定であることを考慮すると、クライアントアプリケーションは、劣化したアクセスフローを処理する必要があります。これは、このようなシナリオで REST API v2 エンドポイントがどのように動作するかについて説明しています。

詳しくは、「アクセス フローの低下 [ のドキュメントを参照し ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md) ください。

+++

### 事前認証フェーズの FAQ {#preauthorization-phase-faqs-general}

+++事前認証フェーズに関するよくある質問

#### 1.事前認証フェーズの目的は何ですか。 {#preauthorization-phase-faq1}

事前認証フェーズの目的は、ユーザーがアクセスする権利を持つカタログからリソースのサブセットをクライアントアプリケーションに提示できるようにすることです。

事前認証フェーズでは、ユーザーが初めてクライアントアプリケーションを開いた場合や、新しいセクションに移動した場合のユーザーエクスペリエンスが向上します。

#### 2.事前認証フェーズは必須ですか？ {#preauthorization-phase-faq2}

事前認証フェーズは必須ではありません。クライアントアプリケーションは、ユーザーの使用権限に基づいて最初にリソースのカタログをフィルタリングせずに提示する場合は、このフェーズをスキップできます。

#### 3.事前認証の決定とは？ {#preauthorization-phase-faq3}

事前認証は、[ 用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization) ドキュメントで定義されている用語ですが、決定用語は [ 用語集 ](rest-api-v2-glossary.md#decision) にも記載されています。

事前認証決定は、決定の事前認証エンドポイントから取得できる、MVPDの事前認証プロセスの問い合わせ結果に関する情報を格納します。

クライアントアプリケーションは、事前認証決定を使用して、ユーザーがアクセスできるリソースのサブセットを TV プロバイダー（情報）決定で提示できます。

詳しくは、次のドキュメントを参照してください。

* [事前承認決定 API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. クライアントアプリケーションは、永続的なストレージに事前認証決定をキャッシュする必要がありますか？ {#preauthorization-phase-faq4}

クライアントアプリケーションは、永続的なストレージに事前認証の決定を保存する必要はありません。 ただし、ユーザーエクスペリエンスを向上させるために、許可決定をメモリにキャッシュすることをお勧めします。 これにより、既に事前認証されているリソースに対して、決定の事前認証エンドポイントへの不要な呼び出しが回避され、待ち時間が短縮され、パフォーマンスが向上します。

#### 5. クライアントアプリケーションは、事前認証の決定が拒否された理由をどのように判断できますか？ {#preauthorization-phase-faq5}

クライアントアプリケーションは、決定の事前承認エンドポイントからの応答に含まれる [ エラーコードとメッセージ ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を調べることで、拒否された事前承認の決定の理由を判断できます。 これらの詳細は、事前認証リクエストが拒否された具体的な理由に関するインサイトを提供し、アプリケーションでの必要な処理をユーザーエクスペリエンスやトリガーに知らせるのに役立ちます。

事前認証の決定を取得するために実装された再試行メカニズムが、事前認証の決定が拒否された場合に無限ループが発生しないことを確認します。

再試行を適切な数に制限し、ユーザーに明確なフィードバックを表示して、拒否を適切に処理することを検討してください。

#### 6.事前認証決定にメディアトークンがないのはなぜですか？ {#preauthorization-phase-faq6}

事前認証フェーズの目的と同様に、事前認証フェーズをリソースの再生に使用してはならないので、事前認証決定にメディアトークンがありません。

#### 7.事前認証決定が既に存在する場合、認証フェーズはスキップできますか？ {#preauthorization-phase-faq7}

いいえ。

事前認証決定が使用可能な場合でも、認証フェーズはスキップできません。 事前認証の決定は情報提供のみで、実際の再生権限は付与されません。 事前認証フェーズは、初期のガイダンスを提供することを目的としていますが、コンテンツを再生する前には引き続き認証フェーズが必要です。

#### 8. リソースとは何ですか？また、どのような形式がサポートされていますか？ {#preauthorization-phase-faq8}

リソースとは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) ドキュメントで定義されている用語です。

リソースは、MVPD と合意された一意の識別子で、クライアントアプリケーションによってストリーミングされる可能性のあるコンテンツに関連付けられます。

リソースの一意の ID には、次の 2 つの形式があります。

* チャネル（ブランド）の一意の ID などの単純な文字列形式。
* タイトル、規制、保護者による制限のメタデータなどの追加情報を含むメディア RSS （MRSS）形式。

詳しくは、[ 保護されたリソース ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) ドキュメントを参照してください。

#### 9. クライアントアプリケーションは、一度にいくつのリソースについて事前認証の決定を取得できますか？ {#preauthorization-phase-faq9}

クライアントアプリケーションは、MVPD によって課せられた条件により、1 回の API リクエストで、通常は 5 回まで、限られた数のリソースに対して事前認証の決定を取得できます。

この最大数のリソースは、組織の管理者の 1 人がAdobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を通じて、またはお客様に代わってAdobe Pass認証担当者が MVPD に同意した後に表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) ドキュメントを参照してください。

+++

### 承認フェーズに関するよくある質問 {#authorization-phase-faqs-general}

+++承認フェーズに関するよくある質問

#### 1.承認フェーズの目的は何ですか？ {#authorization-phase-faq1}

認証フェーズの目的は、ユーザーがMVPDで権限を検証した後でリクエストしたリソースをクライアントアプリケーションで再生できるようにすることです。

#### 2.認証フェーズは必須ですか？ {#authorization-phase-faq2}

認証フェーズは必須です。クライアントアプリケーションは、ユーザーがリクエストするリソースを再生する場合、このフェーズをスキップできません。ストリームをリリースする前に、ユーザーが権限を持っていることをMVPDで確認する必要があるからです。

#### 3.認証の決定とは何ですか？また、有効期限はどのくらいですか？ {#authorization-phase-faq3}

認証とは、[ 用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization) のドキュメントで定義されている用語ですが、決定用語は [ 用語集 ](rest-api-v2-glossary.md#decision) にもあります。

認証決定は、決定の認証エンドポイントから取得できる、MVPD認証プロセスの問い合わせ結果に関する情報を格納します。

クライアントアプリケーションは、メディアトークンを含む認証決定を使用して、TV プロバイダー（権限を持つ）の決定によってユーザーがリソースストリームにアクセスできる場合に備えて、リソースストリームを再生できます。

詳しくは、次のドキュメントを参照してください。

* [承認決定 API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

認証決定は発行時に指定された限られた短い期間のみ有効で、MVPDに対する再度のクエリが必要になるまでの、Adobe Pass認証によってキャッシュされる時間を示します。

この制限付き期間は、認証（authZ） [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) と呼ばれ、組織管理者の 1 人またはAdobe Pass認証担当者が ](rest-api-v2-glossary.md#tve-dashboard) 客様に代わってAdobe Pass[TVE ダッシュボード）を通じて表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) ドキュメントを参照してください。

#### 4. クライアントアプリケーションは、認証決定を永続的なストレージにキャッシュする必要がありますか？ {#authorization-phase-faq4}

クライアントアプリケーションは、認証決定を永続ストレージに保存する必要はありません。

#### 5.認証決定が拒否された理由を、クライアントアプリケーションはどのように判断できますか。 {#authorization-phase-faq5}

クライアントアプリケーションは、決定を承認エンドポイントからの応答に含まれる [ エラーコードとメッセージ ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を調べることで、拒否された承認決定の理由を判断できます。 これらの詳細は、承認リクエストが拒否された具体的な理由に関するインサイトを提供し、アプリケーションでの必要な処理をユーザーエクスペリエンスやトリガーに伝えるのに役立ちます。

認証決定を取得するために実装された再試行メカニズムが、認証決定が拒否された場合に無限ループが発生しないことを確認します。

再試行を適切な数に制限し、ユーザーに明確なフィードバックを表示して、拒否を適切に処理することを検討してください。

#### 6. メディアトークンとは何で、どれくらい有効ですか？ {#authorization-phase-faq6}

メディアトークンは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token) ドキュメントで定義されている用語です。

メディアトークンは、決定を承認エンドポイントから取得できる、クリアテキストで送信された署名済み文字列で構成されます。

詳しくは、[ メディアトークンベリファイア ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) ドキュメントを参照してください。

メディアトークンは、問題発生時に指定された制限された短い期間のみ有効で、クライアントアプリケーションで検証および使用される前の時間制限を示します。

クライアントアプリケーションは、TV プロバイダー（権限を持つ）の決定によってユーザーがアクセスできる場合に、メディアトークンを使用してリソースストリームを再生できます。

詳しくは、次のドキュメントを参照してください。

* [承認決定 API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 7. クライアントアプリケーションは、リソースストリームを再生する前にメディストークンを検証する必要がありますか？ {#authorization-phase-faq7}

はい。

クライアントアプリケーションは、リソースストリームの再生を開始する前に、メディアトークンを検証する必要があります。 この検証は、[ メディアトークン検証子 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) を使用して実行する必要があります。 返された `token` ージから `serializedToken` を検証することで、クライアントアプリケーションは、ストリームのリッピングなどの不正アクセスを防ぎ、適切に許可されたユーザーのみがコンテンツを再生できるようにします。

#### 8. クライアントアプリケーションは、再生中に期限切れのメディアトークンを更新する必要がありますか？ {#authorization-phase-faq8}

いいえ。

ストリームのアクティブな再生中に、クライアントアプリケーションが期限切れのメディアトークンを更新する必要はありません。 再生中にメディアトークンの有効期限が切れた場合、ストリームが中断されることなく続行されるようにしてください。 ただし、次回、ユーザーが同じリソースを再生しようとすると、クライアントは新しい認証決定をリクエストし、新しいメディアトークンを取得する必要があります。

#### 9.認証決定における各タイムスタンプ属性の目的は何ですか？ {#authorization-phase-faq9}

認証決定には、認証自体と関連するメディアトークンの有効期間に関する重要なコンテキストを提供する、複数のタイムスタンプ属性が含まれます。 これらのタイムスタンプの目的は、認証決定に関連しているかメディアトークンに関連しているかによって異なります。

**決定レベルのタイムスタンプ**

これらのタイムスタンプは、承認決定全体の有効期間を示します。

| 属性 | 説明 | 備考 |
|-------------|------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 承認決定が発行された時刻。 | これにより、認証の有効期間の開始がマークされます。 |
| `notAfter` | 承認決定が期限切れになる時間です。 | [ 認証有効期間（TTL） ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) は、再認証が必要になるまでの、認証が有効である期間を決定します。 この TTL は、MVPDの担当者とネゴシエートされます。 |

**トークンレベルのタイムスタンプ**

これらのタイムスタンプは、認証決定に関連付けられたメディアトークンの有効期間を表します。

| 属性 | 説明 | 備考 |
|-------------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | メディアトークンが発行された時間。 | これは、トークンが再生に対して有効になるタイミングを示します。 |
| `notAfter` | メディアトークンの有効期限が切れる時間。 | メディアトークンは、誤用リスクを最小限に抑えるために故意に存続期間が短く（通常 7 分）、トークン生成サーバーとトークン検証サーバー間の潜在的なクロック差を考慮しています。 |

#### 10. リソースとは何ですか？また、サポートされている形式は何ですか？ {#authorization-phase-faq10}

リソースとは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource) ドキュメントで定義されている用語です。

リソースは、MVPD と合意された一意の識別子で、クライアントアプリケーションによってストリーミングされる可能性のあるコンテンツに関連付けられます。

リソースの一意の ID には、次の 2 つの形式があります。

* チャネル（ブランド）の一意の ID などの単純な文字列形式。
* タイトル、規制、保護者による制限のメタデータなどの追加情報を含むメディア RSS （MRSS）形式。

詳しくは、[ 保護されたリソース ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources) ドキュメントを参照してください。

#### 11. クライアントアプリケーションが一度に認証決定を取得できるリソースの数 {#authorization-phase-faq11}

クライアントアプリケーションは、MVPD によって課せられる条件により、1 回の API リクエストで限られた数のリソースに対する承認決定を、通常は 1 つまで取得できます。

+++

### ログアウトフェーズに関するよくある質問 {#logout-phase-faqs-general}

+++ログアウトフェーズに関するよくある質問

#### 1. ログアウトフェーズの目的は何ですか？ {#logout-phase-faq1}

ログアウトフェーズの目的は、クライアントアプリケーションが、ユーザーのリクエストに応じてAdobe Pass Authentication 内でユーザーの認証済みプロファイルを終了できるようにすることです。

#### 2. ログアウトフェーズは必須ですか？ {#logout-phase-faq2}

ログアウトフェーズは必須です。クライアントアプリケーションは、ログアウトする機能をユーザーに提供する必要があります。

+++

### ヘッダーの FAQ {#headers-faqs-general}

+++ヘッダーの FAQ

#### 1.認証ヘッダーの値を計算する方法 {#headers-faq1}

>[!IMPORTANT]
>
> クライアントアプリケーションが REST API V1 から REST API V2 に移行する場合、クライアントアプリケーションは引き続き同じメソッドを使用して、以前と同じアクセストークン値を取得 `Bearer` きます。

[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) リクエストヘッダーには、Adobe Passで保護された API にアクセスするためにクライアントアプリケーションで必要な `Bearer` アクセストークンが含まれています。

Authorization ヘッダー値は、登録段階でAdobe Pass Authentication から取得する必要があります。

詳しくは、次のドキュメントを参照してください。

* [Dynamic Client Registration の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [クライアント資格情報 API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークン API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### 2. AP-Device-Identifier ヘッダーの値を計算する方法 {#headers-faq2}

>[!IMPORTANT]
>
> クライアントアプリケーションが REST API V1 から REST API V2 に移行する場合、クライアントアプリケーションは引き続き同じ方法を使用して、以前と同じようにデバイス識別子の値を計算できます。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 要求ヘッダーには、クライアント アプリケーションによって作成されたストリーミング デバイスの識別子が含まれています。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ヘッダーのドキュメントには、値の計算方法に関する主なプラットフォームの例が記載されていますが、クライアントアプリケーションは、独自のビジネスロジックと要件に基づいて別の方法を使用するように選択できます。

#### 3. X-Device-Info ヘッダーの値を計算する方法 {#headers-faq3}

>[!IMPORTANT]
>
> クライアントアプリケーションが REST API V1 から REST API V2 に移行する場合、クライアントアプリケーションは引き続き同じ方法を使用して、以前と同じようにデバイス情報の値を計算できます。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) リクエストヘッダーには、実際のストリーミングデバイスに関連するクライアント情報（デバイス、接続、アプリケーション）が含まれます。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ヘッダーのドキュメントには、値の計算方法に関する主なプラットフォームの例が記載されていますが、クライアントアプリケーションは、独自のビジネスロジックと要件に基づいて別の方法を使用するように選択できます。

+++

### その他の FAQ {#misc-faqs-general}

+++その他の FAQ

#### 1. REST API V2 のリクエストと応答を調べて、API をテストできますか。 {#misc-faq1}

はい。

専用の [Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトから REST API V2 を参照できます。 Adobe Developerの web サイトでは、次の場所への無制限のアクセスが可能です。

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) とやり取りするには、[DCR API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) を介して取得した `Bearer` アクセストークンに [Authorization](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) ヘッダーを含める必要があります。

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) を使用するには、REST API V2 スコープを含むソフトウェアステートメントが必要です。 詳しくは、[Dynamic Client Registration （DCR）の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md) ドキュメントを参照してください。

#### 2. OpenAPI をサポートする API 開発ツールを使用して、REST API V2 のリクエストと応答を調べることはできますか。 {#misc-faq2}

はい。

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) および [REST API V2} の OpenAPI 仕様ファイルは、{4](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトからダウンロードできます。[

OpenAPI 仕様ファイルをダウンロードするには、「ダウンロード」ボタンをクリックして、次のファイルをローカルマシンに保存します。

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

その後、これらのファイルを好みの API 開発ツールに読み込み、REST API V2 のリクエストと応答を調べて、API をテストできます。

#### 3. https://sp.auth-staging.adobe.com/apitest/api.htmlでホストされている既存の API テストツールを引き続き使用できますか？ {#misc-faq3}

いいえ。

REST API V2 に移行するクライアントアプリケーションでは、https://developer.adobe.com/adobe-pass/でホストされる新しいテストツールを使用する必要があります。 Adobe Developerの web サイトでは、次の場所への無制限のアクセスが可能です。

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) とやり取りするには、[DCR API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) を介して取得した `Bearer` アクセストークンに [Authorization](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) ヘッダーを含める必要があります。

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) を使用するには、REST API V2 スコープを含むソフトウェアステートメントが必要です。 詳しくは、[Dynamic Client Registration （DCR）の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md) ドキュメントを参照してください。

+++

## 移行に関する FAQ {#migration-faqs}

既存のアプリケーションを REST API V2 に移行する必要があるアプリケーションで作業している場合は、この節を続行してください。

>[!MORELIKETHIS]
>
> * [Dynamic Client Registration （DCR）に関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### 一般的な移行に関する FAQ {#general-migration-faqs}

+++一般的な移行の FAQ

#### 1. REST API V2 に移行された新しいクライアントアプリケーションを、すべてのユーザーに一度にロールアウトする必要がありますか？ {#migration-faq1}

いいえ。

クライアントアプリケーションは、REST API V2 を統合した新しいバージョンをすべてのユーザーに同時にロールアウトする必要はありません。

Adobe Pass認証では、2025 年末まで、REST API V1 またはSDKを統合する古いクライアントアプリケーションバージョンを引き続きサポートします。

#### 2. REST API V2 に移行された新しいクライアントアプリケーションを、すべての API とフローにわたって一度にロールアウトする必要がありますか？ {#migration-faq2}

はい。

クライアントアプリケーションは、REST API V2 を統合した新しいバージョンを、すべてのAdobe Pass認証 API およびフローに同時にロールアウトする必要があります。

「2 画面目の認証」フローの場合、クライアントアプリケーションは、「プライマリ」と「セカンダリ [ の両方のアプリケーションに対して REST API V2 を統合した新しいバージョンを同時にロ [ ルアウトす ](rest-api-v2-glossary.md#primary-application) 必要が ](rest-api-v2-glossary.md#secondary-application) ります。

Adobe Pass認証では、API とフローの間で REST API V2 と REST API V1/SDKの両方を統合する「ハイブリッド」実装をサポートしていません。

#### 3. REST API V2 に移行された新しいクライアントアプリケーションに更新する際、ユーザー認証は保持されますか。 {#migration-faq3}

いいえ。

REST API V1 またはSDKを統合する古いクライアントアプリケーションバージョンで取得したユーザー認証は保持されません。

したがって、ユーザーは、REST API V2 に移行された新しいクライアントアプリケーション内で再認証する必要があります。

#### 4. REST API V2 では、拡張エラーコードはデフォルトで有効になっていますか。 {#migration-faq4}

はい。

REST API V2 に移行するクライアントアプリケーションでは、デフォルトでこの機能のメリットが自動的に得られ、より詳細で正確なエラー情報が提供されます。

詳しくは、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) ドキュメントを参照してください。

#### 5.既存の統合を REST API V2 に移行する際に、設定を変更する必要はありますか。 {#migration-faq5}

いいえ。

REST API V2 に移行するクライアントアプリケーションでは、既存のMVPD統合の設定を変更する必要はありません。 また、既存の [ サービスプロバイダー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) および [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) に対しても、引き続き同じ識別子を使用します。

また、MVPD エンドポイントと通信するためにAdobe Pass認証で使用されるプロトコルは変更されません。

+++

### REST API V1 から REST API V2 への移行 {#migration-rest-api-v1-to-rest-api-v2-faqs}

REST API V1 から REST API V2 に移行する必要があるアプリケーションを操作している場合は、このサブセクションを続行します。

#### 登録フェーズに関するよくある質問 {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++登録フェーズに関するよくある質問

##### 1.登録フェーズに必要な高レベルの API 移行は何ですか。 {#registration-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、登録フェーズに関して大きな変更はありません。

クライアントアプリケーションは、引き続き同じフローを使用して、[ 動的クライアント登録（DCR） ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) プロセスを通じてAdobe Pass認証に登録し、アクセストークンを取得できます。

詳しくは、次のドキュメントを参照してください。

* [Dynamic Client Registration の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [クライアント資格情報 API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークン API の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### 設定フェーズの FAQ {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++設定フェーズに関するよくある質問

##### 1.設定フェーズに必要な、大まかな API 移行は何ですか？ {#configuration-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 認証フェーズの FAQ {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++認証フェーズに関するよくある質問

##### 1.認証フェーズに必要な、高レベルの API 移行は何ですか。 {#authentication-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コードを確認（認証コード） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2 番目の画面（アプリケーション）で認証を再開（MVPD） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証の開始 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [GET <br/> /api/v1/checkauthn （最初の画面） ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn （2 番目の画面） ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザー認証トークンの取得（プロファイル） | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 事前認証フェーズの FAQ {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++事前認証フェーズに関するよくある質問

##### 1.事前認証フェーズに必要な高レベルの API 移行は何ですか。 {#preauthorization-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [GET <br/> /api/v1/preauthorize （最初の画面） ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize （2 番目の画面） ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 承認フェーズに関するよくある質問 {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++承認フェーズに関するよくある質問

##### 1.認証フェーズに必要な高レベルの API 移行は何ですか。 {#authorization-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 認証トークンの取得（認証決定） | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 短い認証トークンの取得（メディアトークン） | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### ログアウトフェーズに関するよくある質問 {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++ログアウトフェーズに関するよくある質問

##### 1. ログアウトフェーズに必要な、大まかな API 移行は何ですか。 {#logout-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトの開始 | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### SDKから REST API V2 への移行 {#migration-sdk-to-rest-api-v2}

SDKから REST API V2 に移行する必要があるアプリケーションを操作している場合は、このサブセクションの手順を続けます。

#### 登録フェーズに関するよくある質問 {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++登録フェーズに関するよくある質問

##### 1.登録フェーズに必要な高レベルの API 移行は何ですか。 {#registration-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

###### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### 設定フェーズの FAQ {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++設定フェーズに関するよくある質問

##### 1.設定フェーズに必要な、大まかな API 移行は何ですか？ {#configuration-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

###### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 認証フェーズの FAQ {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++認証フェーズに関するよくある質問

##### 1.認証フェーズに必要な、高レベルの API 移行は何ですか。 {#authentication-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

###### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOSSDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コードを確認（認証コード） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2 番目の画面（アプリケーション）で認証を再開（MVPD） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コードを確認（認証コード） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2 番目の画面（アプリケーション）で認証を再開（MVPD） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 事前認証フェーズの FAQ {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++事前認証フェーズに関するよくある質問

##### 1.事前認証フェーズに必要な高レベルの API 移行は何ですか。 {#preauthorization-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

###### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 承認フェーズに関するよくある質問 {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++承認フェーズに関するよくある質問

##### 1.認証フェーズに必要な高レベルの API 移行は何ですか。 {#authorization-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

###### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### ログアウトフェーズに関するよくある質問 {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++ログアウトフェーズに関するよくある質問

##### 1. ログアウトフェーズに必要な、大まかな API 移行は何ですか。 {#logout-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

###### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
