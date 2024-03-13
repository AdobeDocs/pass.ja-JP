---
title: 認証を開始
description: 認証を開始
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# 認証を開始 {#initiate-authorization}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

>[!NOTE]
>
> REST API 実装は、 [スロットルメカニズム](/help/authentication/throttling-mechanism.md)

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

承認応答を取得します。

| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | ストリーミングアプリ</br></br>または</br></br>プログラマーサービス | 1.要求者（必須）</br>2.  deviceId（必須）</br>3.  resource （必須）</br>4.  device_info/X-Device-Info （必須）</br>5.  _deviceType_</br> 6.  _deviceUser_ （廃止）</br>7.  _appId_ （廃止）</br>8.  追加のパラメーター（オプション） | GET | 認証の詳細または失敗した場合のエラーの詳細を含む XML または JSON。 以下のサンプルを参照してください。 | 200 — 成功  </br>403 — 成功なし |

{style="table-layout:auto"}

</br>


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| deviceId | デバイス ID バイト。 |
| リソース | resourceId（または MRSS フラグメント）を含む文字列。ユーザーが要求したコンテンツを識別し、MVPD 認証エンドポイントによって認識されます。 |
| device_info/</br></br>X-Device-Info | デバイス情報のストリーミング。</br></br>**注意**：この INFO は、URL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGETURL の長さの制限により、HTTP ヘッダーで X-Device-Info として渡す必要があります。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br>このパラメータが正しく設定されている場合、ESM は以下の指標を提供します。 [デバイスタイプ別に分類](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) クライアントレスを使用する場合に、Roku、AppleTV、Xbox など、様々な種類の分析を実行できます。</br></br>詳しくは、 [パス指標でのクライアントレスデバイスタイプパラメーターのメリット&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**:device_info がこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスのユーザー ID。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**:device_info がこのパラメーターを置き換えます。 |
| 追加のパラメーター | また、呼び出しには、次のような他の機能を有効にするオプションのパラメーターを含めることができます。</br></br>* generic_data - [プロモーション TempPass](/help/authentication/promotional-temp-pass.md)</br></br>例： `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**ストリーミングデバイスの IP アドレス**</br>
>クライアント/サーバ間実装の場合、ストリーミングデバイスの IP アドレスはこの呼び出しで暗黙的に送信されます。  サーバー間実装の場合、 **regcode** 呼び出しは、ストリーミングデバイスではなく、Programmer Service によっておこなわれます。ストリーミングデバイスの IP アドレスを渡すには、次のヘッダーが必要です。</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>場所 `<streaming\_device\_ip>` は、ストリーミングデバイスのパブリック IP アドレスです。</br></br>
>例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### レスポンスのサンプル {#sample-response}

* **例 1：成功**
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
    &quot;&#39;



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
>応答がプロキシ MVPD から来た場合は、という名前の追加要素が含まれる場合があります。 `proxyMvpd`.



* **ケース 2：承認が拒否されました**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
