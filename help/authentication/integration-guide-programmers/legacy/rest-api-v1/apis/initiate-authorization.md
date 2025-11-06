---
title: 認証の開始
description: 認証の開始
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# （レガシー）認証の開始 {#initiate-authorization}

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

認証応答を取得します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | &#x200B;1. リクエスター（必須） </br>2.  deviceId （必須） </br>3.  resource （必須） </br>4.  device_info/X-Device-Info （必須） </br>5.  _deviceType_</br> 6.  _deviceUser_ （非推奨） </br>7.  _appId_ （非推奨） </br>8。  追加のパラメーター（オプション） | GET | 認証の詳細またはエラーの詳細を含む XML または JSON （失敗した場合）。 以下のサンプルを参照してください。 | 200 – 成功 </br>403 – 成功なし |

{style="table-layout:auto"}

</br>


| 入力パラメーター | 説明 |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| resource | resourceId （または MRSS フラグメント）を含む文字列は、ユーザーからリクエストされたコンテンツを識別し、MVPD認証エンドポイントによって認識されます。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br></br> 詳しくは、「デバイスと接続情報の受け渡し [&#x200B; を参照してください &#x200B;](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターが正しく設定されている場合、ESM では、クライアントレスの使用時に [&#x200B; デバイスタイプごとに分類 &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) される指標を提供し、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できるようにします。</br></br> パス指標の [&#x200B; クライアントレスデバイスタイプパラメーターのメリット &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**:device_info はこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**：このパラメーターは device_info に置き換えられます。 |
| 追加のパラメーター | また、この呼び出しには、次のような他の機能を有効にするオプションのパラメーターが含まれる場合もあります。</br></br>* generic_data - [Promotion TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>Example: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**ストリーミングデバイスの IP アドレス**</br>
>クライアントからサーバーへの実装の場合、ストリーミングデバイスの IP アドレスは、この呼び出しで暗黙的に送信されます。  **regcode** 呼び出しが、ストリーミングデバイスではなく、プログラマーサービスによって行われるサーバー間実装の場合、ストリーミングデバイスの IP アドレスを渡すには、次のヘッダーが必要です。</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>ここで、`<streaming\_device\_ip>` はストリーミングデバイスのパブリック IP アドレスです。</br></br>
>例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### 応答のサンプル {#sample-response}

* **ケース 1：成功**
</br>
  * **XML:**
  </br>

    ```XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    ```



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
>プロキシMVPDからの応答の場合、`proxyMvpd` という名前の要素が含まれる場合があります。



* **ケース 2：認証が拒否される**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
