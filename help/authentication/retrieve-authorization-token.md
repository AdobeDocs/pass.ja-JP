---
title: 認証トークンを取得
description: 認証トークンを取得
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 認証トークンを取得 {#retrieve-authorization-token}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

認証 (AuthZ) トークンを取得します。


| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authz</br></br>例：</br></br>&lt;sp_fqdn>/api/v1/tokens/authz | ストリーミングアプリ</br></br>または</br></br>プログラマーサービス | 1.要求者（必須）</br>2.  deviceId（必須）</br>3.  resource （必須）</br>4.  device_info/X-Device-Info （必須）</br>5.  _deviceType_</br> 6.  _deviceUser_ （廃止）</br>7.  _appId_ （廃止） | GET | 1.成功</br>2.  認証トークン  </br>    見つからないか期限切れです：   </br>    理由を説明する XML  </br>    （authn トークンが見つかりません）</br>3.  認証トークン  </br>    見つかりません：  </br>    XML の説明</br>4.  認証トークン  </br>    期限切れ：  </br>    XML の説明 | 200 — 成功  </br>412 - AuthN なし</br></br>404 - AuthZ なし</br></br>410 - AuthZ の期限切れ |

{style="table-layout:auto"}

</br>

| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| deviceId | デバイス ID バイト。 |
| リソース | resourceId（または MRSS フラグメント）を含む文字列。ユーザーが要求したコンテンツを識別し、MVPD 認証エンドポイントによって認識されます。 |
| device_info/</br></br>X-Device-Info | デバイス情報のストリーミング。</br></br>**注意**：この INFO は、URL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGETURL の長さの制限により、HTTP ヘッダーで X-Device-Info として渡す必要があります。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | デバイスタイプ（例：Roku、PC）。</br></br>このパラメータが正しく設定されている場合、ESM は以下の指標を提供します。 [デバイスタイプ別に分類](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) クライアントレスを使用する場合に、Roku、AppleTV、Xbox など、様々な種類の分析を実行できます。</br></br>詳しくは、 [パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**:device_info がこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスのユーザー ID。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**:device_info がこのパラメーターを置き換えます。 |

{style="table-layout:auto"}


### レスポンスのサンプル {#response}



#### 成功

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### 認証トークンが見つからないか、期限切れです：

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


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
        "message": "Not Found",
        "details": null
    }
```

</br>



#### 期限切れの認証トークン：

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
