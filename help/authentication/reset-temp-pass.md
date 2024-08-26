---
title: 一時パスをリセット
description: 一時パスをリセット
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# 一時パスをリセット {#reset-temp-pass}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> Reset Temp Pass API を使用する前に、次の前提条件が満たされていることを確認してください。
>
> * [ クライアント資格情報の取得 ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメントの説明に従って、クライアント資格情報を取得します。
> * [ アクセストークンの取得 ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、アクセストークンを取得します。
>
> 登録されたアプリケーションを作成してソフトウェアのステートメントをダウンロードする方法について詳しくは、[ 動的クライアント登録の概要 ](./dcr-api/dynamic-client-registration-overview.md) ドキュメントを参照してください。

**特定の一時パスをリセット** するために、Adobe Pass認証はプログラマーに *パブリック* web API を提供します。

* **Environment:** リセットされた一時パス ネットワーク呼び出しを受け取るAdobeの Pay-TV パス サーバーエンドポイントを指定します。 使用可能な値：**Prequal** （*mgmt* prequal.auth.adobe.com*）、**リリース** （*mgmt.auth.adobe.com*）または **カスタム** （Adobeの内部テスト用に予約済み）。
* **OAuth2 アクセストークン：** Adobeの有料テレビ認証用にプログラマーを認証するには、OAuth2 トークンが必要です。 このようなトークンは、[ アクセストークンの取得 ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って取得できます。
* **Temp Pass ID:** リセットされる Temp Pass MVPD の一意の ID。（プログラマは複数の Temp Pass MVPD を使用でき、特定の MVPD をリセットしたい）
* **汎用キー：** 一部の一時パス MVPD （例：[ プロモーション一時パス ](promotional-temp-pass.md)）。

上記のパラメーターはすべて（（汎用キー *を除く）必須* す。 パラメーターと関連するネットワーク呼び出しの例を次に示します（例は*curl *コマンドの形式です）。

* **環境：** リリース（*mgmt.auth.adobe.com*）
* [ アクセストークンの取得 ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、**OAuth2 アクセストークン：** &lt;access_token> を設定します
* **プログラマ ID:** 参照
* **一時パス ID:** TempPassREF
* **汎用キー：** null （値が指定されていません）

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

認証ヘッダーの *OAuth2 アクセストークン* と、*デバイス ID **、* リクエスター ID *、* 一時渡し ID （MVPD ID）をパラメーターとして渡して、**/reset *エンドポイントに対してDELETEの HTTP リクエストが行わ* ます。

プログラマーが *Generic Key* の値を指定すると、別の HTTP 呼び出しが実行され（今回は **/reset/generic** エンドポイントに対して）、*key* リクエストパラメーター内の *Generic Key* を渡します。

例えば、*汎用キー* を電子メールアドレスハッシュに設定します（例：
この種の機能をサポートする Temp Pass MVPD）は、
http 呼び出しの後（メールは SHA-256`user@domain.com` 送信されます）
ハッシュは `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`）:

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


**すべてのデバイスに対して特定の一時パスをリセット** するために、Adobe Pass認証はプログラマーに *パブリック* web API を提供します。

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>上記の URL は、以前のリセット API より優先されます。 古いリセット API （v1）はサポートされなくなりました。

* **プロトコル：** HTTPS
* **ホスト：**
   * リリース - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **パス：** /reset-tempass/v3/reset
* **クエリパラメーター：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **ヘッダー：** 認証：ベアラー &lt;access_token_here>
* **応答：**
   * 成功 – HTTP 204
   * 失敗：
      * 誤ったリクエストに対する HTTP 400
      * HTTP 401：アクセスが拒否された場合。 クライアントは新しい access_token を要求する必要があります。
      * クライアント ID がリクエストの実行を許可されなくなった場合は HTTP 403。 新しいクライアント資格情報を生成する必要があります。


例：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
