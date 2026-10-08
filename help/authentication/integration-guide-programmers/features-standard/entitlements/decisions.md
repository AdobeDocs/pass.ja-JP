---
title: 決定
description: 決定
exl-id: 1efd70af-8c1d-43c4-87fc-14488d42b23d
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1014'
ht-degree: 0%
---
# 決定 {#decisions}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

決定は、ユーザーのAdobe Pass認証または事前認証の問い合わせに基づいてMVPD認証[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)によって生成され、[保護されたコンテンツ &#x200B;](#protected-resources)へのアクセスが許可されるか拒否されるかを判断します。

呼び出されるAPIに応じて、2種類の決定が提供されます。

* 情報提供の決定である[事前認証の決定](#preauthorization-decisions)。
* 権限のある決定である[承認決定](#authorization-decisions)。

## 事前認証の決定 {#preauthorization-decisions}

事前認証の決定は、クライアントアプリケーションに対して、MVPDが[保護されたリソース &#x200B;](#protected-resources)へのユーザーのアクセスを許可するか拒否するかを知らせるための有益な決定です。

事前認証（プリフライト認証）の目的は、ユーザーが表示できる可能性のあるコンテンツに関する正確な情報をアプリケーションが表示できるようにすることです。 これは、アクセスステータスを反映するために、ロックされたアイコンやロック解除されたアイコンなどのインジケーターでユーザーインターフェイスを強化することで実現されます。

>[!IMPORTANT]
>
> 事前認証の決定は、[認証の決定](#authorization-decisions)の目的であるため、リソースを再生する権限のある方法で使用してはなりません。

事前認証APIの使用は必須ではありません。クライアントアプリケーションがフィルタリングなしでリソースカタログを表示する場合は、これをスキップできます。

クライアントアプリケーションがこの機能を使用する場合は、事前認証の決定は、API リクエストごとに限られた数（通常は最大5）のリソースに対してのみ取得できることに注意してください。

>[!IMPORTANT]
> 
> リソースの最大数は、MVPDおよびAdobe Pass Authenticationの担当者と契約を締結した後にのみ増加できます。 同意が得られたら、組織内の管理者またはユーザーの代理でAdobe Pass認証担当者がAdobe Pass TVE ダッシュボードを使用して、変更を実装できます。
> 
> 詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)のドキュメントを参照してください。

MVPDは、パフォーマンスに対する明確な意味と、単一のAPI リクエストで処理できるリソースの最大数を持つ、様々なメカニズムを通じて事前認証をサポートする場合があります。

事前認証をサポートする既存の仕組みについて詳しくは、[MVPD Preflight Authorization](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md)のドキュメントを参照してください。

>[!IMPORTANT]
>
> 完全なプリフライト認証サポートがないMVPDの場合、パフォーマンスの問題や応答時間の低下につながる可能性があるため、事前認証の使用について、MVPDおよびAdobe Pass認証担当者と事前に合意する必要があります。

## 認証の決定 {#authorization-decisions}

承認決定は、クライアントアプリケーションがMVPDの決定に準拠し、[保護されたリソース &#x200B;](#protected-resources)へのユーザーのアクセスを許可または拒否することを許可する権限のある決定です。

認証の目的は、MVPDでの使用権限検証およびAdobe Pass Authenticationからのメディアトークンの受信に従って、ユーザーが要求したリソースをアプリケーションが再生できるようにすることです。

>[!IMPORTANT]
> 
> Adobe Pass Authenticationでは、ビデオストリームを開始する前に安全なアクセスを確保しながら、認証決定に含まれるメディアトークンを検証するためにMedia Token Verifier ライブラリを使用することをお勧めします。
> 
> 詳しくは、[&#x200B; メディアトークン &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)のドキュメントを参照してください。

認証APIの使用は必須です。クライアントアプリケーションは、ユーザーがリクエストしたリソースを再生する場合、このフェーズをスキップできません。ユーザーがストリームをリリースする前に、MVPDで権限があることを確認する必要があるからです。

認証の決定は、API リクエストごとに限られた数のリソース（通常は1）に対してのみ取得できることに注意してください。

>[!IMPORTANT]
>
> リソースの最大数は、MVPDおよびAdobe Pass Authenticationの担当者と契約を締結した後にのみ増加できます。

## Authorization Time-to-Live （TTL）管理 {#authorization-ttl-management}

Authorization Time-to-Live （TTL）は、リソースが再認証を必要とする前に許可された状態を維持する期間を定義します。 この期間は限られており、MVPDの担当者と合意する必要があります。 TTL値は、次の要素によって異なります。

* プラットフォームカテゴリ（例：デスクトップ、モバイル、TV接続デバイス）
* 特定のプラットフォーム（例：iOS、Android、tvOS、Roku、FireTV）

認証（authZ） TTLは、Adobe Pass [TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて、組織管理者の1人またはAdobe Pass認証担当者が代理で表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)のドキュメントを参照してください。

## 保護されたリソース {#protected-resources}

保護されたリソースとは、MVPDと参加プログラマー間の合意によって定義された一意の値によって識別される、ストリーミング可能なコンテンツを指します。

保護されたリソースは階層的なツリー構造に従い、各レベルでコンテンツの認証をより詳細に行うことができます。

* ネットワーク
  * チャネル
    * 表示
      * エピソード
        * アセット

>[!IMPORTANT]
>
> 事前認証（プリフライト認証）は、単純な文字列またはMRSS形式の識別子を使用するチャネルレベルのリソースに焦点を当てています。
> 
> 事前認証の場合、`CDATA` セクションを含む識別子を持つリソースは、主にMRSSで定義されたアセットレベルのリソースに使用されるため、使用することはお勧めしません。

### リソース識別子 {#resource-identifier}

リソースの一意のIDには、次の2つの形式を使用できます。

* チャネル（ブランド）の一意の識別子などの簡単な文字列形式。
* タイトル、評価、ペアレンタルコントロールのメタデータなどの追加情報を含むMedia RSS （MRSS）形式。

「REF30」（チャネルを表していると想定）などの単純なリソース識別子の場合、次のようにRSS リソース識別子に変換できます。

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

より複雑なリソース識別子の場合、RSS リソース識別子には、次のような追加のレーティング情報を含めることができます。

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

一意のIDは主にAdobe Pass認証に対して不透明ですが、MVPDの機能と要件に基づいてトランスフォーマが適用される場合があります。 MVPDがリソース IDを認識または解析できない場合、Adobe Pass Authenticationにエラーを返し、その後[拡張エラーコード &#x200B;](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)を使用してクライアントアプリケーションにエラーをリレーします。

## REST API V2 {#rest-api-v2}

事前認証の決定は、次のAPIを使用して取得できます。

* [特定のmvpdを使用して事前承認決定を取得する](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

認証の決定は、次のAPIを使用して取得できます。

* [特定のmvpdを使用して認証の決定を取得する](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

事前認証と認証の決定の構造については、上記のAPIの&#x200B;**応答**&#x200B;および&#x200B;**サンプル**&#x200B;のセクションを参照してください。

上記のAPIを統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [プライマリアプリケーション内で実行される基本的な認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [事前承認フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)
> [承認フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
