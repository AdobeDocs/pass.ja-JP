---
title: Temp Passおよびプロモーション Temp Passの無料プレビュー
description: Temp Passおよびプロモーション Temp Passの無料プレビュー
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 689e2f86550d9fa59337c15dd38767975a1d6d30
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 2%

---

# （レガシー）一時パスとプロモーション一時パスの無料プレビュー {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

2つ目の画面を必要とせずに、Temp Passおよびプロモーションテンプレートパスの認証トークンを作成できます。


| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  requestor_id （必須） </br>    </br>2.  deviceId （必須） </br>    </br>3.  mso_id （必須） </br>    </br>4.  domain_name （必須） </br>    </br>5.  device_info/X-Device-Info （必須） </br>6.  deviceType</br>    </br>7.  deviceUser （非推奨） </br>    </br>8.  appId （非推奨） </br>    </br>9.  generic_data （オプション） | 投稿する | 応答が成功すると、トークンが正常に作成され、認証フローに使用する準備ができていることを示す204 No Contentになります。 | 204 - コンテンツなし</br>400 – 不正なリクエスト |

<div>


| 入力パラメーター | 説明 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| mso_id | この操作が有効なMVPD ID。 |
| domain_name | トークンが付与されるドメイン名。 これは、認証トークンが付与されているときに、サービスプロバイダーのドメインと比較されます。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、HTTP ヘッダーでX-Device-Infoとして渡す必要があります。 </br></br>詳細については、[ デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br>このパラメーターが正しく設定されている場合、ESMは、クライアントレスを使用する場合にデバイスの種類](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)ごとに[分割された指標を提供するため、Roku、AppleTV、Xboxなどのさまざまなタイプの分析を実行できます。</br></br>詳しくは、[ クライアントレスのデバイスタイプパラメーターを使用するメリット ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注**&#x200B;を参照してください。device_infoはこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイス ユーザー識別子。</br></br>**注**：使用する場合、deviceUserは、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**メモ**:device_infoがこのパラメーターに置き換わります。 使用する場合、`appId`は、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求と同じ値を持つ必要があります。 |
| generic_data | プロモーションテンプパスのトークンの範囲を制限するために使用します。 |


**[REST API リファレンスに戻る](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)**
