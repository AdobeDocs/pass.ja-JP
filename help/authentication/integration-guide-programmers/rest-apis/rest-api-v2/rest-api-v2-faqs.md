---
title: REST API V2 FAQ
description: REST API V2 FAQ
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '11094'
ht-degree: 1%

---

# REST API V2 FAQ {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このドキュメントでは、Adobe Pass Authentication REST API V2の導入に関するよくある質問に対する高い概要回答を提供します。

REST API V2全体の詳細については、[REST API V2の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)のドキュメントを参照してください。

## 一般的なFAQ {#general-faqs}

新しいアプリケーションであるか、[REST API V1](#migration-rest-api-v1-to-rest-api-v2)または[SDK](#migration-sdk-to-rest-api-v2)から移行する既存のアプリケーションであるかを問わず、REST API V2を統合する必要があるアプリケーションを使用している場合は、この節から始めてください。

移行の詳細と手順について詳しくは、次の節も参照してください。

### 登録フェーズに関するFAQ {#registration-phase-faqs-general}

+++登録フェーズに関するFAQ

[Dynamic Client Registration （DCR）に関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs)のドキュメントを参照してください。

+++

### 設定段階に関するFAQ {#configuration-phase-faqs-general}

+++設定段階に関するFAQ

#### &#x200B;1. 設定フェーズの目的は何か？ {#configuration-phase-faq1}

設定フェーズの目的は、設定の詳細（例：`id`、`displayName`、`logoUrl`など）と共にアクティブに統合されるMVPDのリストをクライアントアプリケーションに提供することです。 各MVPDに対してAdobe Pass認証によって保存されます。

設定フェーズは、クライアントアプリケーションがユーザーにTV プロバイダーの選択を依頼する必要がある場合に、認証フェーズの前提条件ステップとして機能します。

#### &#x200B;2. 設定フェーズは必須ですか？ {#configuration-phase-faq2}

設定フェーズは必須ではありません。クライアントアプリケーションは、利用者が認証または再認証のためにMVPDを選択する必要がある場合にのみ、設定を取得する必要があります。

クライアントアプリケーションは、次のシナリオでこのフェーズをスキップできます。

* ユーザーは既に認証されています。
* ユーザーには、基本またはプロモーションの[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)機能を通じて一時的なアクセスが提供されます。
* ユーザー認証の有効期限が切れましたが、クライアントアプリケーションは、ユーザーエクスペリエンスに動機づけられた選択肢として以前に選択したMVPDをキャッシュし、そのMVPDのサブスクライバーであることを確認するようにユーザーに促すだけです。

#### &#x200B;3. 設定とは何か？また、設定の有効な期間はどれくらいか？ {#configuration-phase-faq3}

設定は、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration)のドキュメントで定義されている用語です。

設定には、次の属性`id`、`displayName`、`logoUrl`などで定義されたMVPDのリストが含まれています。このリストは、[設定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) エンドポイントから取得できます。

クライアントアプリケーションは、ユーザーが認証または再認証のためにMVPDを選択する必要がある場合にのみ、設定を取得する必要があります。

クライアントアプリケーションは、設定レスポンスを使用して、ユーザーがTV プロバイダーを選択する必要があるたびに、使用可能なMVPD オプションをUI ピッカーに表示できます。

クライアントアプリケーションは、認証、事前認証、認証、またはログアウトの各フェーズを進めるために、MVPDの設定レベル `id`属性で指定されている、ユーザーが選択したMVPD IDを保存する必要があります。

詳しくは、[設定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) ドキュメントを参照してください。

#### &#x200B;4. 設定は、サービスプロバイダー、プラットフォーム、またはユーザーに固有ですか？ {#configuration-phase-faq4}

設定は[&#x200B; サービスプロバイダー](rest-api-v2-glossary.md#service-provider)に固有です。

設定はプラットフォームタイプに固有です。

設定はユーザーに固有ではありません。

サーバー間アーキテクチャを使用するクライアントアプリケーションの場合は、サーバーサイドメモリストレージ内の各プラットフォームタイプに対する設定応答（2分TTLなど）をキャッシュすることをお勧めします。 これにより、各ユーザーへの不要なリクエストを減らし、ユーザーエクスペリエンス全体を向上させることができます。

#### &#x200B;5. クライアントアプリケーションは、設定応答情報を永続的なストレージにキャッシュする必要がありますか？ {#configuration-phase-faq5}

>[!IMPORTANT]
> 
> サーバー間アーキテクチャを使用するクライアントアプリケーションの場合は、サーバーサイドメモリストレージ内の各プラットフォームタイプに対する設定応答（2分TTLなど）をキャッシュすることをお勧めします。 これにより、各ユーザーへの不要なリクエストを減らし、ユーザーエクスペリエンス全体を向上させることができます。

クライアントアプリケーションは、ユーザーが認証または再認証のためにMVPDを選択する必要がある場合にのみ、設定を取得する必要があります。

クライアントアプリケーションは、不要なリクエストを避け、次の場合にユーザーエクスペリエンスを向上させるために、構成応答情報をメモリストレージにキャッシュする必要があります。

* ユーザーは既に認証されています。
* ユーザーには、基本またはプロモーションの[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)機能を通じて一時的なアクセスが提供されます。
* ユーザー認証の有効期限が切れましたが、クライアントアプリケーションは、ユーザーエクスペリエンスに動機づけられた選択肢として以前に選択したMVPDをキャッシュし、そのMVPDのサブスクライバーであることを確認するようにユーザーに促すだけです。

#### &#x200B;6. クライアントアプリケーションは、MVPDの独自のリストを管理できますか？ {#configuration-phase-faq6}

クライアントアプリケーションは、MVPDの独自のリストを管理できますが、Adobe Pass認証とMVPD IDを同期させる必要があります。 そのため、Adobe Pass Authenticationが提供する設定を使用して、リストが最新かつ正確であることを確認することをお勧めします。

指定されたMVPD IDが無効な場合、または指定された[&#x200B; サービスプロバイダー](rest-api-v2-glossary.md#service-provider)とアクティブな統合がない場合、クライアントアプリケーションはAdobe Pass Authentication REST API V2から[&#x200B; エラー](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)を受け取ります。

#### &#x200B;7. クライアントアプリケーションはMVPDのリストをフィルタリングできますか？ {#configuration-phase-faq7}

クライアントアプリケーションは、独自のビジネスロジックと、ユーザーの場所や以前の選択のユーザー履歴などの要件に基づいてカスタムメカニズムを実装することで、構成応答で提供されるMVPDのリストをフィルタリングできます。

クライアントアプリケーションは、[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)のMVPDのリスト、または統合がまだ開発中またはテスト中のMVPDのリストをフィルタリングできます。

#### &#x200B;8. MVPDとの統合が無効になっていて、非アクティブとマークされている場合はどうなりますか？ {#configuration-phase-faq8}

MVPDとの統合が無効になり、非アクティブとマークされた場合、MVPDは追加の設定応答で提供されるMVPDのリストから削除され、考慮すべき2つの重要な結果があります。

* そのMVPDの未認証のユーザーは、そのMVPDを使用して認証フェーズを完了できなくなります。
* そのMVPDの認証済みユーザーは、そのMVPDを使用して事前認証、認証、またはログアウトフェーズを完了できなくなります。

ユーザーが選択したAdobe Passが、指定した[&#x200B; サービスプロバイダー](rest-api-v2-glossary.md#service-provider)とのアクティブな統合を持たない場合、クライアントアプリケーションはMVPD Authentication REST API V2から[&#x200B; エラー](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)を受け取ります。

#### &#x200B;9. MVPDとの統合が有効になり、アクティブとしてマークされた場合はどうなりますか？ {#configuration-phase-faq9}

MVPDとの統合が有効になり、アクティブとしてマークされた場合、MVPDは追加の設定応答で提供されるMVPDのリストに戻され、考慮すべき重要な結果が2つあります。

* そのMVPDの未認証ユーザーは、そのMVPDを使用して認証フェーズを再度完了できます。
* そのMVPDの認証済みユーザーは、そのMVPDを使用して、再認証、認証、またはログアウトフェーズを完了できます。

#### &#x200B;10. MVPDとの統合を有効または無効にする方法を教えてください。 {#configuration-phase-faq10}

この操作は、Adobe Pass [TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて、組織管理者の1人またはAdobe Pass認証担当者が代わりに実行します。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration)のドキュメントを参照してください。

+++

### 認証フェーズに関するFAQ {#authentication-phase-faqs-general}

+++認証フェーズに関するFAQ

#### &#x200B;1. 認証フェーズの目的は何ですか？ {#authentication-phase-faq1}

認証フェーズの目的は、クライアントアプリケーションにユーザーのIDを検証し、ユーザーメタデータ情報を取得する機能を提供することです。

認証フェーズは、クライアントアプリケーションでコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件ステップとして機能します。

#### &#x200B;2. 認証フェーズは必須ですか？ {#authentication-phase-faq2}

認証フェーズは必須です。クライアントアプリケーションは、Adobe Pass認証内に有効なプロファイルがない場合にユーザーを認証する必要があります。

クライアントアプリケーションは、次のシナリオでこのフェーズをスキップできます。

* ユーザーは既に認証されており、プロファイルは引き続き有効です。
* ユーザーには、基本またはプロモーションの[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)機能を通じて一時的なアクセスが提供されます。

クライアントアプリケーションのエラー処理では、[&#x200B; エラー](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) コード （例：`authenticated_profile_missing`、`authenticated_profile_expired`、`authenticated_profile_invalidated`など）を処理する必要があります。これは、クライアントアプリケーションがユーザーに認証を求めていることを示しています。

#### &#x200B;3. 認証セッションとは何ですか？また、認証セッションの有効期限はどれくらいですか？ {#authentication-phase-faq3}

認証セッションは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session)のドキュメントで定義されている用語です。

認証セッションには、開始された認証プロセスに関する情報が保存されます。この情報は、セッションエンドポイントから取得できます。

認証セッションは、問題の時点で`notAfter` タイムスタンプで指定された制限付き短い期間に有効です。これは、ユーザーがフローを再起動する前に認証プロセスを完了する必要がある時間を示します。

クライアントアプリケーションは、認証セッション応答を使用して、認証プロセスの進め方を把握できます。 一時的なアクセスの提供、アクセスの低下、ユーザーが既に認証されている場合など、ユーザーが認証する必要がない場合があることに注意してください。

詳しくは、次のドキュメントを参照してください。

* [認証セッション APIの作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッション APIの再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. 認証コードとは何ですか？また、有効な期間はどれくらいですか？ {#authentication-phase-faq4}

認証コードは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code)のドキュメントで定義されている用語です。

認証コードは、ユーザが認証プロセスを開始したときに生成された一意の値を格納し、プロセスが完了するか認証セッションが期限切れになるまで、ユーザの認証セッションを一意に識別する。

認証コードは、認証セッションを開始する時点で`notAfter` タイムスタンプで指定された制限付き短い期間に有効です。この期間は、ユーザーがフローを再起動する前に認証プロセスを完了する必要がある時間を示します。

クライアントアプリケーションは、認証コードを使用して、ユーザーが認証を正常に完了したかどうかを確認し、通常はポーリングメカニズムを介してユーザーのプロファイル情報を取得できます。

クライアントアプリケーションは、認証コードを使用して、認証セッションが期限切れになっていないことを考慮して、同じデバイスまたは2番目の（画面）デバイスで認証プロセスを完了または再開できるようにすることもできます。

詳しくは、次のドキュメントを参照してください。

* [認証セッション APIの作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッション APIの再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. クライアントアプリケーションは、ユーザーが有効な認証コードを入力したかどうか、および認証セッションがまだ期限切れでなかったかどうかを知るにはどうすればよいですか？ {#authentication-phase-faq5}

クライアントアプリケーションは、認証セッションの再開または認証コードに関連付けられた認証セッション情報の取得を担当するセッションエンドポイントのいずれかにリクエストを送信することにより、セカンダリ（画面）アプリケーションでユーザーが入力した認証コードを検証できます。

指定された認証コードが間違って入力された場合、または認証セッションの有効期限が切れた場合、クライアントアプリケーションは[&#x200B; エラー](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)を受け取ります。

詳細については、[認証セッションの再開](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)および[認証セッションの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)文書を参照してください。

#### &#x200B;6. クライアントアプリケーションは、ユーザーが既に認証されているかどうかをどのように確認できますか？ {#authentication-phase-faq6}

クライアントアプリケーションは、ユーザーが既に認証されているかどうかを確認し、プロファイル情報を返すことができる次のいずれかのエンドポイントをクエリできます。

* [プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のMVPD APIのプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定の（認証）コード APIのプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本的なプロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. プロファイルとは何か？どのくらいの期間有効か？ {#authentication-phase-faq7}

プロファイルは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile)のドキュメントで定義されている用語です。

このプロファイルには、ユーザーの認証の有効性に関する情報、メタデータ情報など、プロファイルエンドポイントから取得できる情報が格納されます。

クライアントアプリケーションは、プロファイルを使用して、ユーザーの認証ステータスの把握、ユーザーのメタデータ情報へのアクセス、認証に使用される方法の検索、またはIDの提供に使用されるエンティティの検索を行うことができます。

詳しくは、次のドキュメントを参照してください。

* [プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のMVPD APIのプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定の（認証）コード APIのプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本的なプロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

プロファイルは、`notAfter` タイムスタンプによってクエリされたときに指定された期間限定で有効です。このプロファイルは、ユーザーが認証フェーズを再度通過する前に有効な認証を受ける時間を示します。

認証（authN） [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)と呼ばれるこの制限付き期間は、Adobe Pass [TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて、組織管理者の1人またはAdobe Pass認証担当者が代理で表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)のドキュメントを参照してください。

#### &#x200B;8. クライアントアプリケーションは、ユーザーのプロファイル情報を永続的なストレージにキャッシュする必要がありますか？ {#authentication-phase-faq8}

クライアントアプリケーションは、ユーザーのプロファイル情報の一部を永続的なストレージにキャッシュし、不要な要求を回避して、次の点を考慮してユーザーエクスペリエンスを向上させる必要があります。

| 属性 | ユーザーエクスペリエンス |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | この機能を使用して、選択したテレビ プロバイダーを管理し、事前認証または認証フェーズ中に引き続き使用できます。<br/><br/>現在のユーザープロファイルの有効期限が切れると、クライアント アプリケーションは記憶されているMVPDの選択範囲を使用して、ユーザーに確認を依頼できます。 |
| `attributes` | クライアントアプリケーションは、これを使用して、様々な[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) キー（例：`zip`、`maxRating`など）に基づいてユーザーエクスペリエンスをパーソナライズできます。<br/><br/>認証フローが完了すると、ユーザーメタデータが利用可能になります。そのため、クライアントアプリケーションは、[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)情報を取得するために別のエンドポイントをクエリする必要はありません。この情報はプロファイル情報に既に含まれています。<br/><br/>MVPDと特定のメタデータ属性に応じて、承認フロー中に特定のメタデータ属性が更新される場合があります。 その結果、クライアントアプリケーションは、最新のユーザーメタデータを取得するためにProfiles APIを再度クエリする必要がある場合があります。 |
| `notAfter` | クライアントアプリケーションはこれを使用して、ユーザープロファイルの有効期限を追跡できます。<br/><br/> クライアントアプリケーションのエラー処理では、[&#x200B; エラー](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) コード （例：`authenticated_profile_missing`、`authenticated_profile_expired`、`authenticated_profile_invalidated`など）を処理する必要があります。これは、クライアントアプリケーションがユーザーに認証を求めていることを示します。 |

#### &#x200B;9. クライアントアプリケーションは、再認証を必要とせずにユーザーのプロファイルを拡張できますか？ {#authentication-phase-faq9}

いいえ。

ユーザープロファイルは、MVPDで確立された認証TTLによって有効期限が決まるため、ユーザーによる操作がなければ有効期限切れにすることはできません。

したがって、クライアントアプリケーションは、ユーザーがMVPD ログインページを再認証して操作し、システム上のプロファイルを更新するようにユーザーに促す必要があります。

ただし、[&#x200B; ホームベース認証](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) （HBA）をサポートするMVPDの場合、ユーザーは資格情報を入力する必要はありません。

#### &#x200B;10. 利用可能な各プロファイルエンドポイントのユースケースは何ですか？ {#authentication-phase-faq10}

基本的なプロファイルエンドポイントは、クライアントアプリケーションに、ユーザーの認証ステータスを知り、ユーザーのメタデータ情報にアクセスし、認証に使用される方法またはIDの提供に使用されるエンティティを見つける機能を提供するように設計されています。

各エンドポイントは、次のような特定のユースケースに適しています。

| API | 説明 | ユースケース |
|--- |--- |--- |
| [&#x200B; プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | すべてのユーザープロファイルを取得します。 | **ユーザーが初めてクライアントアプリケーションを開きます**<br/><br/>&#x200B;このシナリオでは、クライアントアプリケーションには、ユーザーが選択したMVPD IDが永続ストレージにキャッシュされていません。<br/><br/>その結果、使用可能なすべてのユーザープロファイルを取得するための1回のリクエストが送信されます。 |
| 特定のMVPD APIの[&#x200B; プロファイルエンドポイント &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | 特定のMVPDに関連付けられているユーザープロファイルを取得します。 | **前回の訪問で認証した後、クライアントアプリケーションに戻ります**<br/><br/>&#x200B;この場合、クライアントアプリケーションには、ユーザーが以前に選択したMVPD IDが永続ストレージにキャッシュされている必要があります。<br/><br/>その結果、その特定のMVPDのユーザーのプロファイルを取得するための1回のリクエストが送信されます。 |
| 特定の（認証）コード API[&#128279;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)の プロファイル エンドポイント | 特定の認証コードに関連付けられているユーザープロファイルを取得します。 | **ユーザーが認証プロセスを開始します**<br/><br/>&#x200B;このシナリオでは、クライアントアプリケーションは、ユーザーが認証を正常に完了したかどうかを判断し、プロファイル情報を取得する必要があります。<br/><br/>その結果、ポーリング メカニズムを開始して、認証コードに関連付けられたユーザーのプロファイルを取得します。 |

詳細については、プライマリアプリケーション内で実行される[基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)およびセカンダリアプリケーション内で実行される[基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)文書を参照してください。

プロファイル SSO エンドポイントは異なる目的を果たします。クライアントアプリケーションには、パートナー認証応答を使用してユーザープロファイルを作成し、1回の操作で取得する機能が提供されます。

| API | 説明 | ユースケース |
| --- | --- | --- |
| [&#x200B; プロファイル SSO エンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | パートナー認証応答を使用して、ユーザープロファイルを作成および取得します。 | **ユーザーは、アプリケーションがパートナーシングルサインオンを使用して認証することを許可しています**<br/><br/>&#x200B;このシナリオでは、クライアントアプリケーションはパートナー認証応答を受信した後にユーザープロファイルを作成し、1回限りの操作で取得する必要があります。 |

その後のクエリでは、ユーザーの認証ステータスの決定、ユーザーのメタデータ情報へのアクセス、認証に使用される方法またはIDの提供に使用されるエンティティの検索に、基本的なプロファイルエンドポイントを使用する必要があります。

詳しくは、[&#x200B; パートナーフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)および[Apple SSO クックブック （REST API V2） &#x200B;](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)のドキュメントを参照してください。

#### &#x200B;11. ユーザーが複数のMVPD プロファイルを持っている場合、クライアントアプリケーションはどうすればよいですか？ {#authentication-phase-faq11}

複数のプロファイルをサポートする決定は、クライアントアプリケーションのビジネス要件によって異なります。

ほとんどのユーザーは1つのプロファイルのみを持ちます。 ただし、複数のプロファイルが存在する場合（以下で詳しく説明します）、クライアントアプリケーションは、プロファイル選択に最適なユーザーエクスペリエンスを決定する責任があります。

クライアントアプリケーションは、ユーザーに必要なMVPD プロファイルの選択を促すか、応答から最初のユーザープロファイルを選択するか、有効期間が最も長いユーザープロファイルを選択するなど、自動的に選択するかを選択できます。

REST API v2では、次の機能に対応する複数のプロファイルをサポートしています。

* 通常のMVPD プロファイルと、シングルサインオン（SSO）で取得したプロファイルのどちらかを選択する必要がある場合があります。
* 通常のMVPDからログアウトすることなく、一時的なアクセスが提供されます。
* MVPD サブスクリプションとDirect-to-Consumer （DTC）サービスを組み合わせたユーザー。
* 複数のMVPD サブスクリプションを持つユーザー。

#### &#x200B;12. ユーザープロファイルの有効期限が切れるとどうなりますか？ {#authentication-phase-faq12}

ユーザープロファイルの有効期限が切れると、プロファイルエンドポイントからの応答にユーザープロファイルが含まれなくなります。

プロファイルエンドポイントが空のプロファイルマップ応答を返す場合、クライアントアプリケーションは新しい認証セッションを作成し、ユーザーに再認証を促す必要があります。

詳しくは、[認証セッション APIの作成](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)のドキュメントを参照してください。

#### &#x200B;13. ユーザープロファイルはいつ無効になりますか？ {#authentication-phase-faq13}

ユーザープロファイルは、次のシナリオで無効になります。

* 認証TTLが期限切れになると、プロファイルエンドポイント応答の`notAfter` タイムスタンプに示されます。
* クライアントアプリケーションが[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ヘッダー値を変更すると。
* クライアントアプリケーションが更新されると、[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) ヘッダー値の取得に使用されるクライアント資格情報が更新されます。
* クライアントアプリケーションが、クライアント資格情報の取得に使用したソフトウェアステートメントを取り消すか更新する場合。

#### &#x200B;14. クライアントアプリケーションはいつポーリングメカニズムを開始する必要がありますか？ {#authentication-phase-faq14}

効率を確保し、不要な要求を避けるために、クライアントアプリケーションは次の条件でポーリングメカニズムを開始する必要があります。

**プライマリ（画面）アプリケーション内で実行された認証**

プライマリ （ストリーミング） アプリケーションは、ユーザーが最終宛先ページに到達したときに、ブラウザーコンポーネントが[&#x200B; セッション &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイントリクエストで`redirectUrl` パラメーターに指定されたURLを読み込んだ後にポーリングを開始する必要があります。

**セカンダリ （画面） アプリケーション内で実行された認証**

プライマリ（ストリーミング）アプリケーションは、ユーザーが認証プロセスを開始するとすぐにポーリングを開始する必要があります。これは、[Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答を受信し、ユーザーに認証コードを表示した直後です。

#### &#x200B;15. クライアントアプリケーションはいつポーリングメカニズムを停止する必要がありますか？ {#authentication-phase-faq15}

効率を確保し、不要な要求を避けるために、クライアントアプリケーションは次の条件でポーリングメカニズムを停止する必要があります。

**認証に成功しました**

ユーザーのプロファイル情報が正常に取得され、認証ステータスが確認されます。 この時点では、投票はもう必要ありません。

**認証セッションとコードの有効期限**

認証セッションとコードは、[&#x200B; セッション &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) エンドポイント応答の`notAfter` タイムスタンプ （30分など）で示されているように、有効期限が切れます。 この場合、ユーザーは認証プロセスを再起動する必要があり、以前の認証コードを使用したポーリングはすぐに停止する必要があります。

**新しい認証コードが生成されました**

ユーザーがプライマリ（画面）デバイスで新しい認証コードを要求した場合、既存のセッションは無効になり、以前の認証コードを使用したポーリングはすぐに停止する必要があります。

#### &#x200B;16. クライアントアプリケーションがポーリングメカニズムに使用する呼び出しの間隔はどれくらいですか？ {#authentication-phase-faq16}

効率を確保し、不要な要求を避けるために、クライアントアプリケーションは次の条件でポーリングメカニズムの頻度を設定する必要があります。

| **プライマリ（画面）アプリケーション内で実行された認証** | **セカンダリ （画面） アプリケーション内で実行された認証** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| プライマリ（ストリーミング）アプリケーションは、3～5秒以上ごとにポーリングする必要があります。 | プライマリ（ストリーミング）アプリケーションは、3～5秒以上ごとにポーリングする必要があります。 |

#### &#x200B;17. クライアントアプリケーションが送信できるポーリングリクエストの最大数は？ {#authentication-phase-faq17}

クライアントアプリケーションは、Adobe Pass認証[&#x200B; スロットリングメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits)で定義されている現在の制限に従う必要があります。

クライアントアプリケーションのエラー処理では、[429 Too Many Requests](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response) エラーコードを処理できる必要があります。これは、クライアントアプリケーションが許可されているリクエストの最大数を超えていることを示します。

詳しくは、[&#x200B; スロットリングメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)のドキュメントを参照してください。

#### &#x200B;18. クライアントアプリケーションはどのようにユーザーのメタデータ情報を取得できますか？ {#authentication-phase-faq18}

クライアントアプリケーションは、プロファイル情報の一部として[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)情報を返すことができる次のいずれかのエンドポイントをクエリできます。

* [プロファイルエンドポイント API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のMVPD APIのプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定の（認証）コード APIのプロファイルエンドポイント](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

認証フローが完了すると、ユーザーメタデータが使用可能になります。そのため、クライアントアプリケーションは、プロファイル情報に既に含まれているため、[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)情報を取得するために別のエンドポイントをクエリする必要はありません。

詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本的なプロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

特定のメタデータ属性は、MVPDと特定のメタデータ属性に応じて、認証フロー中に更新される場合があります。 その結果、クライアントアプリケーションは、最新のユーザーメタデータを取得するために、上記のAPIを再度クエリする必要がある場合があります。

#### &#x200B;19. クライアントアプリケーションは、アクセスの低下をどのように管理する必要がありますか？ {#authentication-phase-faq19}

[&#x200B; デグラデーション機能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)を使用すると、MVPDの認証サービスまたは認証サービスで問題が発生した場合でも、クライアントアプリケーションはユーザーにシームレスなストリーミング体験を提供できます。

要約すると、MVPDの一時的なサービスの中断にもかかわらず、コンテンツへの中断のないアクセスを確保できます。

組織がプレミアムのデプロイメント機能を使用する場合、クライアントアプリケーションはデプロイメントされたアクセスフローを処理する必要があり、そのようなシナリオでREST API v2 エンドポイントがどのように動作するかを概説します。

詳しくは、[&#x200B; デグレードされたアクセス フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)のドキュメントを参照してください。

#### &#x200B;20. クライアントアプリケーションは一時的なアクセスをどのように管理しますか？ {#authentication-phase-faq20}

[TempPass機能](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)を使用すると、クライアントアプリケーションはユーザーに一時的なアクセス権を提供できます。

要約すると、一定期間、コンテンツまたは定義済みの数のVODタイトルへの時間制限されたアクセスを提供することで、ユーザーをエンゲージできます。

組織がプレミアム TempPass機能を使用する場合、クライアントアプリケーションは一時的なアクセスフローを処理する必要があり、そのようなシナリオでREST API v2 エンドポイントがどのように動作するかを概説します。

以前のAPI バージョンでは、クライアントアプリケーションは、一時的なアクセスを提供するために、通常のMVPDで認証されたユーザーをログアウトする必要がありました。

REST API v2を使用すると、クライアントアプリケーションは、ストリームの承認時に通常のMVPDとTempPass MVPDをシームレスに切り替えることができ、ユーザーがログアウトする必要がなくなります。

詳しくは、[一時アクセスフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)のドキュメントを参照してください。

#### &#x200B;21. クライアントアプリケーションは、クロスデバイスのシングルサインオンアクセスをどのように管理する必要がありますか？ {#authentication-phase-faq21}

クライアントアプリケーションがデバイス間で一貫した一意のユーザーIDを提供する場合、REST API v2はクロスデバイスのシングルサインオンを有効にできます。

サービストークンと呼ばれるこの識別子は、選択した外部ID サービスの実装または統合を通じて、クライアントアプリケーションによって生成される必要があります。

詳しくは、「[&#x200B; サービストークンのフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)」のドキュメントを参照してください。

+++

### 事前認証フェーズに関するFAQ {#preauthorization-phase-faqs-general}

+++事前認証フェーズに関するFAQ

#### &#x200B;1. 事前認証フェーズの目的は何ですか？ {#preauthorization-phase-faq1}

事前認証フェーズの目的は、クライアントアプリケーションに、ユーザーがアクセスする権限を持つリソースのサブセットをカタログから提示する機能を提供することです。

事前認証フェーズでは、ユーザーが初めてクライアントアプリケーションを開いたり、新しいセクションに移動したりすると、ユーザーエクスペリエンスが向上します。

#### &#x200B;2. 事前認証フェーズは必須ですか？ {#preauthorization-phase-faq2}

事前認証フェーズは必須ではありません。クライアントアプリケーションは、ユーザーの使用権限に基づいて最初にリソースをフィルタリングせずにリソースのカタログを表示する場合、このフェーズをスキップできます。

#### &#x200B;3. 事前認証の決定とは何ですか？ {#preauthorization-phase-faq3}

事前認証は[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization) ドキュメントで定義されている用語ですが、決定用語は[用語集](rest-api-v2-glossary.md#decision)にも記載されています。

事前認証の決定には、決定事前認証エンドポイントから取得できるMVPD事前認証プロセスの問い合わせ結果に関する情報が格納されます。

クライアントアプリケーションは、事前承認決定を使用して、TV プロバイダー（情報）決定でユーザーがアクセスできるリソースのサブセットを表示できます。

詳しくは、次のドキュメントを参照してください。

* [事前承認決定APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. クライアントアプリケーションは、事前承認決定を永続的なストレージにキャッシュする必要がありますか？ {#preauthorization-phase-faq4}

クライアントアプリケーションは、事前認証の決定を永続ストレージに保存する必要はありません。 ただし、ユーザーエクスペリエンスを向上させるために、メモリ内で許可の決定をキャッシュすることをお勧めします。 これにより、既に事前承認されているリソースに対して、決定の事前承認エンドポイントへの不要な呼び出しを回避し、遅延を減らし、パフォーマンスを向上させることができます。

#### &#x200B;5. 事前認証の決定が拒否された理由をクライアントアプリケーションで判断するにはどうすればよいですか？ {#preauthorization-phase-faq5}

クライアントアプリケーションは、決定事前認証エンドポイントから応答に含まれる[&#x200B; エラーコードとメッセージ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)を調べることで、拒否された事前認証決定の理由を判断できます。 これらの詳細は、事前認証リクエストが拒否された具体的な理由をinsightに提供し、ユーザーエクスペリエンスまたはトリガーにアプリケーションで必要な処理を通知するのに役立ちます。

事前認証の決定を取得するために実装された再試行メカニズムが、事前認証の決定が拒否された場合に無限ループが発生しないようにします。

利用者に明確なフィードバックを提供することで、再試行を合理的な数に制限し、拒否を適切に処理することを検討してください。

#### &#x200B;6. 事前承認決定にメディアトークンが欠落しているのはなぜですか？ {#preauthorization-phase-faq6}

事前認証フェーズの目的は、リソースの再生に事前認証フェーズを使用してはならないため、事前認証の決定にメディアトークンがありません。

#### &#x200B;7. 事前認証の決定が既に存在する場合、認証フェーズをスキップできますか？ {#preauthorization-phase-faq7}

いいえ。

事前認証の決定が使用可能な場合でも、認証フェーズをスキップすることはできません。 事前認証の決定は情報提供のみであり、実際の再生権限は付与されません。 事前承認フェーズは、早期ガイダンスを提供することを目的としていますが、コンテンツを再生する前に承認フェーズが必要です。

#### &#x200B;8. リソースとは何か、サポートされている形式は何か？ {#preauthorization-phase-faq8}

リソースは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)のドキュメントで定義されている用語です。

リソースは、MVPDで合意された一意の識別子であり、クライアントアプリケーションがストリームできるコンテンツに関連付けられています。

リソースの一意のIDには、次の2つの形式を使用できます。

* チャネル（ブランド）の一意の識別子などの簡単な文字列形式。
* タイトル、評価、ペアレンタルコントロールのメタデータなどの追加情報を含むMedia RSS （MRSS）形式。

詳しくは、[保護リソース &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)のドキュメントを参照してください。

#### &#x200B;9. クライアントアプリケーションは、一度にどれだけのリソースで事前認証の決定を取得できますか？ {#preauthorization-phase-faq9}

クライアントアプリケーションは、MVPDによって課される条件により、1回のAPI リクエストで限られた数のリソースに対して、通常は最大5件の事前承認決定を取得できます。

このリソースの最大数は、Adobe Pass [TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じてMVPDと契約した後、組織の管理者またはAdobe Pass認証担当者が表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)のドキュメントを参照してください。

+++

### 承認フェーズに関するFAQ {#authorization-phase-faqs-general}

+++承認フェーズに関するFAQ

#### &#x200B;1. 承認段階の目的は何か？ {#authorization-phase-faq1}

認証フェーズの目的は、MVPDでユーザーの権利を検証した後、クライアントアプリケーションにユーザーがリクエストしたリソースを再生する機能を提供することです。

#### &#x200B;2. 認証フェーズは必須ですか？ {#authorization-phase-faq2}

認証フェーズは必須です。クライアントアプリケーションは、ユーザーがリクエストしたリソースを再生する場合、このフェーズをスキップできません。ユーザーがストリームをリリースする前に、MVPDで権限があることを確認する必要があるからです。

#### &#x200B;3. 認証の決定とは何ですか？また、その有効期間はどれくらいですか？ {#authorization-phase-faq3}

承認は[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization) ドキュメントで定義されている用語ですが、決定用語は[用語集](rest-api-v2-glossary.md#decision)でも確認できます。

認証の決定には、決定の認証エンドポイントから取得できるMVPD認証プロセスの問い合わせ結果に関する情報が格納されます。

クライアントアプリケーションは、メディアトークンを含む承認決定を使用して、TV プロバイダー（オーサスティナティブ）の決定によってユーザーがリソースストリームにアクセスできる場合に備えて、リソースストリームを再生できます。

詳しくは、次のドキュメントを参照してください。

* [認証の決定APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本的な認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

認証の判断は、問題の時点で指定された期間限定で有効であり、MVPDに対して再度クエリを実行する前にAdobe Pass認証によってキャッシュされる時間を示します。

認証（authZ） [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)と呼ばれるこの制限付き期間は、Adobe Pass [TVE ダッシュボード &#x200B;](rest-api-v2-glossary.md#tve-dashboard)を通じて、組織管理者の1人またはAdobe Pass認証担当者が代理で表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)のドキュメントを参照してください。

#### &#x200B;4. クライアントアプリケーションは、永続ストレージで承認決定をキャッシュしますか？ {#authorization-phase-faq4}

永続ストレージに認証の決定を保存するためにクライアントアプリケーションは必要ありません。

#### &#x200B;5. 認証の決定が拒否された理由をクライアントアプリケーションはどのように判断できますか？ {#authorization-phase-faq5}

クライアントアプリケーションは、「決定の承認」エンドポイントからの応答に含まれる[&#x200B; エラーコードとメッセージ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)を調べることで、拒否された承認決定の理由を判断できます。 これらの詳細は、insightに認証リクエストが拒否された具体的な理由を示し、ユーザーエクスペリエンスまたはトリガーにアプリケーションで必要な処理を通知するのに役立ちます。

承認決定を取得するために実装された再試行メカニズムが、承認決定が拒否された場合にエンドレスループにならないようにします。

利用者に明確なフィードバックを提供することで、再試行を合理的な数に制限し、拒否を適切に処理することを検討してください。

#### &#x200B;6. メディアトークンとは何か？また、その有効期間は？ {#authorization-phase-faq6}

メディアトークンは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token)のドキュメントで定義されている用語です。

メディアトークンは、決定承認エンドポイントから取得できるクリアテキストで送信される署名済み文字列で構成されます。

詳しくは、[Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)のドキュメントを参照してください。

メディアトークンは、問題の時点で指定された制限された短い期間に対して有効であり、クライアントアプリケーションで検証して使用する必要がある時間制限を示します。

クライアントアプリケーションは、TV プロバイダー（権威）の決定によってユーザーがリソースストリームにアクセスできる場合に備えて、メディアトークンを使用してリソースストリームを再生できます。

詳しくは、次のドキュメントを参照してください。

* [認証の決定APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本的な認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. リソースストリームを再生する前に、クライアントアプリケーションでメディアトークンを検証する必要がありますか？ {#authorization-phase-faq7}

はい。

クライアントアプリケーションは、リソースストリームの再生を開始する前に、メディアトークンを検証する必要があります。 この検証は、[Media Token Verifier](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)を使用して実行する必要があります。 返された`token`から`serializedToken`を確認することで、クライアントアプリケーションは、ストリームリッピングなどの不正アクセスを防ぐのに役立ち、適切に承認されたユーザーのみがコンテンツを再生できるようになります。

#### &#x200B;8. 再生中にクライアントアプリケーションが期限切れのメディアトークンを更新する必要がありますか？ {#authorization-phase-faq8}

いいえ。

ストリームがアクティブに再生されている間、クライアントアプリケーションは期限切れのメディアトークンを更新する必要はありません。 再生中にメディアトークンが期限切れになった場合は、ストリームを中断せずに続行できるようにする必要があります。 ただし、クライアントは、ユーザーがリソースの再生を試みたときに、新しい承認決定をリクエストし、新しいメディアトークンを取得する必要があります。

#### &#x200B;9. 承認決定における各タイムスタンプ属性の目的は何ですか？ {#authorization-phase-faq9}

認証の決定には、認証自体と関連するメディアトークンの有効期間に関する重要なコンテキストを提供する複数のタイムスタンプ属性が含まれます。 これらのタイムスタンプは、認証の決定とメディアトークンのどちらに関連しているかに応じて、異なる目的を果たします。

**決定レベルのタイムスタンプ**

これらのタイムスタンプは、全体的な認証決定の有効期間を表します。

| 属性 | 説明 | メモ |
|-------------|-------------|---------|
| `notBefore` | 承認決定が発行された時間（ミリ秒単位）。 | これは、認証有効ウィンドウの開始を示します。 |
| `notAfter` | 認証の決定が期限切れになるまでの時間（ミリ秒単位）。 | [認証の有効期間（TTL） &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management)は、再認証が必要になるまでの有効期間を決定します。 このTTLはMVPDの担当者と交渉されます。 |

**トークンレベルのタイムスタンプ**

これらのタイムスタンプは、承認決定に関連付けられたメディアトークンの有効期間を表します。

| 属性 | 説明 | メモ |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | メディアトークンが発行された時間（ミリ秒単位）。 | これは、トークンが再生に有効になったときにマークされます。 |
| `notAfter` | メディアトークンの有効期限が切れる時間（ミリ秒）。 | メディアトークンの使用期間は意図的に短く（通常は7分）、誤用リスクを最小限に抑え、トークン生成サーバーとトークン検証サーバー間の潜在的なクロックの違いを考慮します。 |

#### &#x200B;10. リソースとは何か、サポートされている形式は何か？ {#authorization-phase-faq10}

リソースは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)のドキュメントで定義されている用語です。

リソースは、MVPDで合意された一意の識別子であり、クライアントアプリケーションがストリームできるコンテンツに関連付けられています。

リソースの一意のIDには、次の2つの形式を使用できます。

* チャネル（ブランド）の一意の識別子などの簡単な文字列形式。
* タイトル、評価、ペアレンタルコントロールのメタデータなどの追加情報を含むMedia RSS （MRSS）形式。

詳しくは、[保護リソース &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)のドキュメントを参照してください。

#### &#x200B;11. クライアントアプリケーションは、一度にどれだけのリソースを認証の決定を取得できますか？ {#authorization-phase-faq11}

クライアントアプリケーションは、MVPDによって課される条件により、1回のAPI リクエストで限られた数のリソースに対して承認決定を取得できます（通常は1件まで）。

+++

### ログアウトフェーズに関するFAQ {#logout-phase-faqs-general}

+++ログアウトフェーズに関するFAQ

#### &#x200B;1. ログアウトフェーズの目的は何ですか？ {#logout-phase-faq1}

ログアウトフェーズの目的は、ユーザーリクエストに応じて、Adobe Pass認証内でユーザーの認証プロファイルを終了する機能をクライアントアプリケーションに提供することです。

#### &#x200B;2. ログアウトフェーズは必須ですか？ {#logout-phase-faq2}

ログアウトフェーズは必須です。クライアントアプリケーションは、ユーザーにログアウト機能を提供する必要があります。

+++

### Headers FAQ {#headers-faqs-general}

+++Headers FAQ

#### &#x200B;1. Authorization ヘッダーの値を計算する方法を教えてください。 {#headers-faq1}

>[!IMPORTANT]
>
> クライアントアプリケーションがREST API V1からREST API V2に移行している場合、クライアントアプリケーションは引き続き同じメソッドを使用して、以前と同じ`Bearer` アクセストークン値を取得できます。

[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) リクエストヘッダーには、クライアントアプリケーションがAdobe Passで保護されたAPIにアクセスするために必要な`Bearer` アクセストークンが含まれています。

認証ヘッダー値は、登録フェーズ中にAdobe Pass認証から取得する必要があります。

詳しくは、次のドキュメントを参照してください。

* [動的なクライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [クライアント資格情報APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークン APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. AP-Device-Identifier ヘッダーの値の計算方法を教えてください。 {#headers-faq2}

>[!IMPORTANT]
>
> クライアントアプリケーションがREST API V1からREST API V2に移行する場合、クライアントアプリケーションは引き続き同じ方法を使用して、デバイス識別子の値を以前と同じように計算できます。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) リクエストヘッダーには、クライアントアプリケーションによって作成されたストリーミングデバイス識別子が含まれています。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ヘッダーのドキュメントには、値の計算方法の主要なプラットフォームの例が記載されていますが、クライアントアプリケーションは独自のビジネスロジックと要件に基づいて別の方法を使用できます。

#### &#x200B;3. X-Device-Info ヘッダーの値を計算する方法 {#headers-faq3}

>[!IMPORTANT]
>
> クライアントアプリケーションがREST API V1からREST API V2に移行する場合、クライアントアプリケーションは引き続き同じ方法を使用して、デバイス情報の値を以前と同じように計算できます。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) リクエストヘッダーには、実際のストリーミングデバイスに関連するクライアント情報（デバイス、接続、アプリケーション）が含まれており、MVPDが適用する可能性のあるプラットフォーム固有のルールを決定するために使用されます。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ヘッダーのドキュメントには、値の計算方法の主要なプラットフォームの例が記載されていますが、クライアントアプリケーションは、独自のビジネスロジックと要件に基づいて別の方法を使用できます。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ヘッダーが見つからないか、誤った値が含まれている場合、リクエストは`unknown` プラットフォームから送信されたものとして分類される可能性があります。

これにより、リクエストが安全でないと扱われ、認証TTLが短くなるなど、より制限のあるルールの対象となる可能性があります。 さらに、ストリーミングデバイス `connectionIp`や`connectionPort`などの一部のフィールドは、Spectrumの[&#x200B; ホームベース認証](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)などの機能に必須です。

リクエストがデバイスの代理でサーバーから送信された場合でも、[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ヘッダー値は、実際のストリーミングデバイス情報を反映する必要があります。

+++

### その他のFAQ {#misc-faqs-general}

+++その他のFAQ

#### &#x200B;1. REST API V2のリクエストと応答を調べて、APIをテストできますか？ {#misc-faq1}

はい。

REST API V2は、専用の[Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトから検索できます。 Adobe Developerのweb サイトでは、以下に無制限にアクセスできます。

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)とやり取りするには、[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)を介して取得した`Bearer` アクセストークンを[認証](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) ヘッダーに含める必要があります。

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)を使用するには、REST API V2 スコープを持つソフトウェア ステートメントが必要です。 詳しくは、[Dynamic Client Registration （DCR）に関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)を参照してください。

#### &#x200B;2. OpenAPI サポートを備えたAPI開発ツールを使用して、REST API V2 リクエストとレスポンスを調査できますか？ {#misc-faq2}

はい。

[Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトから[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)および[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)のOpenAPI仕様ファイルをダウンロードできます。

OpenAPI仕様ファイルをダウンロードするには、ダウンロードボタンをクリックして、次のファイルをローカルマシンに保存します。

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

次に、これらのファイルを任意のAPI開発ツールにインポートして、REST API V2のリクエストと応答を確認し、APIをテストします。

#### &#x200B;3. https://sp.auth-staging.adobe.com/apitest/api.htmlでホストされている既存のAPI テストツールを引き続き使用できますか？ {#misc-faq3}

いいえ。

REST API V2に移行するクライアントアプリケーションでは、https://developer.adobe.com/adobe-pass/でホストされている新しいテストツールを使用する必要があります。 Adobe Developerのweb サイトでは、以下に無制限にアクセスできます。

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)とやり取りするには、[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)を介して取得した`Bearer` アクセストークンを[認証](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) ヘッダーに含める必要があります。

[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)を使用するには、REST API V2 スコープを持つソフトウェア ステートメントが必要です。 詳しくは、[Dynamic Client Registration （DCR）に関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)を参照してください。

+++

### Apple SSOに関するよくある質問 {#apple-sso-general}

+++Apple SSOに関するよくある質問

#### &#x200B;1. Apple SSOとは何ですか？また、REST API V2とどのように連携しますか？ {#apple-sso-faq1}

Apple SSO （シングルサインオン）を使用すると、Appleの[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)を使用して、デバイスシステムレベルでTV プロバイダーアカウントにサインインできるので、アプリごとに認証する必要はありません。

REST API V2は、パートナーフローを介してiOS、iPadOS、またはtvOSで動作するクライアントアプリケーションのエンドユーザー向けのパートナーシングルサインオン（SSO）をサポートしています。

詳しくは、次のドキュメントを参照してください。

* [Apple SSOの概要](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
* [Apple SSO クックブック （REST API V2）](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
* [パートナーフローを使用したシングルサインオン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)

#### &#x200B;2. Apple SSOを導入するための前提条件は何ですか？ {#apple-sso-faq2}

Apple SSOを実装する前に、次の前提条件を満たしていることを確認してください。

**ストリーミングアプリケーションの要件：**

* Apple チーム IDの一部として[&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount)を有効にするには、Appleにお問い合わせください。
* Apple Developer アカウントの一部として、ビデオ購読者のシングルサインオン使用権限を設定します。
* Xcode バージョン 8以上およびiOS/tvOS バージョン 10以上を使用してください。
* デバイスレベルでサブスクリプション情報にアクセスするためのユーザー権限をリクエストします。
* Adobe Pass Authentication バックエンドがデバイスプラットフォームを識別できるように、`X-Device-Info`および/または`User-Agent` ヘッダーを含めます。
* 関連するすべてのAPI リクエストに、有効なパートナーフレームワークのステータスを含む`AP-Partner-Framework-Status` ヘッダーを含めます。

**Adobe Pass設定：**

* `Enable Single Sign On` プロパティを`Yes`に設定して、Adobe Pass TVE ダッシュボードを通じて、目的の統合およびプラットフォーム（iOS/tvOS）ごとにシングルサインオン（SSO）を有効にします。

**MVPDの要件：**

* MVPDは、Apple SSOをサポートするためにAppleをオンボーディングする必要があります。
* MVPDは、パートナーSSO設定のためにAdobe Pass認証を使用してオンボーディングする必要があります。

詳しくは、[Apple SSOの概要 – 前提条件](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites)のドキュメントを参照してください。

#### &#x200B;3. AP-Partner-Framework-Status ヘッダーとは何ですか？なぜそれが必要なのですか？ {#apple-sso-faq3}

`AP-Partner-Framework-Status` ヘッダーには、AppleのVideo Subscriber Account Frameworkから取得したパートナーフレームワークのステータス情報が含まれています。

**常に必須：**

* [パートナー認証リクエストの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [パートナー認証応答を使用したプロファイルの作成と取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**条件付き必須：**

* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のmvpdのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のmvpdを使用して認証の決定を取得する](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [特定のmvpdを使用して事前承認決定を取得する](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

ストリーミングアプリケーションは、ビデオ購読者アカウントフレームワークを呼び出して、この情報を取得し、ヘッダーにbase64 エンコードされたJSON ペイロードとして含める必要があります。

**ベストプラクティス：**

* ストリーミングアプリケーションは、アプリケーションがフォアグラウンド状態になると、パートナーフレームワークのステータスを取得する必要があります。
* パートナーフレームワークのステータス情報をキャッシュし、アプリケーションがバックグラウンドからフォアグラウンドに移行したときに更新します。
* パートナーフレームワークのステータスに有効な値（許可されたユーザー権限、有効なプロバイダー識別子、有効な有効期限）が含まれていることを確認します。

詳しくは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)のドキュメントを参照してください。

#### &#x200B;4. Apple SSOの問題のトラブルシューティング方法を教えてください。 {#apple-sso-faq4}

Apple SSOの問題をトラブルシューティングする場合は、次の一般的なアプローチに従います。

**手順1: SAML リクエスト生成の検証**

* [&#x200B; パートナー認証リクエストの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) エンドポイントが有効なパートナー認証リクエスト （SAML リクエスト）を返していることを確認します。
* base64 デコード後に、`authenticationRequest - request`属性に適切な形式のSAML リクエストが含まれていることを確認します。
* `AP-Partner-Framework-Status` ヘッダーに有効なパートナーフレームワークのステータス情報が含まれていることを確認します。

**手順2: VSA エラーの特定**

* ビデオ購読者アカウントフレームワークからのエラー応答を確認します。
* エラーコードと説明については、[&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vserror#Error-codes)のドキュメントを参照してください。
* VSA エラードキュメントはかなり一般的なものであり、詳細な根本原因の情報を提供しない可能性があることに注意してください。

**手順3：一般的な問題の確認**

* ユーザー権限のアクセス状態を付与する必要があります（チェック `VSAccountAccessStatus.granted`）。
* ユーザープロバイダーマッピング IDが存在し、有効である必要があります（`accountProviderIdentifier`）。
* ユーザープロバイダープロファイルの有効期限は有効である必要があります（`authenticationExpirationDate`）。
* MVPDはAppleでオンボーディングする必要があります（Configuration エンドポイントの応答で`boardingStatus`を確認してください）。
* MVPD統合では、TVE ダッシュボードでApple SSOを有効にする必要があります。

**手順4:MVPDを調査に参加させる**

* ビデオ購読者アカウントフレームワークが有効なSAML リクエストを受け取ったものの、MVPDのインタラクション後にSAML レスポンスを返さない場合は、Apple SSO対応MVPDを含めてトラブルシューティングを行います。
* この問題は、Apple側でのMVPD固有の設定または実装に関連している場合もあります。

#### &#x200B;5. 一般的なVSAのエラーとその対処方法を教えてください。 {#apple-sso-faq5}

一般的なシナリオとその取り扱い：

**ユーザーが権限を拒否しました：**

* `VSAccountAccessStatus`は`.granted`ではありません。
* 基本認証フローに戻り、アプリケーション独自のMVPD ピッカーを表示します。

**MVPDがAppleでオンボーディングされていません（エラーコード 1）:**

* VSA フレームワークが`error.code == 1`のエラーを返します。
* `error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"]`には、Apple MSO IDが含まれています。
* 基本の認証フローに戻りますが、Apple MSO IDを設定のMVPDにマッピングできる場合は、MVPD ピッカーを使用してユーザーにプロンプトを表示することをスキップできます。

**メタデータが返されませんでした：**

* `vsaMetadata`が`nil`であるか、必須フィールドがありません。
* 基本認証フローにフォールバックします。

**SAML応答が返されませんでした：**

* MVPD認証後の`samlAttributeQueryResponse`は`nil`です。
* これは、MVPDのApple SSOの実装に関する問題を示している可能性があります。
* 調査については、Adobe、MVPD、Appleに問い合わせることを検討してください。

エラー情報の詳細については、[&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount)のドキュメントを参照してください。

#### &#x200B;6. プロファイルタイプ「appleSSO」は何を示しますか？ {#apple-sso-faq6}

`type`が「appleSSO」に設定されているプロファイルは、ビデオ購読者アカウントフレームワークを使用してAppleのパートナーシングルサインオンフローを通じて認証されたユーザーであることを示します。

このプロファイルタイプには特定の要件があります。

* 「appleSSO」プロファイルを使用して意思決定（事前認証/認証）要求を行う場合、ストリーミングアプリケーションには、現在のパートナーフレームワークのステータスを持つ有効な`AP-Partner-Framework-Status` ヘッダーを含める必要があります。
* ログアウト時の応答には、`actionName`が「partner_logout」に設定され、`actionType`が「partner_interactive」に設定され、ユーザーがパートナー（システム）レベルでログアウトを完了する必要があることを示します。

通常のプロファイル（Apple SSO以外）には、これらの要件はなく、基本的な認証フローに従います。

#### &#x200B;7. Apple SSO プロファイルのログアウトはどのように処理しますか？ {#apple-sso-faq7}

「appleSSO」タイプのプロファイルを持つユーザーのログアウトを開始する場合：

* Adobe Pass ログアウトエンドポイントの応答には、次のものが含まれます。
   * `actionName`が「partner_logout」に設定されました
   * `actionType`が「partner_interactive」に設定されました
   * `url`属性が見つかりません

* ストリーミングアプリケーションは、次の場所に移動して、パートナー（システム）レベルでログアウトプロセスを完了するようにユーザーに促す必要があります。
   * iOS/iPadOSの`Settings -> TV Provider`
   * tvOSの`Settings -> Accounts -> TV Provider`

* ユーザーは、ログアウトプロセスを完了するために、システムレベルでTV プロバイダーから手動でログアウトする必要があります。

詳しくは、[Apple SSO Cookbook （REST API V2） – ログアウトフェーズ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md#cookbook)のドキュメントを参照してください。

#### &#x200B;8. Apple SSOが失敗した場合、基本認証にフォールバックできますか？ {#apple-sso-faq8}

はい。Adobe Pass Authentication REST API V2は、次のシナリオで基本認証フローに自動的にフォールバックします。

**自動フォールバック：**

* Adobe Pass バックエンドでパートナーシングルサインオンの検証が失敗する
* ユーザーがサブスクリプション情報へのアクセス権限を拒否
* MVPDはAppleのオンボーディングを行っていません
* VSA Frameworkは有効なメタデータを返しません

**応答表示：**

基本認証にフォールバックすると、[&#x200B; パートナー認証リクエストの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) エンドポイント応答に次の内容が含まれます。

* `actionName`が「認証」または「再開」に設定されました
* `actionType`が「インタラクティブ」または「ダイレクト」に設定されました

ストリーミングアプリケーションは、基本認証フローを開始することでこれらの応答を処理する必要があります。

**手動フォールバック：**

特定の統合に対してApple SSOを無効にし、常に基本認証を使用するには、次の手順を実行します。

* 目的の統合とプラットフォーム（iOS/tvOS）用に、Adobe Pass TVE ダッシュボードの`Enable Single Sign On` プロパティを`No`に設定します。

詳しくは、[&#x200B; パートナーフローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)のドキュメントを参照してください。

#### &#x200B;9. ビデオサブスクライバーアカウントフレームワークに関する詳細はどこで確認できますか？ {#apple-sso-faq9}

API参照、エラーコード、統合ガイドラインなど、AppleのVideo Subscriber Account Frameworkについて詳しくは、Appleの公式ドキュメントを参照してください。

* [ビデオ購読者アカウントフレームワークのドキュメント](https://developer.apple.com/documentation/videosubscriberaccount)

確認すべき主要なクラスとプロトコル：

* [VSAccountManager](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager) – 購読者アカウント操作のメイン マネージャー
* [VSAccountMetadataRequest](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) – 購読者アカウント情報の要求
* [VSAccountMetadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) – 購読者アカウント情報を含む応答
* [VSAccountManagerDelegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) - アカウント マネージャーイベントのデリゲート プロトコル
* [VSAccountAccessStatus](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountaccessstatus) - ユーザー権限のステータス列挙

+++

## 移行に関するFAQ {#migration-faqs}

既存のアプリケーションをREST API V2に移行する必要があるアプリケーションを使用している場合は、この節を続行します。

>[!MORELIKETHIS]
>
> * [動的クライアント登録（DCR）に関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### 一般的な移行に関するFAQ {#general-migration-faqs}

+++一般的な移行に関するFAQ

#### &#x200B;1. REST API V2に移行された新しいクライアントアプリケーションをすべてのユーザーに一度にロールアウトする必要がありますか？ {#migration-faq1}

いいえ。

クライアントアプリケーションは、REST API V2を統合する新しいバージョンをすべてのユーザーに同時にロールアウトする必要はありません。

Adobe Pass Authenticationは、2025年末まで、REST API V1またはSDKを統合する古いクライアントアプリケーションのバージョンを引き続きサポートします。

#### &#x200B;2. REST API V2に移行された新しいクライアントアプリケーションを、すべてのAPIとフローで一度にロールアウトする必要がありますか？ {#migration-faq2}

はい。

クライアントアプリケーションは、すべてのAdobe Pass認証APIとフローを同時に使用して、REST API V2を統合する新しいバージョンをロールアウトする必要があります。

「2番目の画面認証」フローの場合、クライアントアプリケーションは、[primary](rest-api-v2-glossary.md#primary-application)と[secondary](rest-api-v2-glossary.md#secondary-application)の両方のアプリケーションに対して、REST API V2を統合する新しいバージョンを同時にロールアウトする必要があります。

Adobe Pass Authenticationでは、APIとフロー間でREST API V2とREST API V1/SDKの両方を統合する「ハイブリッド」実装はサポートされません。

#### &#x200B;3. REST API V2に移行された新しいクライアントアプリケーションに更新する際に、ユーザー認証は保持されますか？ {#migration-faq3}

いいえ。

REST API V1またはSDKを統合した古いクライアントアプリケーションのバージョンで取得したユーザー認証は保持されません。

したがって、ユーザーは、REST API V2に移行された新しいクライアントアプリケーション内で再認証する必要があります。

#### &#x200B;4. REST API V2では、拡張エラーコードがデフォルトで有効になっていますか？ {#migration-faq4}

はい。

REST API V2に移行するクライアントアプリケーションは、デフォルトでこの機能のメリットを自動的に受け取り、より詳細で正確なエラー情報を提供します。

詳しくは、[拡張エラーコード &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)のドキュメントを参照してください。

#### &#x200B;5. REST API V2に移行する際に、既存の統合で設定変更が必要ですか？ {#migration-faq5}

いいえ。

REST API V2に移行するクライアントアプリケーションでは、既存のMVPD統合の設定変更は必要ありません。 また、既存の[&#x200B; サービスプロバイダー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider)および[MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd)に対して引き続き同じ識別子を使用します。

さらに、Adobe Pass認証でMVPD エンドポイントとの通信に使用されるプロトコルは変更されません。

+++

### REST API V1からREST API V2への移行 {#migration-rest-api-v1-to-rest-api-v2-faqs}

REST API V1からREST API V2に移行する必要があるアプリケーションを使用している場合は、このサブセクションを続行します。

#### 登録フェーズに関するFAQ {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++登録フェーズに関するFAQ

##### &#x200B;1. 登録フェーズに必要な高レベルのAPI移行は何ですか？ {#registration-phase-v1-to-v2-faq1}

REST API V1からREST API V2への移行では、登録フェーズに関する大きな変更はありません。

クライアントアプリケーションは、引き続き同じフローを使用して、[Dynamic Client Registration （DCR） &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) プロセスを通じてAdobe Pass Authenticationに対して登録し、アクセストークンを取得できます。

詳しくは、次のドキュメントを参照してください。

* [動的なクライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [クライアント資格情報APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークン APIの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### 設定段階に関するFAQ {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++設定段階に関するFAQ

##### &#x200B;1. 設定フェーズに必要な高レベルのAPI移行は何ですか？ {#configuration-phase-v1-to-v2-faq1}

REST API V1からREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

| 範囲 | REST API V1 | REST API V2 | 観察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つMVPDのリストを取得 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 認証フェーズに関するFAQ {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++認証フェーズに関するFAQ

##### &#x200B;1. 認証フェーズに必要な高レベル APIの移行は何ですか？ {#authentication-phase-v1-to-v2-faq1}

REST API V1からREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

| 範囲 | REST API V1 | REST API V2 | 観察 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コード（認証コード）を確認する | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2つ目の画面（アプリケーション）で（MVPD）認証を再開する | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証を開始 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [GET <br/> /api/v1/checkauthn （最初の画面） &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn （2つ目の画面） &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザー認証トークン（プロファイル）の取得 | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーのメタデータ情報の取得 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 事前認証フェーズに関するFAQ {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++事前認証フェーズに関するFAQ

##### &#x200B;1. 事前認証フェーズに必要な高レベルのAPI移行は何ですか？ {#preauthorization-phase-v1-to-v2-faq1}

REST API V1からREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

| 範囲 | REST API V1 | REST API V2 | 観察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認済みリソースの取得（事前承認済みの決定） | [GET <br/> /api/v1/preauthorize （最初の画面） &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize （second screen） &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的な事前承認フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 承認フェーズに関するFAQ {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++承認フェーズに関するFAQ

##### &#x200B;1. 承認フェーズに必要な高レベルのAPI移行は何ですか？ {#authorization-phase-v1-to-v2-faq1}

REST API V1からREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

| 範囲 | REST API V1 | REST API V2 | 観察 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証を開始 | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 認証トークンの取得（承認決定） | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 短い認証トークン（メディアトークン）の取得 | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### ログアウトフェーズに関するFAQ {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++ログアウトフェーズに関するFAQ

##### &#x200B;1. ログアウトフェーズに必要な高レベル API移行は何ですか？ {#logout-phase-v1-to-v2-faq1}

REST API V1からREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

| 範囲 | REST API V1 | REST API V2 | 観察 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトを開始 | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的なログアウトフローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### SDKからREST API V2への移行 {#migration-sdk-to-rest-api-v2}

SDKからREST API V2に移行する必要があるアプリケーションを使用している場合は、このサブセクションを続行します。

#### 登録フェーズに関するFAQ {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++登録フェーズに関するFAQ

##### &#x200B;1. 登録フェーズに必要な高レベルのAPI移行は何ですか？ {#registration-phase-sdk-to-v2-faq1}

SDKからREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

###### AccessEnabler JavaScript SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェアステートメントの提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェアステートメントの提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェアステートメントの提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェアステートメントの提供 | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動的なクライアント登録フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### 設定段階に関するFAQ {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++設定段階に関するFAQ

##### &#x200B;1. 設定フェーズに必要な高レベルのAPI移行は何ですか？ {#configuration-phase-sdk-to-v2-faq1}

SDKからREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

###### AccessEnabler JavaScript SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つMVPDのリストを取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つMVPDのリストを取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つMVPDのリストを取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つMVPDのリストを取得 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 認証フェーズに関するFAQ {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++認証フェーズに関するFAQ

##### &#x200B;1. 認証フェーズに必要な高レベル APIの移行は何ですか？ {#authentication-phase-sdk-to-v2-faq1}

SDKからREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

###### AccessEnabler JavaScript SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証を開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーのメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証を開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーのメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コード（認証コード）を確認する | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2つ目の画面（アプリケーション）で（MVPD）認証を再開する | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証を開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーのメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範囲 | SDK | REST API V2 | 観察 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証を開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーのメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コード（認証コード）を確認する | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2つ目の画面（アプリケーション）で（MVPD）認証を再開する | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証を開始 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[&#x200B; セカンダリ アプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーのメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 次のいずれかを使用してください：<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、これらのAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルの取得</li><li>ユーザーのメタデータ情報の取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>セカンダリ アプリケーション内で[基本プロファイル フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 事前認証フェーズに関するFAQ {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++事前認証フェーズに関するFAQ

##### &#x200B;1. 事前認証フェーズに必要な高レベルのAPI移行は何ですか？ {#preauthorization-phase-sdk-to-v2-faq1}

SDKからREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

###### AccessEnabler JavaScript SDK

| 範囲 | SDK | REST API V2 | 観察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認済みリソースの取得（事前承認済みの決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的な事前承認フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認済みリソースの取得（事前承認済みの決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的な事前承認フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範囲 | SDK | REST API V2 | 観察 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認済みリソースの取得（事前承認済みの決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的な事前承認フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 範囲 | SDK | REST API V2 | 観察 |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認済みリソースの取得（事前承認済みの決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的な事前承認フローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 承認フェーズに関するFAQ {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++承認フェーズに関するFAQ

##### &#x200B;1. 承認フェーズに必要な高レベルのAPI移行は何ですか？ {#authorization-phase-sdk-to-v2-faq1}

SDKからREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

###### AccessEnabler JavaScript SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークン（メディアトークン）の取得 | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークン（メディアトークン）の取得 | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークン（メディアトークン）の取得 | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 短い認証トークン（メディアトークン）の取得 | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、このAPIの応答を一度に複数の目的で使用できます：<br/> <ul><li>（MVPD）認証を開始</li><li>承認決定の取得</li><li>ショートメディアトークンの取得</li></ul> <br/>詳細については、次のドキュメントを参照してください：<br/> <ul><li>[&#x200B; プライマリアプリケーション内で実行された基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### ログアウトフェーズに関するFAQ {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++ログアウトフェーズに関するFAQ

##### &#x200B;1. ログアウトフェーズに必要な高レベル API移行は何ですか？ {#logout-phase-sdk-to-v2-faq1}

SDKからREST API V2への移行では、次の表に示す大きな変更を考慮する必要があります。

###### AccessEnabler JavaScript SDK

| 範囲 | SDK | REST API V2 | 観察 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトを開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的なログアウトフローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトを開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的なログアウトフローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範囲 | SDK | REST API V2 | 観察 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトを開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的なログアウトフローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範囲 | SDK | REST API V2 | 観察 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトを開始 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳細については、次のドキュメントを参照してください。<br/> <ul><li>[&#x200B; プライマリアプリケーション内で基本的なログアウトフローが実行されました](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
