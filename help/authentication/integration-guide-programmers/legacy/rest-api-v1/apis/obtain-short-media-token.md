---
title: ショートメディアトークンの取得
description: 短いメディアトークンの取得
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 3%

---

# （レガシー）ショートメディアトークンの取得 {#obtain-short-media-token}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* ステージング - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* ステージング - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

ショートメディアトークンを取得します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> または </br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br> 例：</br></br>&lt;SP_FQDN>/api/v1/tokens/media | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | &#x200B;1.  要求者（必須） </br>2.  deviceId （必須） </br>3.  resource （必須） </br>4.  device_info/X-Device-Info （必須） </br>5.  _deviceType_</br> 6.  _deviceUser_ （非推奨） </br>7.  _appId_ （非推奨） | GET | Base64 でエンコードされたメディアトークンを含む XML または JSON。失敗した場合は、エラーの詳細。 | 200 – 成功 </br>403 – 成功なし |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| 入力パラメーター | 説明 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| resource | resourceId （または MRSS フラグメント）を含む文字列は、ユーザーからリクエストされたコンテンツを識別し、MVPD認証エンドポイントによって認識されます。 |
| device_info/</br></br>X-Device-Info | Streaming Device information.</br></br>**Note**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。</br></br>詳しくは、「デバイスと接続情報の受け渡し [ を参照してください ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターが正しく設定されている場合、ESM は、クライアントレスの使用時に [ デバイスタイプごとに分類 ]/（help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type）される指標を提供し、様々なタイプの分析を実行できるようにします。 例えば、Roku、AppleTV、Xbox などです。</br></br>[ クライアントレス devicetype パラメーターを使用するメリット ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意** を参照してください。device_info は、このパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。</br></br>**メモ**：使用する場合、deviceUser は [ 登録コードの作成 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。</br></br>**注意**：このパラメータは device_info に置き換えられます。 `appId` を使用する場合は、[ 登録コードの作成 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を指定する必要があります。 |

{style="table-layout:auto"}

### 応答のサンプル {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Media Verification Library の互換性

「短いメディアトークンの取得」呼び出しからのフィールド `serializedToken` は、Base64 でエンコードされたトークンで、Adobe Media 検証ライブラリに対して検証できます。
