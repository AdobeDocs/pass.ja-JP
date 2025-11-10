---
title: 認証トークンの取得
description: 認証トークンの取得
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# （レガシー）認証トークンの取得 {#retrieve-authentication-token}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

認証（AuthN） トークンを取得します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br> 例：</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | &#x200B;1. リクエスター（必須） </br>2.  deviceId （必須） </br>3.  device_info/X-Device-Info （必須） </br>4.  _deviceType_ （非推奨） </br>5.  _deviceUser_ （非推奨） </br>6.  _appId_ （非推奨） | GET | 失敗した場合に認証情報またはエラーの詳細を含む XML または JSON。 | 200 – 成功。  </br>404 - トークンが見つかりません </br>410 - トークンの有効期限が切れています |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br></br> 詳しくは、「デバイスと接続情報の受け渡し [&#x200B; を参照してください &#x200B;](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br>**注意**：このパラメーターは device_info に置き換えられます。 |
| _deviceUser_ | デバイスユーザー識別子。</br></br>**注意**：使用する場合、deviceUser は [&#x200B; 登録コードの作成 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**：このパラメーターは device_info に置き換えられます。 `appId` を使用する場合は、[&#x200B; 登録コードの作成 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を指定する必要があります。 |

{style="table-layout:auto"}

</br>

### 応答のサンプル {#response}



#### 成功

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### 認証トークンが見つかりません：

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```


**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found"
    }
```
