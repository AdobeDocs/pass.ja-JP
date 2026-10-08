---
title: ショートメディアトークンの取得
description: ショートメディアトークンを取得
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%
---
# （レガシー） ショートメディアトークンの取得 {#obtain-short-media-token}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!NOTE]
>
> REST APIの実装は[&#x200B; スロットル メカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって制限されています

## REST API エンドポイント {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* 実稼動 – [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* ステージング - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* ステージング - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

ショートメディアトークンを取得します。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br>または</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>例：</br></br>&lt;SP_FQDN>/api/v1/tokens/media | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者（必須） </br>2。  deviceId （必須） </br>3。  リソース （必須） </br>4。  device_info/X-Device-Info （必須） </br>5.  _deviceType_</br> 6.  _deviceUser_ （非推奨） </br>7。  _appId_ （非推奨） | GET | Base64でエンコードされたメディアトークンを含むXMLまたはJSON。失敗した場合は、エラーの詳細。 | 200 – 成功</br>403 – 成功なし |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| 入力パラメーター | 説明 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| リソース | resourceId （またはMRSS フラグメント）を含む文字列は、ユーザーが要求したコンテンツを識別し、MVPD認証エンドポイントによって認識されます。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、HTTP ヘッダーでX-Device-Infoとして渡す必要があります。 </br></br>詳細については、[&#x200B; デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>このパラメーターが正しく設定されている場合、ESMは、クライアントレスを使用する場合にデバイスの種類&rbrack;/（help/authentication/entitlement-service-monitoring-overview.md#clientless_device_typeごとに&lbrack;分割される指標を提供するため、様々な種類の分析を実行できます。 例えば、Roku、AppleTV、Xboxなどです。</br></br>詳しくは、[&#x200B; クライアントレスのdevicetype パラメーターを使用するメリット &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注**&#x200B;を参照してください。device_infoはこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイス ユーザー識別子。</br></br>**注**：使用する場合、deviceUserは、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**メモ**:device_infoがこのパラメーターに置き換わります。 使用する場合、`appId`は、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求と同じ値を持つ必要があります。 |

{style="table-layout:auto"}

### 応答サンプル {#response}

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



### Media Verification Libraryの互換性

「短いメディアトークンを取得」呼び出しのフィールド `serializedToken`は、Adobe Media Verification Libraryに対して検証可能なBase64 エンコードされたトークンです。
