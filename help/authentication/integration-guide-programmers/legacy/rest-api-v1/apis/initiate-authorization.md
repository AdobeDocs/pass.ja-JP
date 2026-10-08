---
title: 認証を開始
description: 認証を開始
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%
---
# （レガシー）認証の開始 {#initiate-authorization}

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

承認応答を取得します。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者（必須） </br>2。  deviceId （必須） </br>3。  リソース （必須） </br>4。  device_info/X-Device-Info （必須） </br>5.  _deviceType_</br> 6.  _deviceUser_ （非推奨） </br>7。  _appId_ （非推奨） </br>8。  追加パラメーター（オプション） | GET | 認証の詳細またはエラーの詳細を含むXMLまたはJSONが失敗した場合。 以下のサンプルを参照してください。 | 200 – 成功</br>403 – 成功なし |

{style="table-layout:auto"}

</br>


| 入力パラメーター | 説明 |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| リソース | resourceId （またはMRSS フラグメント）を含む文字列は、ユーザーが要求したコンテンツを識別し、MVPD認証エンドポイントによって認識されます。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、HTTP ヘッダーでX-Device-Infoとして渡す必要があります。 </br></br>詳細については、[ デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>このパラメーターが正しく設定されている場合、ESMは、クライアントレスを使用する場合にデバイスの種類](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)ごとに[分割された指標を提供するため、Roku、AppleTV、Xboxなどのさまざまなタイプの分析を実行できます。</br></br> パス指標の[ クライアントレス デバイス タイプ パラメーターの利点&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注**&#x200B;を参照：device_infoはこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザーID。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**メモ**:device_infoがこのパラメーターに置き換わります。 |
| 追加パラメーター | 呼び出しには、次のような他の機能を有効にするオプションのパラメーターも含めることができます。</br></br>* generic_data - [ プロモーション TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>例：`generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**ストリーミングデバイス IP アドレス**</br>
>クライアント間の実装の場合、ストリーミングデバイスのIP アドレスは、この呼び出しで暗黙的に送信されます。  サーバー間の実装では、**regcode**&#x200B;呼び出しはストリーミングデバイスではなくプログラマーサービスによって行われます。ストリーミングデバイスのIP アドレスを渡すには、次のヘッダーが必要です：</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>ここで、`<streaming\_device\_ip>`はストリーミングデバイスのパブリック IP アドレスです。</br></br>
>例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### 応答サンプル {#sample-response}

* **ケース 1：成功**
</br>
  * **XML:**
  </br>

    &quot;&#39;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>応答がプロキシ MVPDから来る場合、その応答に`proxyMvpd`という名前の追加エレメントが含まれる場合があります。



* **ケース 2：承認が拒否されました**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
