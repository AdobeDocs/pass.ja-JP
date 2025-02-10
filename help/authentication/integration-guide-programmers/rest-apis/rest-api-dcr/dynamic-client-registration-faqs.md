---
title: Dynamic Client Registration （DCR）に関する FAQ
description: Dynamic Client Registration （DCR）に関する FAQ
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# Dynamic Client Registration （DCR）に関する FAQ {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass Authentication Dynamic Client Registration （DCR）の導入に関するよくある質問に対して、概要の大まかな回答を示します。

動的クライアント登録（DCR）全体について詳しくは、[ 動的クライアント登録の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## 一般的な FAQ {#general-faqs}

動的クライアント登録（DCR）を統合する必要があるアプリケーションを扱う場合、それが新しいアプリケーションであるか、以前のいずれかのメカニズムから移行した既存のアプリケーションであるかを問わず、この節から開始します。

>[!MORELIKETHIS]
>
> * [REST API v2 に関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### REST API V2 アクセスの FAQ {#rest-api-v2-access-faqs}

+++REST API V2 アクセスに関する FAQ

#### 1.登録段階の目的は何ですか？ {#rest-api-v2-access-faq1}

登録フェーズの目的は、[Dynamic Client Registration （DCR） ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) プロセスを通じて、Adobe Pass Authentication に対してクライアントアプリケーションを登録することです。

動的クライアント登録（DCR）プロセスでは、クライアントアプリケーションがクライアント資格情報のペアを取得し、登録フェーズの最終目標としてアクセストークンを取得する必要があります。

詳しくは、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

#### 2.登録フェーズは必須ですか？ {#rest-api-v2-access-faq2}

登録フェーズは必須ですが、クライアント資格情報とアクセストークンのペアがキャッシュされていて、それらが引き続き有効な場合、クライアントアプリケーションはこのフェーズをスキップできます。

#### 3. ソフトウェアのステートメントとは何ですか？また、有効期限はどのくらいですか？ {#rest-api-v2-access-faq3}

ソフトウェア ステートメントは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement) ドキュメントで定義されている用語です。

このソフトウェア ステートメントは、組織管理者の 1 人が、またはユーザーに代わってAdobe Pass認証担当者がAdobe Pass[TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) から生成およびダウンロードできる JSON web トークン（JWT）で構成されます。

このソフトウェアのステートメントは無期限に有効ですが、Adobe Pass認証担当者にいつでも取り消しを依頼することもできます。

クライアントアプリケーションは、ソフトウェア文を保存し、クライアント資格情報を取得する必要がある場合に使用する必要があります。

詳しくは、[ 動的クライアント登録の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

#### 4. ソフトウェア・ステートメントを生成およびダウンロードする方法は？ {#rest-api-v2-access-faq4}

この操作は、組織管理者の 1 人がAdobe Pass[TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を使用して、またはお客様に代わってAdobe Pass認証担当者が実行できます。

詳しくは、[TVE ダッシュボードチャネルユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) または [TVE ダッシュボードプログラマーユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) ドキュメントを参照してください。

#### 5. ソフトウェアのステートメントが取り消されるとどうなりますか？ {#rest-api-v2-access-faq5}

ソフトウェアのステートメントが取り消された場合、考慮すべき重要な結果が 1 つあります。

* 失効したソフトウェア文を使用するクライアントアプリケーションは [ 使用権限 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement) フローを実行できなくなるので、ユーザーによるコンテンツの再生がブロックされます。

#### 6. クライアント資格情報とは何ですか？また、どれくらい有効ですか？ {#rest-api-v2-access-faq6}

クライアント資格情報とは、[ 用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials) ドキュメントで定義されている用語です。

クライアント資格情報は、クライアント登録エンドポイントから取得できるクライアント識別子とクライアント秘密鍵のペアで構成されます。

クライアント資格情報は無制限の期間に対して有効です。

クライアントアプリケーションは、アクセストークンを取得する必要がある場合、クライアント資格情報を保存して無期限に使用する必要があります。

詳しくは、[ クライアント資格情報の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) ドキュメントを参照してください。

#### 7. クライアント資格情報の管理方法 {#rest-api-v2-access-faq7}

Adobe Pass Authentication を使用してクライアントとサーバーの両方を統合する場合は、クライアントアプリケーションでユーザーアプリケーションインスタンスごとに一意のクライアント資格情報のペアを管理することをお勧めします。

#### 8. クライアントアプリケーションは、クライアント資格情報を永続的なストレージにキャッシュする必要がありますか？ {#rest-api-v2-access-faq8}

クライアントアプリケーションは、アクセストークンを取得する必要がある場合、クライアント資格情報を保存して無期限に使用する必要があります。

#### 9. キャッシュされたクライアント資格情報が失われた場合はどうなりますか？ {#rest-api-v2-access-faq9}

キャッシュされたクライアント資格情報が失われた場合、考慮すべき重要な結果は次の 3 つです。

* クライアントアプリケーションは、新しいクライアント資格情報のペアを取得する必要があります。
* クライアントアプリケーションは、新しいクライアント資格情報のペアを使用して、新しいアクセストークンを取得する必要があります。
* クライアントアプリケーションは、以前に取得した認証済みプロファイルへのアクセスを失うので、クライアントアプリケーションはユーザーに再認証を求める必要があります。

#### 10. アクセストークンとは何ですか？また、どれくらい有効ですか？ {#rest-api-v2-access-faq10}

アクセストークンとは、[Glossary](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token) ドキュメントで定義されている用語です。

アクセストークンは、クライアントトークンエンドポイントから取得できる [ ベアラートークン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) で構成されています。

アクセストークンは、発行時に指定された限られた短い期間のみ有効です。

クライアントアプリケーションは、REST API V2 をターゲット設定する際に、アクセストークンを保存し、有効期限が切れるまで使用する必要があります。

クライアントアプリケーションは、現在のアクセストークンの有効期限が切れる前に、承認されていないリクエストを防ぐために、新しいアクセストークンを取得する必要があります。

詳しくは、[ アクセストークンの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントを参照してください。

#### 11. クライアントアプリケーションは、アクセストークンを永続的なストレージにキャッシュする必要がありますか？ {#rest-api-v2-access-faq11}

クライアントアプリケーションは、有効期限が切れるまでアクセストークンを保存して使用し、その後、破棄して新しいアクセストークンを取得する必要があります。

#### 12. クライアントアプリケーションは、アクセストークンをどのように更新できますか。 {#rest-api-v2-access-faq12}

クライアントアプリケーションは、新しいアクセストークンを取得するのと同じ方法で、キャッシュされたクライアント資格情報を使用してアクセストークンを更新する必要があります。

クライアントアプリケーションは、アクセストークンを更新するために再登録するのではなく、保存されたクライアント資格情報を使用する必要があります。再登録しない場合は、ユーザーが再認証する必要があります。

詳しくは、[ アクセストークンの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントを参照してください。

+++

## 移行に関する FAQ {#migration-faqs}

Dynamic Client Registration （DCR）を使用するために既存のアプリケーションを移行する必要があるアプリケーションで作業している場合は、この節を続行します。

>[!MORELIKETHIS]
>
> * [REST API v2 に関する FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### REST API V2 移行に関する FAQ {#rest-api-v2-migration-faqs}

+++REST API V2 移行に関する FAQ

#### 1. クライアントアプリケーションは既存の登録済みアプリケーション （ソフトウェア文）を再利用できますか？ {#rest-api-v2-migration-faq1}

クライアントアプリケーションは、既存の登録アプリケーション（ソフトウェアステートメント）を再利用できないので、REST API V2 を使用する専用の新しい登録アプリケーション（ソフトウェアステートメント）を生成し、ダウンロードする必要があります。

この操作は、組織管理者の 1 人がAdobe Pass[TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を使用して、またはお客様に代わってAdobe Pass認証担当者が実行できます。

詳しくは、[TVE ダッシュボードチャネルユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) または [TVE ダッシュボードプログラマーユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications) ドキュメントを参照してください。

この間、新しい登録アプリケーション（ソフトウェアステートメント）で REST API V2 を使用できるように、Adobe Pass認証担当者に依頼する必要があります。その後、Adobe Pass[TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) が更新されて、この処理を自己管理できるようになる予定です。

REST API V2 を使用するクライアントアプリケーションで使用される登録済みアプリケーション（ソフトウェアステートメント）を区別するために、登録済みアプリケーション名に「RESTV2」などの特定のサフィックスを追加する必要があります。

#### 2. クライアントアプリケーションは既存のカスタムスキームを再利用できますか？ {#rest-api-v2-migration-faq2}

クライアントアプリケーションは、Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を通じて生成された既存のカスタムスキームを再利用できます。

詳しくは、[TVE ダッシュボードチャネルユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) または [TVE ダッシュボードプログラマーユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes) ドキュメントを参照してください。

+++
