---
title: 認証トークンを確認
description: 認証トークンを確認
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 689e2f86550d9fa59337c15dd38767975a1d6d30
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# （レガシー）認証トークンの確認 {#check-authentication-token}

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

デバイスに期限切れでない認証トークンがあるかどうかを示します。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者（必須） </br>2。  deviceId （必須） </br>3。  device_info/X-Device-Info （必須） </br>4.  _deviceType_ </br>5。  _deviceUser_ （非推奨） </br>6。  _appId_ （非推奨） | GET | 失敗した場合のエラーの詳細を含むXMLまたはJSON。 | 200 – 成功</br>403 – 成功なし |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --- | --- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、http ヘッダーのX-Device-Infoとして渡す必要があります。 </br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>このパラメーターが正しく設定されている場合、ESMは、クライアントレスを使用する場合にデバイスの種類](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)ごとに[分割された指標を提供するため、Roku、AppleTV、Xboxなどのさまざまなタイプの分析を実行できます。</br></br>詳細については、[Adobe Pass Authentication metricsでClientless deviceType パラメーターを使用する利点&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注**&#x200B;を参照してください。device_infoはこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザーID。 |
| _appId_ | アプリケーション ID/名前。</br>**注**:device_infoはこのパラメーターを置き換えます。 |

{style="table-layout:auto"}


## 応答（失敗した場合） {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

**[REST API リファレンスに戻る](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)**
