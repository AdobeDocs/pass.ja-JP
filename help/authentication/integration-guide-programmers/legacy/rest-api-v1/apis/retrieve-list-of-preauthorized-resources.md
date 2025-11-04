---
title: 事前承認済みリソースのリストの取得
description: 事前承認済みリソースのリストの取得
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# （レガシー）事前承認済みリソースのリストの取得 {#retrieve-list-of-preauthorized-resources}

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

Adobe Pass Authentication に対するリクエストで、事前に許可されたリソースのリストを取得します。

API には、ストリーミングアプリまたはプログラマーサービス用の API セットと、2 番目の画面の web アプリ用の API セットの 2 つのセットがあります。 ここでは、ストリーミングアプリまたはプログラマーサービスの API について説明します。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/事前承認 | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | &#x200B;1. リクエスター（必須） </br>2.  deviceId （必須） </br>3.  resource （必須） </br>4.  device_info/X-Device-Info （必須） </br>5.  _deviceType_</br> 6.  _deviceUser_ （非推奨） </br>7.  _appId_ （非推奨） | GET | 個々の事前認証の決定やエラーの詳細が含まれる XML または JSON。 以下のサンプルを参照してください。 | 200 - Success</br></br>400 - Bad request</br></br>401 - Unauthorized</br></br>405 - Method not allowed </br></br>412 - Precondition failed</br></br>500 – 内部サーバーエラー |


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| resource | ユーザーがアクセス可能で、MVPD認証エンドポイントによって認識されるコンテンツを識別する resourceId のコンマ区切りリストを含む文字列。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br></br> 詳しくは、「デバイスと接続情報の受け渡し [&#x200B; を参照してください &#x200B;](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターを正しく設定すると、ESM では、クライアントレスの使用時に [&#x200B; デバイスタイプごとに分類 &#x200B;](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) される指標を提供するので、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できます。</br></br> 参照 [&#x200B; パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**:`device_info` はこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**：このパラメーターは device_info に置き換えられます。 |



### 応答のサンプル {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
