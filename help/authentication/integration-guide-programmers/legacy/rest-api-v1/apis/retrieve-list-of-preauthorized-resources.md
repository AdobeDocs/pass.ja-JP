---
title: 事前承認済みリソースのリストの取得
description: 事前承認済みリソースのリストの取得
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%
---
# （レガシー）事前承認済みリソースのリストの取得 {#retrieve-list-of-preauthorized-resources}

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

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

事前承認済みリソースのリストを取得するためのAdobe Pass認証へのリクエスト。

APIには2つのセットがあります。1つはストリーミングアプリまたはプログラマーサービス用、もう1つはSecond Screen Web App用です。 このページでは、ストリーミングアプリまたはプログラマーサービスのAPIについて説明します。


| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者（必須） </br>2。  deviceId （必須） </br>3。  リソース （必須） </br>4。  device_info/X-Device-Info （必須） </br>5.  _deviceType_</br> 6.  _deviceUser_ （非推奨） </br>7。  _appId_ （非推奨） | GET | 個別の事前認証の決定またはエラーの詳細を含むXMLまたはJSON。 以下のサンプルを参照してください。 | 200 – 成功</br></br>400 – 不正なリクエスト </br></br>401 – 許可されていない</br></br>405 - メソッドが許可されていません</br></br>412 – 前提条件が失敗しました</br></br>500 – 内部サーバーエラー |


| 入力パラメーター | 説明 |
| --- | --- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| リソース | ユーザーがアクセスできる可能性のあるコンテンツを識別し、MVPD認証エンドポイントが認識する、resourceIdのコンマ区切りリストを含む文字列。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、HTTP ヘッダーでX-Device-Infoとして渡す必要があります。 </br></br>詳細については、[&#x200B; デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>このパラメーターが正しく設定されている場合、ESMはクライアントレスを使用する際にデバイスの種類[&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)ごとに分割された指標を提供するため、様々なタイプの分析を実行できます（Roku、AppleTV、Xboxなど）。</br></br>参照、[&#x200B; パス指標でクライアントレスのデバイスタイプパラメーターを使用する利点&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注**: `device_info`は、このパラメーターに置代わります。 |
| _deviceUser_ | デバイスユーザーID。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**メモ**:device_infoがこのパラメーターに置き換わります。 |



### 応答サンプル {#sample-response}



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
