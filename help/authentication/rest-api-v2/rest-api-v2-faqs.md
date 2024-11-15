---
title: REST API V2 の FAQ
description: REST API V2 の FAQ
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# REST API V2 の FAQ {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass認証 REST API V2 の導入に関するよくある質問に対する概要の回答を示します。

REST API V2 全体について詳しくは、[REST API V2 の概要 ](./rest-api-v2-overview.md) ドキュメントを参照してください。

## 一般的な FAQ {#general-faqs}

[REST API V1](#migrate-rest-api-v1-to-rest-api-v2) または [SDK](#migrate-sdk-to-rest-api-v2) から移行する新しいアプリケーションや既存のアプリケーションにかかわらず、REST API V2 を統合する必要があるアプリケーションを使用している場合は、この節から開始してください。

移行の詳細と手順については、次の節も参照してください。

+++登録フェーズに関するよくある質問

### 1.登録段階の目的は何ですか？ {#registration-phase-faq1}

登録フェーズの目的は、[Dynamic Client Registration （DCR） ](./rest-api-v2-glossary.md#dcr) プロセスを通じて、Adobe Pass Authentication に対してクライアントアプリケーションを登録することです。

動的クライアント登録（DCR）プロセスでは、クライアントアプリケーションがクライアント資格情報のペアを取得し、登録フェーズの最終目標としてアクセストークンを取得する必要があります。

詳しくは、[Dynamic Client Registration Overview](/help/authentication/dcr-api/dynamic-client-registration-overview.md) ドキュメントを参照してください。

### 2.登録フェーズは必須ですか？ {#registration-phase-faq2}

登録フェーズは必須ですが、クライアント資格情報とアクセストークンのペアがキャッシュされていて、それらが引き続き有効な場合、クライアントアプリケーションはこのフェーズをスキップできます。

### 3. ソフトウェアのステートメントとは何ですか？また、有効期限はどのくらいですか？ {#registration-phase-faq3}

ソフトウェア ステートメントは、[Glossary](./rest-api-v2-glossary.md#software-statement) ドキュメントで定義されている用語です。

このソフトウェア ステートメントは、組織管理者の 1 人が、またはユーザーに代わってAdobe Pass認証担当者がAdobe Pass[TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) から生成およびダウンロードできる JSON web トークン（JWT）で構成されます。

このソフトウェアのステートメントは無期限に有効ですが、Adobe Pass認証担当者にいつでも取り消しを依頼することもできます。

クライアントアプリケーションは、ソフトウェア文を保存し、クライアント資格情報を取得する必要がある場合に使用する必要があります。

詳しくは、[ 動的クライアント登録の概要 ](/help/authentication/dcr-api/dynamic-client-registration-overview.md) ドキュメントを参照してください。

### 4. ソフトウェア・ステートメントを生成およびダウンロードする方法は？ {#registration-phase-faq4}

この操作は、組織管理者の 1 人がAdobe Pass[TVE ダッシュボード ](./rest-api-v2-glossary.md#tve-dashboard) を使用して、またはお客様に代わってAdobe Pass認証担当者が実行できます。

詳しくは、[TVE ダッシュボードチャネルユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) または [TVE ダッシュボードプログラマーユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) ドキュメントを参照してください。

### 5. ソフトウェアのステートメントが取り消されるとどうなりますか？ {#registration-phase-faq5}

ソフトウェアのステートメントが取り消された場合、考慮すべき重要な結果が 1 つあります。

* 失効したソフトウェア文を使用するクライアントアプリケーションは [ 使用権限 ](./rest-api-v2-glossary.md#entitlement) フローを実行できなくなるので、ユーザーによるコンテンツの再生がブロックされます。

### 6. クライアント資格情報とは何ですか？また、どれくらい有効ですか？ {#registration-phase-faq6}

クライアント資格情報とは、[ 用語集 ](./rest-api-v2-glossary.md#client-credentials) ドキュメントで定義されている用語です。

クライアント資格情報は、クライアント登録エンドポイントから取得できるクライアント識別子とクライアント秘密鍵のペアで構成されます。

クライアント資格情報は無制限の期間に対して有効です。

クライアントアプリケーションは、クライアント資格情報を無期限に保存し、アクセストークンを取得する必要がある場合に使用する必要があります。

詳しくは、[ クライアント資格情報の取得 ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) ドキュメントを参照してください。

### 7. クライアント資格情報の管理方法 {#registration-phase-faq7}

Adobe Pass Authentication を使用してクライアントとサーバーの両方を統合する場合は、クライアントアプリケーションでユーザーアプリケーションインスタンスごとに一意のクライアント資格情報のペアを管理することをお勧めします。

クライアントアプリケーションは、クライアント資格情報を無期限に保存し、アクセストークンを取得する必要がある場合に使用する必要があります。

### 8. キャッシュされたクライアント資格情報が失われた場合はどうなりますか？ {#registration-phase-faq8}

キャッシュされたクライアント資格情報が失われた場合、考慮すべき重要な結果は次の 3 つです。

* クライアントアプリケーションは、新しいクライアント資格情報のペアを取得する必要があります。
* クライアントアプリケーションは、新しいクライアント資格情報のペアを使用して、新しいアクセストークンを取得する必要があります。
* クライアントアプリケーションは、以前に取得した認証済みプロファイルへのアクセスを失うので、クライアントアプリケーションはユーザーに再認証を求める必要があります。

### 9. アクセストークンとは何ですか？また、どれくらい有効ですか？ {#registration-phase-faq9}

アクセストークンとは、[Glossary](./rest-api-v2-glossary.md#access-token) ドキュメントで定義されている用語です。

アクセストークンは、クライアントトークンエンドポイントから取得できる [ ベアラートークン ](./appendix/headers/rest-api-v2-appendix-headers-authorization.md) で構成されています。

アクセストークンは、発行時に指定された限られた短い期間のみ有効です。

クライアントアプリケーションは、REST API V2 をターゲット設定する際に、アクセストークンを保存し、有効期限が切れるまで使用する必要があります。

クライアントアプリケーションは、現在のアクセストークンの有効期限が切れる前に、承認されていないリクエストを防ぐために、新しいアクセストークンを取得する必要があります。

詳しくは、[ アクセストークンの取得 ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントを参照してください。

### 10. クライアントアプリケーションは、アクセストークンをどのように更新できますか。 {#registration-phase-faq10}

クライアントアプリケーションは、新しいアクセストークンを取得するのと同じ方法で、キャッシュされたクライアント資格情報を使用してアクセストークンを更新する必要があります。

クライアントアプリケーションは、アクセストークンを更新するために再登録するのではなく、保存されたクライアント資格情報を使用する必要があります。再登録しない場合は、ユーザーが再認証する必要があります。

詳しくは、[ アクセストークンの取得 ](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントを参照してください。

+++

+++設定フェーズに関するよくある質問

### 1.設定フェーズの目的は何ですか？ {#configuration-phase-faq1}

設定フェーズの目的は、クライアントアプリケーションに、アクティブに統合されている MVPD のリストと、各 MVPD に対してAdobe Pass Authentication によって保存された設定の詳細を提供することです。

設定フェーズは、クライアントアプリケーションがユーザーに TV プロバイダーの選択を依頼する必要がある場合に、認証フェーズの前提条件の手順として機能します。

### 2.設定フェーズは必須ですか？ {#configuration-phase-faq2}

設定フェーズは必須ではありません。クライアントアプリケーションは、次のシナリオでは、このフェーズをスキップできます。

* ユーザーは既に認証済みです。
* ユーザーは、基本的またはプロモーションの TempPass 機能を通じて一時的なアクセスを提供されます。
* ユーザ認証の期限は切れているが、ユーザ体験の動機付け選択として先に選択した MVPD をクライアントアプリケーションがキャッシュし、ユーザに対してその MVPD の加入者であることを確認するように促すだけである。

### 3.設定とは何で、どれくらい有効ですか？ {#configuration-phase-faq3}

設定とは、[Glossary](./rest-api-v2-glossary.md#configuration) ドキュメントで定義されている用語です。

設定は、設定エンドポイントから取得できる MVPD のリストで構成されます。

クライアントアプリケーションは、ユーザーに MVPD の選択を求める際に、設定を使用して「ピッカー」と呼ばれる UI コンポーネントを表示できます。

ユーザーが認証フェーズを経る前に、クライアントアプリケーションが設定を更新する必要があります。

詳しくは、[ 設定の取得 ](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) ドキュメントを参照してください。

### 4. クライアントアプリケーションは、独自の MVPD リストを管理できますか。 {#configuration-phase-faq4}

クライアントアプリケーションは独自の MVPD のリストを管理できますが、リストを最新かつ正確なものにするには、Adobe Pass Authentication が提供する設定を使用することをお勧めします。

選択した MVPD が指定の [ サービスプロバイダー ](/help/authentication/enhanced-error-codes.md) とのアクティブな統合を持っていない場合、クライアントアプリケーションはAdobe Pass認証 REST API V2 から [ エラー ](./rest-api-v2-glossary.md#service-provider) を受け取ります。

### 5. クライアントアプリケーションは MVPD のリストをフィルタリングできますか。 {#configuration-phase-faq5}

クライアントアプリケーションは、独自のビジネスロジックおよび要件（以前に選択したユーザーの場所やユーザーの履歴など）に基づいてカスタムメカニズムを実装することで、設定応答で提供される MVPD のリストをフィルタリングできます。

### 6. MVPD との統合が無効になり、非アクティブとしてマークされた場合はどうなりますか。 {#configuration-phase-faq6}

MVPD との統合が無効で非アクティブとしてマークされている場合、その MVPD は、追加の設定応答で提供される MVPD のリストから削除されます。考慮すべき重要な結果が 2 つあります。

* その MVPD の認証されていないユーザーは、その MVPD を使用して認証フェーズを完了できなくなります。
* その MVPD の認証済みユーザーは、その MVPD を使用して事前承認、承認、またはログアウトの各フェーズを完了できなくなります。

選択した MVPD が指定の [ サービスプロバイダー ](/help/authentication/enhanced-error-codes.md) とのアクティブな統合を持たなくなった場合、クライアントアプリケーションはAdobe Pass認証 REST API V2 から [ エラー ](./rest-api-v2-glossary.md#service-provider) を受け取ります。

### 7. MVPD との統合が有効になり、アクティブとしてマークされた場合はどうなりますか。 {#configuration-phase-faq7}

MVPD との統合がイネーブルに戻され、アクティブとしてマークされると、その MVPD は以降の設定応答で提供される MVPD のリストに再度含まれますが、考慮すべき重要な結果は次の 2 つです。

* その MVPD の認証されていないユーザーは、その MVPD を使用して認証フェーズを再び完了できます。
* その MVPD の認証済みユーザーは、その MVPD を使用して事前認証、認証、またはログアウトの各フェーズを完了できるようになります。

### 8. MVPD との統合を有効または無効にする方法 {#configuration-phase-faq8}

この操作は、組織管理者の 1 人がAdobe Pass[TVE ダッシュボード ](./rest-api-v2-glossary.md#tve-dashboard) を使用して、またはお客様に代わってAdobe Pass認証担当者が実行できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration) ドキュメントを参照してください。

+++

+++認証フェーズに関するよくある質問

### 1.認証段階の目的は何ですか。 {#authentication-phase-faq1}

認証フェーズの目的は、クライアントアプリケーションにユーザーの ID を検証し、ユーザーメタデータ情報を取得する機能を提供することです。

認証フェーズは、クライアントアプリケーションでコンテンツを再生する必要がある場合に、事前認証フェーズまたは認証フェーズの前提条件の手順として機能します。

### 2. ユーザーが既に認証されているかどうかをクライアントアプリケーションが認識するにはどうすればよいですか？ {#authentication-phase-faq2}

クライアントアプリケーションは、ユーザーが既に認証されているかどうかを確認できる次のエンドポイントのいずれかをクエリし、プロファイル情報を返すことができます。

* [プロファイルエンドポイント API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の MVPD API のプロファイルエンドポイント](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（認証）コード API 用プロファイルエンドポイント](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3. クライアントアプリケーションは、ユーザーのメタデータ情報をどのように取得できますか？ {#authentication-phase-faq3}

クライアントアプリケーションは、プロファイル情報の一部として [ ユーザーメタデータ ](/help/authentication/user-metadata-feature.md) 情報を返すことができる次のエンドポイントのいずれかをクエリできます。

* [プロファイルエンドポイント API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の MVPD API のプロファイルエンドポイント](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（認証）コード API 用プロファイルエンドポイント](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4.認証セッションとは何で、どれくらい有効ですか。 {#authentication-phase-faq4}

認証セッションとは、[Glossary](./rest-api-v2-glossary.md#session) ドキュメントで定義されている用語です。

認証セッションには、セッション エンドポイントから取得できる、開始された認証プロセスに関する情報が格納されます。

認証セッションは、問題が発生した時点で指定された、制限された短い期間にわたって有効です。これは、ユーザーが認証プロセスを完了してからフローを再開する必要が生じるまでの時間を示します。

クライアントアプリケーションは、認証セッション応答を使用して、認証プロセスの進め方を知ることができます。 一時的なアクセスの提供、アクセスの低下、ユーザーが既に認証されている場合など、ユーザーの認証が不要な場合があることに注意してください。

詳しくは、次のドキュメントを参照してください。

* [認証セッション API の作成](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッション API の再開](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリ・アプリケーション内で実行される基本認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5.認証コードとは何ですか？また、有効期限はどのくらいですか？ {#authentication-phase-faq5}

認証コードとは、[Glossary](./rest-api-v2-glossary.md#code) ドキュメントで定義されている用語です。

認証コードは、ユーザが認証プロセスを開始する際に生成される一意の値を格納し、プロセスが完了するまで、または認証セッションが期限切れになるまで、ユーザの認証セッションを一意に識別する。

認証コードは、認証セッションの開始時に指定される限定された短い期間にわたって有効で、ユーザーが認証プロセスを完了してからフローを再開する必要が生じるまでの時間を示します。

クライアントアプリケーションは認証コードを使用して、ユーザーが認証セッションに有効期限がなかったことを考慮して、同じデバイスまたは 2 番目のデバイスで認証プロセスを完了または再開できるようにします。

詳しくは、次のドキュメントを参照してください。

* [認証セッション API の作成](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッション API の再開](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [セカンダリ・アプリケーション内で実行される基本認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6. ユーザーが有効な認証コードを入力し、認証セッションがまだ期限切れになっていないことをクライアントアプリケーションが認識するにはどうすればよいですか？ {#authentication-phase-faq6}

クライアントアプリケーションは、セカンダリ（画面）アプリケーションでユーザーによって入力された認証コードを検証できます。その際、認証コードに関連付けられた認証セッション情報を取得するリクエストを、セッションエンドポイントに送信します。

指定された認証コードが誤って入力された場合や、認証セッションが期限切れになった場合、クライアントアプリケーションは [ エラー ](/help/authentication/enhanced-error-codes.md) を受け取ります。

詳しくは、[ 認証セッションの取得 ](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) ドキュメントを参照してください。

### 7. プロファイルとは何で、どれくらい有効ですか？ {#authentication-phase-faq7}

プロファイルとは、[Glossary](./rest-api-v2-glossary.md#profile) ドキュメントで定義されている用語です。

プロファイルには、ユーザーの認証の有効性、メタデータ情報など、プロファイルエンドポイントから取得できる多くの情報が保存されます。

クライアントアプリケーションは、プロファイルを使用してユーザーの認証ステータスを把握したり、ユーザーメタデータ情報にアクセスしたり、認証に使用する方法を見つけたりできます。

詳しくは、次のドキュメントを参照してください。

* [プロファイルエンドポイント API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の MVPD API のプロファイルエンドポイント](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（認証）コード API 用プロファイルエンドポイント](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

プロファイルはクエリ時に指定された期間制限が有効で、ユーザーが有効な認証を受けた後に認証フェーズを再度進む必要が生じる時間を示します。

この制限された期間は、認証（authN） [TTL](./rest-api-v2-glossary.md#ttl) と呼ばれ、組織管理者の 1 人またはユーザーに代わってAdobe Pass認証担当者が、Adobe Pass[TVE ダッシュボード ](./rest-api-v2-glossary.md#tve-dashboard) を通じて表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) ドキュメントを参照してください。

+++

+++事前認証フェーズに関するよくある質問

### 1.事前認証フェーズの目的は何ですか。 {#preauthorization-phase-faq1}

事前認証フェーズの目的は、ユーザーがアクセスする権利を持つカタログからリソースのサブセットをクライアントアプリケーションに提示できるようにすることです。

事前認証フェーズでは、ユーザーが初めてクライアントアプリケーションを開いた場合や、新しいセクションに移動した場合のユーザーエクスペリエンスが向上します。

### 2.事前認証フェーズは必須ですか？ {#preauthorization-phase-faq2}

事前認証フェーズは必須ではありません。クライアントアプリケーションは、ユーザーの使用権限に基づいて最初にリソースのカタログをフィルタリングせずに提示する場合は、このフェーズをスキップできます。

### 3.事前認証の決定とは？ {#preauthorization-phase-faq3}

事前認証は、[ 用語集 ](./rest-api-v2-glossary.md#preauthorization) ドキュメントで定義されている用語ですが、決定用語は [ 用語集 ](./rest-api-v2-glossary.md#decision) にも記載されています。

事前認証決定は、決定の事前認証エンドポイントから取得可能な MVPD 事前認証処理照会結果に関する情報を格納する。

クライアントアプリケーションは、事前認証決定を使用して、ユーザーがアクセスできるリソースのサブセットを TV プロバイダー（情報）決定で提示できます。

詳しくは、次のドキュメントを参照してください。

* [事前承認決定 API の取得](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4.事前認証決定にメディアトークンがないのはなぜですか？ {#preauthorization-phase-faq4}

事前認証フェーズの目的と同様に、事前認証フェーズをリソースの再生に使用してはならないので、事前認証決定にメディアトークンがありません。

### 5. リソースとは何ですか？また、どのような形式がサポートされていますか？ {#preauthorization-phase-faq5}

リソースとは、[Glossary](./rest-api-v2-glossary.md#resource) ドキュメントで定義されている用語です。

リソースは、MVPD と合意された一意の識別子で、クライアントアプリケーションによってストリーミングされる可能性のあるコンテンツに関連付けられます。

リソースの一意の ID には、次の 2 つの形式があります。

* チャネル（ブランド）の一意の ID などの単純な文字列形式。
* タイトル、規制、保護者による制限のメタデータなどの追加情報を含むメディア RSS （MRSS）形式。

詳しくは、[ 保護されたリソースの識別 ](/help/authentication/identify-protected-resources.md) ドキュメントを参照してください。

### 6. クライアントアプリケーションが一度に事前認証の決定を取得できるリソースの数 {#preauthorization-phase-faq6}

クライアントアプリケーションは、MVPD によって課せられた条件により、1 回の API リクエストで、通常は 5 回まで、限られた数のリソースに対して事前認証の決定を取得できます。

この最大数のリソースは、組織の管理者の 1 人がAdobe Pass [TVE ダッシュボード ](./rest-api-v2-glossary.md#tve-dashboard) を通じて、またはお客様に代わってAdobe Pass認証担当者が MVPD に同意した後に表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) ドキュメントを参照してください。

+++

+++承認フェーズに関するよくある質問

### 1.承認フェーズの目的は何ですか？ {#authorization-phase-faq1}

承認フェーズの目的は、MVPD でユーザーの権限を検証した後、クライアントアプリケーションが要求したリソースを再生できるようにすることです。

### 2.認証フェーズは必須ですか？ {#authorization-phase-faq2}

認証フェーズは必須で、クライアントアプリケーションは、ユーザーが要求するリソースを再生する場合、このフェーズをスキップできません。これは、ストリームをリリースする前に、ユーザーが権限を持っていることを MVPD で検証する必要があるためです。

### 3.認証の決定とは何ですか？また、有効期限はどのくらいですか？ {#authorization-phase-faq3}

認証とは、[ 用語集 ](./rest-api-v2-glossary.md#authorization) のドキュメントで定義されている用語ですが、決定用語は [ 用語集 ](./rest-api-v2-glossary.md#decision) にもあります。

認証決定は、決定認証エンドポイントから取得可能な MVPD 認証処理照会結果に関する情報を格納する。

クライアントアプリケーションは、メディアトークンを含む認証決定を使用して、TV プロバイダー（権限を持つ）の決定によってユーザーがリソースストリームにアクセスできる場合に備えて、リソースストリームを再生できます。

詳しくは、次のドキュメントを参照してください。

* [承認決定 API の取得](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

認証決定は発行時に指定された限られた短い期間のみ有効で、MVPD に対する再度の問い合わせが必要になるまでの、Adobe Pass認証によってキャッシュされる時間を示します。

この制限付き期間は、認証（authZ） [TTL](./rest-api-v2-glossary.md#ttl) と呼ばれ、組織管理者の 1 人またはAdobe Pass認証担当者が ](./rest-api-v2-glossary.md#tve-dashboard) 客様に代わってAdobe Pass[TVE ダッシュボード）を通じて表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) ドキュメントを参照してください。

### 4. メディアトークンとは何で、どれくらい有効ですか？ {#authorization-phase-faq4}

メディアトークンは、[Glossary](./rest-api-v2-glossary.md#media-token) ドキュメントで定義されている用語です。

メディアトークンは、決定を承認エンドポイントから取得できる、クリアテキストで送信された署名済み文字列で構成されます。

詳しくは、[ メディアトークンベリファイアの統合 ](/help/authentication/media-token-verifier-int.md) ドキュメントを参照してください。

メディアトークンは、問題の時点で指定された限られた短い期間にわたって有効です。これは、決定の承認エンドポイントに対する再度のクエリが必要になるまでにクライアントアプリケーションによって使用される必要がある時間を示します。

クライアントアプリケーションは、TV プロバイダー（権限を持つ）の決定によってユーザーがアクセスできる場合に、メディアトークンを使用してリソースストリームを再生できます。

詳しくは、次のドキュメントを参照してください。

* [承認決定 API の取得](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5. リソースとは何ですか？また、どのような形式がサポートされていますか？ {#authorization-phase-faq5}

リソースとは、[Glossary](./rest-api-v2-glossary.md#resource) ドキュメントで定義されている用語です。

リソースは、MVPD と合意された一意の識別子で、クライアントアプリケーションによってストリーミングされる可能性のあるコンテンツに関連付けられます。

リソースの一意の ID には、次の 2 つの形式があります。

* チャネル（ブランド）の一意の ID などの単純な文字列形式。
* タイトル、規制、保護者による制限のメタデータなどの追加情報を含むメディア RSS （MRSS）形式。

詳しくは、[ 保護されたリソースの識別 ](/help/authentication/identify-protected-resources.md) ドキュメントを参照してください。

### 6. クライアントアプリケーションが一度に認証決定を取得できるリソースの数 {#authorization-phase-faq6}

クライアントアプリケーションは、MVPD によって課せられる条件により、1 回の API リクエストで限られた数のリソースに対する承認決定を、通常は 1 つまで取得できます。

+++

+++ログアウトフェーズに関するよくある質問

### 1. ログアウトフェーズの目的は何ですか？ {#logout-phase-faq1}

ログアウトフェーズの目的は、クライアントアプリケーションが、ユーザーのリクエストに応じてAdobe Pass Authentication 内でユーザーの認証済みプロファイルを終了できるようにすることです。

+++

+++ヘッダーの FAQ

### 1.認証ヘッダーの値を計算する方法 {#headers-faq1}

[Authorization](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) リクエストヘッダーには、Adobe Passで保護された API にアクセスするためにクライアントアプリケーションで必要な `Bearer` アクセストークンが含まれています。

Authorization ヘッダー値は、登録段階でAdobe Pass Authentication から取得する必要があります。

詳しくは、次のドキュメントを参照してください。

* [Dynamic Client Registration の概要](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [クライアント資格情報 API の取得](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークン API の取得](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動的なクライアント登録フロー](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

クライアントアプリケーションが REST API V1 から REST API V2 に移行している場合、クライアントアプリケーションは引き続き同じメソッドを使用して、以前と同じように `Bearer` アクセストークンを取得できます。

### 2. AP-Device-Identifier ヘッダーの値を計算する方法 {#headers-faq2}

[AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) 要求ヘッダーには、クライアント アプリケーションによって作成されたストリーミング デバイスの識別子が含まれています。

[AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) ヘッダーのドキュメントには、様々なプラットフォームの値を計算する方法の例がいくつか記載されていますが、クライアントアプリケーションは、独自のビジネスロジックと要件に基づいて異なる方法を使用するように選択できます。

クライアントアプリケーションが REST API V1 から REST API V2 に移行する場合、クライアントアプリケーションは引き続き同じ方法を使用してデバイス識別子を計算できます。

### 3. X-Device-Info ヘッダーの値を計算する方法 {#headers-faq3}

[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) リクエストヘッダーには、実際のストリーミングデバイスに関連するクライアント情報（デバイス、接続、アプリケーション）が含まれます。

[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ヘッダーのドキュメントには、様々なプラットフォームの値を計算する方法の例がいくつか記載されていますが、クライアントアプリケーションは、独自のビジネスロジックと要件に基づいて異なる方法を使用するように選択できます。

クライアントアプリケーションが REST API V1 から REST API V2 に移行する場合、クライアントアプリケーションは引き続き同じ方法を使用してデバイス情報を計算できます。

+++

## 移行に関する FAQ {#migration-faqs}

既存のアプリケーションを REST API V2 に移行する必要があるアプリケーションで作業している場合は、この節を続行してください。

+++一般的な移行の FAQ

### 1. REST API V2 に移行された新しいクライアントアプリケーションを、すべてのユーザーに一度にロールアウトする必要がありますか？ {#migration-faq1}

いいえ。

クライアントアプリケーションは、REST API V2 を統合した新しいバージョンをすべてのユーザーに同時にロールアウトする必要はありません。

Adobe Pass認証では、2025 年末まで、REST API V1 または SDK を統合する古いクライアントアプリケーションバージョンを引き続きサポートします。

### 2. REST API V2 に移行された新しいクライアントアプリケーションを、すべての API とフローにわたって一度にロールアウトする必要がありますか？ {#migration-faq2}

はい。

クライアントアプリケーションは、REST API V2 を統合した新しいバージョンを、すべてのAdobe Pass認証 API およびフローに同時にロールアウトする必要があります。

「2 画面目の認証」フローの場合、クライアントアプリケーションは、「プライマリ」と「セカンダリ [ の両方のアプリケーションに対して REST API V2 を統合した新しいバージョンを同時にロ [ ルアウトす ](./rest-api-v2-glossary.md#primary-application) 必要が ](./rest-api-v2-glossary.md#secondary-application) ります。

Adobe Pass認証では、API とフローの間で REST API V2 と REST API V1/SDK の両方を統合する「ハイブリッド」実装をサポートしていません。

### 3. REST API V2 に移行された新しいクライアントアプリケーションに更新する際、ユーザー認証は保持されますか。 {#migration-faq3}

いいえ。

REST API V1 または SDK を統合する古いクライアントアプリケーションバージョンで取得したユーザー認証は保持されません。

したがって、ユーザーは、REST API V2 に移行された新しいクライアントアプリケーション内で再認証する必要があります。

### 4. クライアントアプリケーションは既存の登録済みアプリケーション （ソフトウェア文）を使用できますか？ {#migration-faq4}

クライアントアプリケーションは、既存の登録アプリケーション（ソフトウェアステートメント）を再利用できないので、REST API V2 を使用する専用の新しい登録アプリケーション（ソフトウェアステートメント）を生成し、ダウンロードする必要があります。

この操作は、組織管理者の 1 人がAdobe Pass[TVE ダッシュボード ](./rest-api-v2-glossary.md#tve-dashboard) を使用して、またはお客様に代わってAdobe Pass認証担当者が実行できます。

詳しくは、[TVE ダッシュボードチャネルユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications) または [TVE ダッシュボードプログラマーユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications) ドキュメントを参照してください。

この間、新しい登録アプリケーション（ソフトウェアステートメント）で REST API V2 を使用できるように、Adobe Pass認証担当者に依頼する必要があります。その後、Adobe Pass[TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) が更新されて、この処理を自己管理できるようになる予定です。

REST API V2 を使用するクライアントアプリケーションで使用される登録済みアプリケーション（ソフトウェアステートメント）を区別するために、登録済みアプリケーション名に「RESTV2」などの特定のサフィックスを追加する必要があります。

### 5. クライアントアプリケーションで既存のカスタムスキームを使用できますか？ {#migration-faq5}

クライアントアプリケーションは、Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard) を通じて生成された既存のカスタムスキームを再利用できます。

詳しくは、[TVE ダッシュボードチャネルユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes) または [TVE ダッシュボードプログラマーユーザーガイド ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) ドキュメントを参照してください。

### 6.拡張エラーコードは、REST API V2 でデフォルトで有効になっていますか。 {#migration-faq6}

はい。

REST API V2 を統合しているクライアントアプリケーションでは、デフォルトで有効になっている拡張エラーコード機能のメリットが得られます。

詳しくは、[ 拡張エラーコード ](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) ドキュメントを参照してください。

+++

### REST API V1 から REST API V2 への移行 {#migration-rest-api-v1-to-rest-api-v2-faqs}

REST API V1 から REST API V2 に移行する必要があるアプリケーションを操作している場合は、このサブセクションを続行します。

+++登録フェーズに関するよくある質問

#### 1.登録フェーズに必要な高レベルの API 移行は何ですか。 {#registration-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、登録フェーズに関して大きな変更はありません。

クライアントアプリケーションは、引き続き同じフローを使用して、[ 動的クライアント登録（DCR） ](./rest-api-v2-glossary.md#dcr) プロセスを通じてAdobe Pass認証に登録し、アクセストークンを取得できます。

詳しくは、次のドキュメントを参照してください。

* [Dynamic Client Registration の概要](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [クライアント資格情報 API の取得](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [アクセストークン API の取得](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動的なクライアント登録フロー](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

+++設定フェーズに関するよくある質問

#### 1.設定フェーズに必要な、大まかな API 移行は何ですか？ {#configuration-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++認証フェーズに関するよくある質問

#### 1.認証フェーズに必要な、高レベルの API 移行は何ですか。 {#authentication-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コードを確認（認証コード） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2 番目の画面で認証を再開（MVPD） （アプリケーション） | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証の開始 | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [GET <br/> /api/v1/checkauthn （最初の画面） ](/help/authentication/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn （2 番目の画面） ](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザー認証トークンの取得（プロファイル） | [GET <br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++事前認証フェーズに関するよくある質問

#### 1.事前認証フェーズに必要な高レベルの API 移行は何ですか。 {#preauthorization-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [GET <br/> /api/v1/preauthorize （最初の画面） ](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize （2 番目の画面） ](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++承認フェーズに関するよくある質問

#### 1.認証フェーズに必要な高レベルの API 移行は何ですか。 {#authorization-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [GET <br/> /api/v1/authorize](/help/authentication/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 認証トークンの取得（認証決定） | [GET <br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 短い認証トークンの取得（メディアトークン） | [GET <br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++ログアウトフェーズに関するよくある質問

#### 1. ログアウトフェーズに必要な、大まかな API 移行は何ですか。 {#logout-phase-v1-to-v2-faq1}

REST API V1 から REST API V2 への移行では、大まかな変更が必要で、次の表に示します。

| 対象範囲 | REST API V1 | REST API V2 | 所見 |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ログアウトの開始 | [GET <br/> /api/v1/logout](/help/authentication/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### SDK から REST API V2 への移行 {#migrate-sdk-to-rest-api-v2}

SDK から REST API V2 に移行する必要があるアプリケーションを操作している場合は、このサブセクションの手順を続行します。

+++登録フェーズに関するよくある質問

#### 1.登録フェーズに必要な高レベルの API 移行は何ですか。 {#registration-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

##### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 動的クライアント登録（DCR）の完了 | コンストラクタへのソフトウェア文の提供 | [POST <br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[Dynamic Client Registration の概要 ](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[ 動的なクライアント登録フロー ](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++設定フェーズに関するよくある質問

#### 1.設定フェーズに必要な、大まかな API 移行は何ですか？ {#configuration-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

##### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| アクティブな統合を持つ MVPD のリストの取得 | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++認証フェーズに関するよくある質問

#### 1.認証フェーズに必要な、高レベルの API 移行は何ですか。 {#authentication-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

##### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コードを確認（認証コード） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2 番目の画面で認証を再開（MVPD） （アプリケーション） | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 登録コードの取得（認証コード） | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 登録コードを確認（認証コード） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 2 番目の画面で認証を再開（MVPD） （アプリケーション） | [GET <br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| （MVPD）認証の開始 | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| ユーザー認証ステータスの確認 | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| ユーザーメタデータ情報の取得 | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | 次のいずれかを使用します。<br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | クライアントアプリケーションは、次の API の応答を複数の目的で一度に使用できます。<br/> <ul><li>ユーザー認証ステータスの確認</li><li>ユーザープロファイルを取得</li><li>ユーザーメタデータ情報の取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[ セカンダリアプリケーション内で実行される基本プロファイルフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++事前認証フェーズに関するよくある質問

#### 1.事前認証フェーズに必要な高レベルの API 移行は何ですか。 {#preauthorization-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

##### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 対象範囲 | SDK | REST API V2 | 所見 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 事前承認されたリソースの取得（事前承認決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本的な事前認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++承認フェーズに関するよくある質問

#### 1.認証フェーズに必要な高レベルの API 移行は何ですか。 {#authorization-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

##### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 短い認証トークンの取得（メディアトークン） | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | クライアントアプリケーションは、この API の応答を一度に複数の目的に使用できます。<br/> <ul><li>（MVPD）認証の開始</li><li>認証決定の取得</li><li>短いメディアトークンの取得</li></ul> <br/> 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本認証フロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++ログアウトフェーズに関するよくある質問

#### 1. ログアウトフェーズに必要な、大まかな API 移行は何ですか。 {#logout-phase-sdk-to-v2-faq1}

SDK から REST API V2 への移行では、大まかな変更が必要で、それを次の表に示します。

##### AccessEnabler JavaScript SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 対象範囲 | SDK | REST API V2 | 所見 |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| ログアウトの開始 | [AccessEnabler.logout](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 詳しくは、次のドキュメントを参照してください。<br/> <ul><li>[ プライマリアプリケーション内で実行される基本ログアウトフロー ](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
