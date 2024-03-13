---
title: REST API リファレンス
description: Rest api リファレンス
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 2%

---

# REST API リファレンス {#rest-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## スロットルメカニズム

Adobe Pass Authentication REST API は、 [スロットルメカニズム](/help/authentication/throttling-mechanism.md).

## 応答の形式 {#response-formats}


>[!NOTE]
>
> これらのサービスで提供される API は、XML または JSON（応答を返す API の場合）で応答を返すことができます。 リクエストで応答形式を指定する方法は 3 つあります。
>
>* HTTP Accept ヘッダーをに設定します。 `application/xml` または `application/json`.
>* リクエストペイロードで、パラメーターを指定します。 `format=xml` または `format=json`.
>* 拡張機能を使用して Web サービスエンドポイントを呼び出す `.xml` または `.json`. 例： `/regcode.xml` または `/regcode.json`
>
>上記のメソッドのいずれかを指定できます。 形式が競合する複数のメソッドを指定すると、エラーや望ましくない出力が発生する場合があります。

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Web サービスの概要 {#web_srvs_summary}

次の表に、クライアントレスアプローチで使用可能な Web サービスを示します。 詳しくは、Web サービスエンドポイントをクリックしてください（リクエストと応答、入力パラメーター、HTTP メソッドなど）。


| Sr | Web サービスエンドポイント | 説明 | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->。 | ホスト時刻 | 呼び出し元 |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode](/help/authentication/registration-code-request.md) | ランダムに生成された登録コードとログインページの URI を返します | 2 | Adobe  </br>Reg Code Service | スマートデバイス |
| 2. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/return-registration-record.md) | 登録コード UUID、登録コード、ハッシュ化されたデバイス ID を含む登録コードレコードを返します | 8 | Adobe  </br>Reg Code Service | Adobe Pass 認証 |
| 3. | [&lt;sp_fqdn>/api/v1/config/  </br>  {requestorId}](/help/authentication/provide-mvpd-list.md) | リクエスト元の設定済み MVPD のリストを返します | 5 | Adobe  </br>Adobe Pass  </br>認証  </br>サービス | ログイン  </br>Web  </br>アプリ |
| 4. | [&lt;sp_fqdn>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | MVPD 選択イベントを通知して AuthN プロセスを開始します。 認証データベースにレコードを作成します。このレコードは、MVPD から正常な応答を受け取ったときに紐付けされます（手順 13）。 | 7 | Adobe  </br>Adobe Pass  </br>認証  </br>サービス | ログイン  </br>Web  </br>アプリ |
| 5. | SAML アサーションコンシューマー | Adobe Pass Authentication と MVPD の間の既存の SAML ワークフロー | 13 | Adobe Pass  </br>認証  </br>サービス | Adobe Pass 認証 |
| 6. | [&lt;sp_fqdn>/api/v1/checkauthn/  </br>  {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | ログイン Web アプリは、試行されたログインフローが成功したかどうかを確認できます |     | Adobe Pass  </br>認証   </br>サービス | ログイン   </br>Web   </br>アプリ |
| 7. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | AuthN トークン関連のメタデータを取得します | 15 | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 8. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/delete-registration-record.md) | reg コードレコードを削除し、reg コードを解放して再利用します。 | 16 | Adobe  </br>Reg Code Service | Adobe Pass 認証 |
| 9. | [&lt;sp_fqdn>/api/v1/authorize](/help/authentication/initiate-authorization.md) | 承認応答を取得します。 | 17 | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 10. | [&lt;sp_fqdn>/api/v1/checkauthn](/help/authentication/check-authentication-token.md) | デバイスに期限切れでない AuthN トークンがあるかどうかを示します。 |     | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 11. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | AuthN トークンが見つかった場合は、そのトークンを返します。 |     | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 12. | [&lt;sp_fqdn>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | AuthZ トークンが見つかった場合は、そのトークンを返します。 |     | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 13. | [&lt;sp_fqdn>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | 見つかった場合、Short Media トークンを返します（/api/v1/mediatoken と同じ） |     | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 14. | [&lt;sp_fqdn>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | ショートメディアトークンを取得 |     | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 15. | [&lt;sp_fqdn>/api/v1/preauthorize](/help/authentication/retrieve-list-of-preauthorized-resources.md) | 事前に認証されたリソースのリストを取得します |     | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |
| 16. | [&lt;sp_fqdn>/api/v1/preauthorize/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | 事前に認証されたリソースのリストを取得します |     | Adobe Pass  </br>認証  </br>サービス | Web アプリにログイン |
| 17. | [&lt;sp_fqdn>/api/v1/logout](/help/authentication/initiate-logout.md) | ストレージから AuthN および AuthZ トークンを削除 |     | Adobe Pass  </br>認証   </br>サービス | スマートデバイス |
| 18. | [&lt;sp_fqdn>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | 認証フローの完了後にユーザーメタデータを取得する | 該当なし | 該当なし | スマートデバイス |
| 19. | [&lt;sp_fqdn>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | 一時パスまたはプロモーション一時パスの認証トークンを作成する | 該当なし | Adobe Pass  </br>認証  </br>サービス | スマートデバイス |


## REST API セキュリティ {#security}

安全な通信を実現するには、すべてのAdobe Pass Authentication クライアントレス API を HTTPS プロトコルを使用して呼び出す必要があります。 さらに、呼び出される API のほとんどには、 [動的クライアントの登録](/help/authentication/dynamic-client-registration.md).
