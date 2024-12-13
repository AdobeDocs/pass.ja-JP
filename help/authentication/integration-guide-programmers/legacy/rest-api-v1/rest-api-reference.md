---
title: REST API リファレンス
description: Rest api リファレンス
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 2%

---

# （レガシー） REST API リファレンス {#rest-api-reference}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## スロットルメカニズム

Adobe Pass認証 REST API は、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) によって制御されます。

## 応答形式 {#response-formats}


>[!NOTE]
>
> これらのサービスで提供される API は、XML または JSON のいずれかで応答を返すことができます（応答を返す API の場合）。 リクエストで応答形式を指定する方法は 3 つあります。
>
>* HTTP Accept ヘッダーを `application/xml` または `application/json` に設定します。
>* リクエストペイロードで、パラメーター `format=xml` または `format=json` を指定します。
>* 拡張子が `.xml` または `.json` の web サービスエンドポイントを呼び出します。 例：`/regcode.xml` または `/regcode.json`
>
>上記のいずれかの方法を指定できます。 形式が競合する複数のメソッドを指定すると、エラーや望ましくない出力が発生する可能性があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Web サービスの概要 {#web_srvs_summary}

次の表に、クライアントレスアプローチで使用可能な web サービスを示します。 詳しくは、web サービスエンドポイントをクリックします（サンプルのリクエストと応答、入力パラメーター、HTTP メソッドなど）。


| 共有済み | Web サービスエンドポイント | 説明 | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->。 | ホスト： | 呼び出し元 |
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------|
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | ランダムに生成された登録コードとログインページ URI を返します | 2 | Adobe </br>Reg コード サービス | スマートデバイス |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | 登録コード UUID、登録コード、ハッシュ化されたデバイス ID を含む登録コードレコードを返します | 8 | Adobe </br>Reg コード サービス | Adobe Pass 認証 |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | リクエスターに設定された MVPD のリストを返します | 5 | Adobe</br>Adobe Pass </br>authentication </br>Service | ログイン </br>Web </br>App |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | MVPD選択イベントを通知して AuthN プロセスを開始します。 認証データベースにレコードを作成します。このレコードは、MVPDからリクエストが成功した場合に紐付けされます（手順 13） | 7 | Adobe</br>Adobe Pass </br>authentication </br>Service | ログイン </br>Web </br>App |
| 5. | SAML アサーションコンシューマー | Adobe Pass認証とMVPDの間の既存の SAML ワークフロー | 13 | Adobe Pass </br>authentication </br>Service | Adobe Pass 認証 |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | ログイン Web アプリケーションは、試行されたログインフローが成功したかどうかを確認できます |                                                                                             | Adobe Pass </br> 認証   </br> サービス | ログイン   </br>Web   </br> アプリ |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | AuthN トークン関連メタデータを取得します | 15 | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md) | 登録コードレコードを削除し、再利用のために登録コードを解放します | 16 | Adobe </br>Reg コード サービス | Adobe Pass 認証 |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | 認証応答を取得します。 | 17 | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) | デバイスに期限切れでない AuthN トークンがあるかどうかを示します。 |                                                                                             | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 見つかった場合は、AuthN トークンを返します。 |                                                                                             | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | 見つかった場合は、AuthZ トークンを返します。 |                                                                                             | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media] （/help/authentication/integration-guide-programmer/rest-apis/rest-api-v1/apis/obtain-short-media-token.md | 見つかった場合、短いメディアトークンを返します – /api/v1/mediatoken と同じ |                                                                                             | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 14。 | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | ショートメディアトークンを取得 |                                                                                             | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) | 事前承認済みリソースのリストを取得します |                                                                                             | Adobe Pass </br>authentication </br>Service | スマートデバイス |
| 16。 | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | 事前承認済みリソースのリストを取得します |                                                                                             | Adobe Pass </br>authentication </br>Service | ログイン Web アプリ |
| 17。 | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | ストレージからの AuthN および AuthZ トークンの削除 |                                                                                             | Adobe Pass </br> 認証   </br> サービス | スマートデバイス |
| 18。 | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 認証フロー完了後にユーザーメタデータを取得します | 該当なし | 該当なし | スマートデバイス |
| 19。 | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md) | 一時パスまたはプロモーション一時パスの認証トークンの作成 | 該当なし | Adobe Pass </br>authentication </br>Service | スマートデバイス |


## REST API セキュリティ {#security}

すべてのAdobe Pass認証 REST API は、安全な通信のために HTTPS プロトコルを使用して呼び出す必要があります。 さらに、と呼ばれるほとんどの API には、[ アクセストークンの取得 ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントで説明されているように取得されたアクセストークンが含まれている必要があります。
