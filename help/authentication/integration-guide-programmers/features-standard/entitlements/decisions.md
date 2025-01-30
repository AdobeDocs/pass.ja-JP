---
title: 決定
description: 決定
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 決定 {#decisions}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

決定は、ユーザーのAdobe Pass認証または事前認証の問い合わせに基づいて、MVPD認証 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) によって生成され、「保護されたコンテンツ [ へのアクセスが許可されているか拒否されているかを判定し ](#protected-resources) す。

提供される決定には、呼び出された API に応じて 2 つのタイプがあります。

* 有益な決定である [ 事前認証の決定 ](#preauthorization-decisions)。
* [ 承認決定 ](#authorization-decisions) 権限のある決定です。

## 事前認証の決定 {#preauthorization-decisions}

事前認証の決定とは、[ 保護されたリソース ](#protected-resources) に対するユーザーのアクセスを、MVPDが許可するか拒否するかをクライアントアプリケーションに通知するための有益な決定です。

事前認証（プリフライト認証）の目的は、ユーザーが表示する資格のあるコンテンツに関する正確な情報をアプリケーションに表示できるようにすることです。 これを実現するには、アクセスステータスを反映するロック済みアイコンやロック解除されたアイコンなどのインジケーターでユーザーインターフェイスを強化します。

>[!IMPORTANT]
>
> 事前承認の決定は、[ 承認の決定 ](#authorization-decisions) の目的なので、リソースを再生する権限のある方法で使用しないでください。

事前認証 API の使用は必須ではありません。フィルタリングを行わずにリソースのカタログを表示する場合は、クライアントアプリケーションでこれをスキップできます。

クライアントアプリケーションでこの機能を使用する場合は、事前認証の決定は、API リクエストごとに限られた数のリソース（通常は 5 つまで）でのみ取得できることに注意が必要です。

>[!IMPORTANT]
> 
> リソースの最大数は、MVPD およびAdobe Pass認証担当者との合意に達した後にのみ増やすことができます。 同意が得られたら、組織の管理者またはユーザーに代わってAdobe Pass認証担当者が、Adobe Pass TVE ダッシュボードを使用して変更を実装できます。
> 
> 詳しくは、[TVE ダッシュボード統合ユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties) ドキュメントを参照してください。

MVPD は、パフォーマンスと 1 つの API リクエストで処理できる最大リソース数に明確な影響を与える、様々なメカニズムを通じて事前認証をサポートできます。

事前認証をサポートする既存のメカニズムについて詳しくは、[MVPD Preflight Authorization](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md) ドキュメントを参照してください。

>[!IMPORTANT]
>
> 完全なプリフライト認証のサポートを持たない MVPD の場合は、事前に MVPD およびAdobe Pass認証担当者と事前認証の使用を合意する必要があります。これにより、パフォーマンスの問題が発生し、応答時間が遅くなる可能性があります。

## 認証の決定 {#authorization-decisions}

認証決定とは、[ 保護されたリソース ](#protected-resources) に対するユーザーのアクセスを許可または拒否するMVPD決定に、クライアントアプリケーションが準拠できるようにする、権限のある決定です。

認証の目的は、MVPDでの権限検証やAdobe Pass Authentication からのメディアトークンの受信に従って、ユーザーから要求されたリソースをアプリケーションが再生できるようにすることです。

>[!IMPORTANT]
> 
> Adobe Pass認証では、プログラマーがメディアトークン検証用ライブラリを使用して、認証決定に含まれるメディアトークンを検証し、ビデオストリームを開始する前に安全にアクセスできるようにすることをお勧めします。
> 
> 詳しくは、[ メディアトークン ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) ドキュメントを参照してください。

認証 API の使用は必須です。クライアントアプリケーションは、ユーザーがリクエストするリソースを再生する場合、このフェーズをスキップできません。ストリームをリリースする前に、ユーザーが権限を持っていることをMVPDで確認する必要があるからです。

承認の決定は、API リクエストごとに限られた数のリソース（通常は 1）に対してのみ取得できます。

>[!IMPORTANT]
>
> リソースの最大数は、MVPD およびAdobe Pass認証担当者との合意に達した後にのみ増やすことができます。

## 保護されたリソース {#protected-resources}

保護されたリソースとは、ストリーミング可能なコンテンツを指し、MVPD と参加プログラマーの間の契約を通じて定義された一意の値によって識別されます。

保護されたリソースは階層ツリー構造に従い、各レベルでコンテンツ認証の精度が向上します。

* ネットワーク
   * チャネル
      * 表示
         * エピソード
            * アセット

>[!IMPORTANT]
>
> 事前認証（preflight authorization）は、単純な文字列または MRSS 形式の識別子を持つチャネルレベルのリソースに焦点を当てています。
> 
> 事前認証の場合は、`CDATA` セクションを含む識別子を持つリソースを使用することはお勧めしません。主に、MRSS によって定義されるアセットレベルのリソースに使用されるからです。

### リソース識別子 {#resource-identifier}

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

一意の ID は主にAdobe Pass Authentication に対して不透明ですが、トランスフォーマーはMVPDの機能と要件に基づいて適用される場合があります。 MVPDがリソース ID を認識または解析できない場合は、Adobe Pass Authentication にエラーを返し、その後、[Enhanced Error Code](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) を使用してエラーをクライアントアプリケーションにリレーします。

## REST API V2 {#rest-api-v2}

事前認証の決定は、次の API を使用して取得できます。

* [特定の mvpd を使用した事前認証決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

認証決定は、次の API を使用して取得できます。

* [特定の mvpd を使用した認証決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

事前認証と認証決定の構造を理解するには、上記の API の **Response** および **Samples** の節を参照してください。

上記の API を統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本的な事前認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
