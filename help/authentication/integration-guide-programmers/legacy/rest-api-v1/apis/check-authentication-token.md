---
title: 認証トークンを確認
description: 認証トークンを確認
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# （レガシー）認証トークンを確認 {#check-authentication-token}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

デバイスに期限切れでない認証トークンがあるかどうかを示します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1. リクエスター（必須） </br>2.  deviceId （必須） </br>3.  device_info/X-Device-Info （必須） </br>4.  _deviceType_</br>5.  _deviceUser_ （非推奨） </br>6.  _appId_ （非推奨） | GET | 失敗した場合にエラーの詳細を含む XML または JSON。 | 200 – 成功   </br>403 – 成功しません |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。</br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターが正しく設定されている場合、ESM では、クライアントレスの使用時に [ デバイスタイプごとに分類 ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) される指標を提供し、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できるようにします。</br></br> 詳しくは、[Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意** を参照してください。device_info はこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。 |
| _appId_ | アプリケーション ID/名前。</br>**注意**：このパラメーターは device_info に置き換えられます。 |

{style="table-layout:auto"}


## 応答（失敗した場合） {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [REST API リファレンスに戻る ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
