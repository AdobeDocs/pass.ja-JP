---
title: 保護されたリソース
description: 保護されたリソース
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# 保護されたリソース {#protected-resources}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

保護されたリソースは、ユーザーがストリーミングできるコンテンツを表し、MVPD と参加プログラマーの間の契約によって確立された一意の値によって識別されます。

保護されたリソースは階層ツリー構造に従い、各レベルでコンテンツ認証の精度が向上します。

* ネットワーク
   * チャネル
      * 表示
         * エピソード
            * アセット

## 保護されたリソース識別子 {#identifiers}

リソースの一意の ID には、次の 2 つの形式があります。

* チャネル（ブランド）の一意の ID などの単純な文字列形式。
* タイトル、規制、保護者による制限のメタデータなどの追加情報を含むメディア RSS （MRSS）形式。

「REF30」などの単純なリソース識別子（チャネルを表すと想定）の場合は、次のように RSS リソース識別子に変換できます。

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

より複雑なリソース識別子の場合、RSS リソース識別子には次のような追加の評価情報を含めることができます。

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

## REST API V2 {#rest-api-v2}

事前認証および認証決定を取得するAdobe Pass Authentication API では、クライアントアプリケーションに保護されたリソースの識別子をパラメーターとして含める必要があります。

事前認証と承認の決定は、次の API を使用して取得できます。

* [特定の mvpd を使用した事前認証決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [特定の mvpd を使用した認証決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

保護されたリソースの一意の識別子を提供するために必要な形式を理解するには、上記 API の **リクエスト** および **サンプル** の節を参照してください。

一意の ID は、MVPDに直接渡されるので、Adobe Pass Authentication からは見えません。 MVPDがリソース ID を認識または解析できない場合は、Adobe Pass Authentication にエラーを返し、そのエラーを [Enhanced Error Code](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を使用してクライアントアプリケーションにリレーします。

上記の API を統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
