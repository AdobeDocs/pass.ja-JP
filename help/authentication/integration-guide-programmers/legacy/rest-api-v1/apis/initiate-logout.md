---
title: ログアウトの開始
description: ログアウトを開始
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 1%
---
# （レガシー） ログアウトの開始 {#initiate-logout}

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

ストレージからAuthN トークンとAuthZ トークンを削除します。


| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者</br>2。  deviceId （必須） </br>3。  device_info/X-Device-Info （必須） </br>4.  _deviceType_</br> 5。  _deviceUser_ （非推奨） </br>6。  _appId_ （非推奨） | DELETE | なし | 204 |


| 入力パラメーター | 説明 |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、HTTP ヘッダーでX-Device-Infoとして渡す必要があります。 </br></br>詳細については、[&#x200B; デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>このパラメーターが正しく設定されている場合、ESMは、クライアントレスを使用する場合にデバイスの種類[&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)ごとに分割された指標を提供するため、Roku、AppleTV、Xboxなどのさまざまなタイプの分析を実行できます。</br></br> パス指標でクライアントレスのデバイスタイプパラメーターを使用するメリット [を参照してください&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注**:device_infoはこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイス ユーザー識別子。</br></br>**注**：使用する場合、deviceUserは、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**メモ**:device_infoがこのパラメーターに置き換わります。 使用する場合、`appId`は、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求と同じ値を持つ必要があります。 |

>[!IMPORTANT]
> 
>ログアウト呼び出しには現在、次の制限があります。ストレージからAuthN トークンとAuthZ トークンをクリアしますが（例：プログラマー/Adobe Pass認証側）、**はMVPD ログアウトエンドポイントを呼び出しません**。
