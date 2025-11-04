---
title: ログアウトの開始
description: ログアウトの開始
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# （レガシー）ログアウトの開始 {#initiate-logout}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

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

ストレージから AuthN および AuthZ トークンを削除します。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | &#x200B;1. リクエスタ </br>2.  deviceId （必須） </br>3.  device_info/X-Device-Info （必須） </br>4.  _deviceType_</br> 5.  _deviceUser_ （非推奨） </br>6.  _appId_ （非推奨） | DELETE | なし | 204 |


| 入力パラメーター | 説明 |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br></br> 詳しくは、「デバイスと接続情報の受け渡し [ を参照してください ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターが正しく設定されている場合、ESM では、クライアントレスの使用時に [ デバイスタイプごとに分類 ](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) される指標を提供し、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できるようにします。</br></br>[ パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意** を参照：device_info はこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。</br></br>**注意**：使用する場合、deviceUser は [ 登録コードの作成 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**：このパラメーターは device_info に置き換えられます。 `appId` を使用する場合は、[ 登録コードの作成 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を指定する必要があります。 |

>[!IMPORTANT]
> 
>現在、ログアウト呼び出しには次の制限があります。ストレージ（プログラマー/Adobe Pass認証側）から AuthN トークンと AuthZ トークンをクリアするが、MVPD ログアウトエンドポイントは呼び出さない ****。
