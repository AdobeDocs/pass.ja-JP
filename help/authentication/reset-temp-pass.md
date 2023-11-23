---
title: 一時パスをリセット
description: 一時パスをリセット
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 一時パスをリセット {#reset-temp-pass}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。
>
>Reset Temp Pass API を使用するには、次の操作が必要です。
>- 登録済みのアプリケーションのソフトウェア文をサポートチームに問い合わせます。
>- 次に基づいてアクセストークンを取得する [動的クライアントの登録](dynamic-client-registration.md)
> 

次に対して **特定の一時パスをリセットする**, Adobe Pass Authentication は、プログラマーに *公開* web API:

- **環境：** reset Temp PassAdobe呼び出しを受け取るネットワーク Pay-TV パスサーバーエンドポイントを指定します。 可能な値： **Prequal** (*mgmt-prequal.auth.adobe.com*), **リリース** (*mgmt.auth.adobe.com*) または **カスタム** (Adobe内部テスト用に予約 )。
- **OAuth2 アクセストークン：** OAuth2 トークンは、AdobePay-TV 認証用のプログラマーを認証するために必要です。 このようなトークンは、 [動的クライアントの登録](dynamic-client-registration.md).
- **一時パス ID :** リセットする一時パス MVPD の一意の ID。（プログラマは複数の Temp Pass MVPD を使用し、特定の MVPD をリセットしたい場合）
- **汎用キー：** 一部の Temp Pass MVPDs ( 例： [プロモーション一時パス](promotional-temp-pass.md)) をクリックします。

上記のパラメーター ( *汎用キー*) は必須です。 次に、パラメーターと関連するネットワーク呼び出しの例を示します（例は、*curl *コマンドの形式です）。

- **環境：** リリース (*mgmt.auth.adobe.com*)
- **OAuth2 アクセストークン：** &lt;access_token> から [動的クライアントの登録](dynamic-client-registration.md)
- **プログラマー ID:** REF
- **一時パス ID :** TempPassREF
- **汎用キー：** null （値が指定されていません）

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

DELETEHTTP リクエストが **/reset** エンドポイント、を渡す *OAuth2 アクセストークン* を設定し、 *デバイス ID*, *要求者 ID* および *一時パス ID (MVPD ID)* をパラメーターとして使用します。

プログラマが *汎用キー*&#x200B;を呼び出すと、別の HTTP 呼び出しが実行されます ( 今回は **/reset/generic** エンドポイント )、を渡す *汎用キー* 内側 *key* リクエストパラメーター。

例えば、 *汎用キー* を（この種の機能をサポートする Temp Pass MVPDs 用に）電子メールアドレスハッシュに対して送信すると、次の HTTP 呼び出しが行われます ( 電子メールは `user@domain.com` その SHA-256 ハッシュは `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


次に対して **すべてのデバイスの特定の一時パスをリセットする**, Adobe Pass Authentication は、プログラマーに *公開* web API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>上記の URL は、以前のリセット API に代わるものです。 古いリセット API(v1) はサポートされなくなりました。

- **プロトコル：** HTTPS
- **ホスト：**
   - リリース — mgmt.auth.adobe.com
   - Prequal - mgmt-prequal.auth.adobe.com
- **パス：** /reset-tempass/v3/reset
- **クエリパラメーター：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **ヘッダー：** Authorization: Bearer &lt;access_token_here>
- **応答：**
   - 成功 — HTTP 204
   - 失敗：
      - HTTP 400 が正しくないリクエストを示しています
      - アクセスが拒否された場合は HTTP 401 クライアントは新しい access_token を MUST リクエストします。
      - HTTP 403（クライアント ID が要求の実行を許可されなくなった場合） 新しいクライアント資格情報が生成される必要があります。


例：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
