---
title: REST API リファレンス
description: Rest API リファレンス
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 5%
---
# （レガシー） REST API リファレンス {#rest-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## スロットル機構

Adobe Pass Authentication REST APIは、[&#x200B; スロットル メカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって管理されます。

## 応答形式 {#response-formats}


>[!NOTE]
>
> これらのサービスで提供されるAPIは、（応答を返すAPIの場合） XMLまたはJSONで応答を返すことができます。 リクエストでレスポンス形式を指定するには、次の3つの方法があります。
>
>* HTTP Accept Headerを`application/xml`または`application/json`に設定します。
>* リクエストペイロードで、パラメーター`format=xml`または`format=json`を指定します。
>* 拡張子が`.xml`または`.json`のweb サービスエンドポイントを呼び出します。 例：`/regcode.xml`または`/regcode.json`
>
>上記のいずれかの方法を指定できます。 競合する形式で複数のメソッドを指定すると、エラーや望ましくない出力が発生する場合があります。

## REST API エンドポイント {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Web サービスの概要 {#web_srvs_summary}

次の表に、クライアントレスアプローチで使用可能なweb サービスを示します。 詳しくは、web サービスエンドポイントをクリックしてください（リクエストとレスポンスのサンプル、入力パラメーター、HTTP メソッドなど）。


&#x200B;| Sr | Web サービスエンドポイント | 説明 | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | ホスト先： | 呼び出し元 |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | ランダムに生成された登録コードとログインページ URIを返します | 2 | Adobe </br>Reg Code Service | スマートデバイス |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | 登録コード UUID、登録コード、およびハッシュ化されたデバイス IDを含む登録コードレコードを返します | 8 | Adobe </br>Reg Code Service | Adobe Pass 認証 |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br>{requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | 要求者に対して設定されたMVPDのリストを返します | 5 | Adobe </br>Adobe Pass </br>認証</br> サービス | </br>Web </br> アプリにログイン |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | MVPDの選択イベントを通知して、認証プロセスを開始します。 MVPDから正常な応答を受信したときに調整される認証データベースのレコードを作成します（ステップ 13） | 7 | Adobe </br>Adobe Pass </br>認証</br> サービス | </br>Web </br> アプリにログイン |
| 5. | SAML アサーションコンシューマー | Adobe Pass認証とMVPD間の既存のSAML ワークフロー | 13 | Adobe Pass </br>認証</br> サービス | Adobe Pass 認証 |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | ログイン Web アプリは、試行されたログインフローが成功したかどうかを確認できます |                                                                                             | Adobe Pass </br>認証</br> サービス | </br>Web </br> アプリにログイン |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | AuthN トークン関連のメタデータを取得します | 15 | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br>  {requestorId}/regcode/ </br>{registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | reg コードレコードを削除し、再利用のためにreg コードをリリースします | 16 | Adobe </br>Reg Code Service | Adobe Pass 認証 |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | 承認応答を取得します。 | 17 | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | デバイスに有効期限のないAuthN トークンがあるかどうかを示します。 |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 見つかった場合は、AuthN トークンを返します。 |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | 見つかった場合は、AuthZ トークンを返します。 |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | 見つかった場合は、/api/v1/mediatokenと同じショートメディアトークンを返します |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | ショートメディアトークンを取得します |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | 事前承認済みリソースのリストを取得します |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | 事前承認済みリソースのリストを取得します |                                                                                             | Adobe Pass </br>認証</br> サービス | Web アプリへのログイン |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | ストレージからAuthNおよびAuthZ トークンを削除する |                                                                                             | Adobe Pass </br>認証</br> サービス | スマートデバイス |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 認証フロー完了後にユーザーメタデータを取得します | 該当なし | 該当なし | スマートデバイス |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Temp Passまたはプロモーション Temp Passの認証トークンを作成する | 該当なし | Adobe Pass </br>認証</br> サービス | スマートデバイス |


## REST API セキュリティ {#security}

すべてのAdobe Pass認証REST APIは、安全な通信のためにHTTPS プロトコルを使用して呼び出す必要があります。 さらに、呼び出されるAPIのほとんどは、[&#x200B; アクセストークンの取得](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って取得されたアクセストークンを含める必要があります。
