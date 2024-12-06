---
title: 一時パスをリセット
description: 一時パスをリセット
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
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
> * [ クライアント資格情報の取得 ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメントの説明に従って、クライアント資格情報を取得します。
> * [ アクセストークンの取得 ](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、アクセストークンを取得します。
>
> 登録されたアプリケーションを作成してソフトウェアのステートメントをダウンロードする方法について詳しくは、[ 動的クライアント登録の概要 ](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

**デバイスまたはすべてのデバイスの特定の一時パスをリセット** するために、Adobe Pass認証ではプログラマーに *パブリック* web API を提供しています。

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **プロトコル：** HTTPS
* **ホスト：**
   * リリース - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **パス：** /reset-tempass/v3/reset
* **クエリパラメーター：** device_id （値を指定しない場合、デフォルトは all）、requestor_id （必須）、mvpd_id （必須）、appId、deviceUser、environment
* **ヘッダー：** 認証：ベアラー &lt;access_token_here>
* **応答：**
   * 成功 – HTTP 204
   * 失敗：xw
      * 誤ったリクエストに対する HTTP 400
      * HTTP 401：アクセスが拒否された場合。 クライアントは新しい access_token を要求する必要があります。
      * クライアント ID がリクエストの実行を許可されなくなった場合は HTTP 403。 新しいクライアント資格情報を生成する必要があります。


例：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Adobe Pass認証では、**汎用キーまたはすべてのキーの柔軟な一時パスをリセット** するために、プログラマーに *公開* web API を提供しています。

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **プロトコル：** HTTPS
* **ホスト：**
   * リリース - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **パス：** /reset-tempass/v3/reset/generic
* **クエリパラメーター：** キー（値を指定しない場合、デフォルトは all）、requestor_id （必須）、mvpd_id （必須）、environment
* **ヘッダー：** 認証：ベアラー &lt;access_token_here>
* **応答：**
   * 成功 – HTTP 204
   * 失敗：
      * 誤ったリクエストに対する HTTP 400
      * HTTP 401：アクセスが拒否された場合。 クライアントは新しい access_token を要求する必要があります。
      * クライアント ID がリクエストの実行を許可されなくなった場合は HTTP 403。 新しいクライアント資格情報を生成する必要があります。


例えば、*汎用キー* を電子メールアドレスハッシュに設定します（例：
この種の機能をサポートする Temp Pass MVPD）は、
http 呼び出しの後（メールは SHA-256`user@domain.com` 送信されます）
ハッシュは `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`）:

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
