---
title: 認証トークンの取得
description: 認証トークンの取得
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%
---
# （レガシー）認証トークンの取得 {#retrieve-authentication-token}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!NOTE]
>
> REST APIの実装は[ スロットル メカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって制限されています

## REST API エンドポイント {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

認証（AuthN）トークンを取得します。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>例：</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者（必須） </br>2。  deviceId （必須） </br>3。  device_info/X-Device-Info （必須） </br>4.  _deviceType_ （非推奨） </br>5。  _deviceUser_ （非推奨） </br>6。  _appId_ （非推奨） | GET | 失敗した場合の認証情報またはエラーの詳細を含むXMLまたはJSON。 | 200 – 成功。 </br>404 - トークンが見つかりません</br>410 - トークンの有効期限が切れました |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --- | --- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、HTTP ヘッダーでX-Device-Infoとして渡す必要があります。 </br></br>詳細については、[ デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>**注意**:device_infoはこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイス ユーザー識別子。</br></br>**注**：使用する場合、deviceUserは、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**メモ**:device_infoがこのパラメーターに置き換わります。 使用する場合、`appId`は、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求と同じ値を持つ必要があります。 |

{style="table-layout:auto"}

</br>

### 応答サンプル {#response}



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
