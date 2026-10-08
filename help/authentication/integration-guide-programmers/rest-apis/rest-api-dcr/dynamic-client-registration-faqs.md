---
title: 動的クライアント登録（DCR）に関するFAQ
description: 動的クライアント登録（DCR）に関するFAQ
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1147'
ht-degree: 1%
---
# 動的クライアント登録（DCR）に関するFAQ {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このドキュメントでは、Adobe Pass Authentication Dynamic Client Registration （DCR）の導入に関するよくある質問に対する概要を説明します。

動的クライアント登録（DCR）全体の詳細については、[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)のドキュメントを参照してください。

## 一般的なFAQ {#general-faqs}

Dynamic Client Registration （DCR）を統合する必要があるアプリケーションを使用している場合は、新しいアプリケーションであるか、以前のメカニズムから移行する既存のアプリケーションであるかを問わず、この節から始めてください。

>[!MORELIKETHIS]
>
> * [REST API v2に関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### REST API V2 アクセスに関するFAQ {#rest-api-v2-access-faqs}

+++REST API V2 アクセスに関するFAQ

#### &#x200B;1. 登録段階の目的は何か？ {#rest-api-v2-access-faq1}

登録フェーズの目的は、[Dynamic Client Registration （DCR） ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) プロセスを通じて、Adobe Pass Authenticationに対してクライアントアプリケーションを登録することです。

動的クライアント登録（DCR）プロセスでは、クライアントアプリケーションが登録フェーズの最終目標として、1組のクライアント資格情報を取得し、アクセストークンを取得する必要があります。

詳しくは、[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)のドキュメントを参照してください。

#### &#x200B;2. 登録フェーズは必須ですか？ {#rest-api-v2-access-faq2}

登録フェーズは必須ですが、クライアントアプリケーションがキャッシュされたクライアント資格情報のペアと有効なアクセストークンを持っている場合、このフェーズをスキップできます。

#### &#x200B;3. ソフトウェアステートメントとは何か？どれくらいの期間有効か？ {#rest-api-v2-access-faq3}

ソフトウェア文は、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement)のドキュメントで定義されている用語です。

ソフトウェアステートメントは、Adobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)から生成およびダウンロードできるJSON Web トークン（JWT）で構成されます。このトークンは、組織管理者またはAdobe Pass認証担当者が代わりに処理します。

ソフトウェアステートメントは無制限の期間で有効ですが、いつでもAdobe Pass認証担当者に取り消しを依頼することができます。

クライアントアプリケーションは、ソフトウェアステートメントを保存し、クライアント資格情報を取得する必要がある場合にそれを使用する必要があります。

詳しくは、[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

#### &#x200B;4. ソフトウェアステートメントを生成してダウンロードするには？ {#rest-api-v2-access-faq4}

この操作は、Adobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて、組織管理者の1人またはAdobe Pass認証担当者が代わりに実行します。

詳しくは、[TVE ダッシュボード チャネル ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)または[TVE ダッシュボード プログラマーユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)のドキュメントを参照してください。

#### &#x200B;5. ソフトウェアステートメントが失効した場合はどうなりますか？ {#rest-api-v2-access-faq5}

ソフトウェアステートメントが取り消された場合、考慮すべき重要な結果が1つあります。

* 失効したソフトウェアステートメントを使用するクライアントアプリケーションは、[使用権限](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement) フローを実行できなくなります。つまり、ユーザーはコンテンツの再生をブロックされます。

#### &#x200B;6. クライアントの資格情報とは何か？また、その有効期限はどれくらいか？ {#rest-api-v2-access-faq6}

クライアント資格情報は、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials)のドキュメントで定義されている用語です。

クライアント資格情報は、クライアント識別子とクライアント秘密鍵のペアで構成され、クライアント登録エンドポイントから取得できます。

クライアントの資格情報は、無制限の期間に有効です。

クライアントアプリケーションは、クライアントの資格情報を保存し、アクセストークンを取得する必要がある場合に無期限に使用する必要があります。

詳しくは、[ クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) ドキュメントを参照してください。

#### &#x200B;7. クライアント資格情報の管理方法 {#rest-api-v2-access-faq7}

Adobe Pass Authenticationとクライアント間およびサーバー間の両方の統合が発生した場合に備えて、各ユーザーアプリケーションインスタンスの一意のクライアント資格情報のペアを管理することをお勧めします。

#### &#x200B;8. クライアントアプリケーションは、永続的なストレージにクライアントの資格情報をキャッシュしますか？ {#rest-api-v2-access-faq8}

クライアントアプリケーションは、クライアントの資格情報を保存し、アクセストークンを取得する必要がある場合に無期限に使用する必要があります。

#### &#x200B;9. キャッシュされたクライアント資格情報が失われた場合はどうなりますか？ {#rest-api-v2-access-faq9}

キャッシュされたクライアント資格情報が失われた場合、考慮すべき3つの重要な結果があります。

* クライアントアプリケーションは、新しいペアのクライアント資格情報を取得する必要があります。
* クライアントアプリケーションは、新しいペアのクライアント資格情報を使用して、新しいアクセストークンを取得する必要があります。
* クライアントアプリケーションは、以前に取得した認証済みプロファイルにアクセスできなくなります。そのため、ユーザーに再認証を依頼する必要があります。

#### &#x200B;10. アクセストークンとは何か、どのくらいの期間有効か？ {#rest-api-v2-access-faq10}

アクセストークンは、[用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token)のドキュメントで定義されている用語です。

アクセストークンは、クライアントトークンエンドポイントから取得できる[ ベアラートークン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)で構成されます。

アクセストークンは、発行の時点で指定された期間限定および短期間で有効です。

クライアントアプリケーションは、アクセストークンを保存し、REST API V2をターゲットする際に有効期限が切れるまで使用する必要があります。

クライアントアプリケーションは、不正な要求を防ぐために、現在のアクセストークンが期限切れになる前に新しいアクセストークンを取得する必要があります。

詳しくは、[ アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントを参照してください。

#### &#x200B;11. クライアントアプリケーションは、アクセストークンを永続的なストレージにキャッシュする必要がありますか？ {#rest-api-v2-access-faq11}

クライアントアプリケーションは、有効期限が切れるまでアクセストークンを保存して使用し、それを破棄して新しいトークンを取得する必要があります。

#### &#x200B;12. クライアントアプリケーションはどのようにアクセストークンを更新できますか？ {#rest-api-v2-access-faq12}

クライアントアプリケーションは、新しいアクセストークンの取得と同じ方法でアクセストークンを更新する必要がありますが、キャッシュされたクライアント資格情報を使用する必要があります。

クライアントアプリケーションは、アクセストークンを更新するために再登録しないでください。代わりに、保存されたクライアント資格情報を使用する必要があります。そうしないと、ユーザーは再認証する必要があります。

詳しくは、[ アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントを参照してください。

+++

## 移行に関するFAQ {#migration-faqs}

Dynamic Client Registration （DCR）を使用するために既存のアプリケーションを移行する必要があるアプリケーションを使用している場合は、この節を続行します。

>[!MORELIKETHIS]
>
> * [REST API v2に関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### REST API V2移行に関するFAQ {#rest-api-v2-migration-faqs}

+++REST API V2移行に関するFAQ

#### &#x200B;1. クライアントアプリケーションは、既存の登録アプリケーション（ソフトウェアステートメント）を再利用できますか？ {#rest-api-v2-migration-faq1}

クライアントアプリケーションは、既存の登録アプリケーション（ソフトウェアステートメント）を再利用できないため、REST API V2を使用するための新しい登録アプリケーション（ソフトウェアステートメント）を生成してダウンロードする必要があります。

この操作は、Adobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて、組織管理者の1人またはAdobe Pass認証担当者が代わりに実行します。

詳しくは、[TVE ダッシュボード チャネル ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)または[TVE ダッシュボード プログラマーユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)のドキュメントを参照してください。

現時点では、新しく登録されたアプリケーション（ソフトウェアステートメント）に対してREST API V2の使用を有効にするようにAdobe Pass認証担当者に依頼する必要があります。この操作を自動管理できるようにAdobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)が更新されます。

REST API V2を使用するクライアントアプリケーションで使用される登録アプリケーション（ソフトウェアステートメント）を区別するには、「RESTV2」など、登録アプリケーション名に特定のサフィックスを追加する必要があります。

#### &#x200B;2. クライアントアプリケーションは、既存のカスタムスキームを再利用できますか？ {#rest-api-v2-migration-faq2}

クライアントアプリケーションは、Adobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて生成された既存のカスタムスキームを再利用できます。

詳しくは、[TVE ダッシュボード チャネル ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes)または[TVE ダッシュボード プログラマーユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes)のドキュメントを参照してください。

+++
