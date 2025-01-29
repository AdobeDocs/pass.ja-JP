---
title: Temp Pass とプロモーション Temp Pass の無料プレビュー
description: Temp Pass とプロモーション Temp Pass の無料プレビュー
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# （レガシー）一時パスとプロモーション一時パスの無料プレビュー {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

2 番目の画面を必要とせずに、一時パスおよびプロモーション一時パスの認証トークンを作成できます。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1. requestor_id （必須） </br>    </br>2。  deviceId （必須） </br>    </br>3。  mso_id （必須） </br>    </br>4。  domain_name （必須） </br>    </br>5。  device_info/X-Device-Info （必須） </br>6.  deviceType</br>    </br>7。  deviceUser （非推奨） </br>    </br>8。  appId （非推奨） </br>    </br>9。  generic_data （オプション） | POST | 正常な応答は「204 No Content」になります。これは、トークンが正常に作成され、authz フローで使用する準備が整ったことを示します。 | 204 - コンテンツなし   </br>400 – 無効なリクエスト |

<div>


| 入力パラメーター | 説明 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| mso_id | この操作が有効なMVPD ID。 |
| domain_name | トークンが付与されるドメイン名。 これは、認証トークンが付与される際のサービスプロバイダーのドメインと比較されています。 |
| device_info/</br></br>X-Device-Info | ストリーミングデバイス情報。</br></br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br></br> 詳しくは、「デバイスと接続情報の受け渡し [ を参照してください ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターが正しく設定されている場合、ESM では、クライアントレスの使用時に [ デバイスタイプごとに分類 ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) される指標を提供し、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できるようにします。</br></br>[ クライアントレスデバイスタイプパラメーターを使用するメリット ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意** を参照してください。device_info はこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。</br></br>**注意**：使用する場合、deviceUser は [ 登録コードの作成 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**：このパラメーターは device_info に置き換えられます。 `appId` を使用する場合は、[ 登録コードの作成 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を指定する必要があります。 |
| generic_data | プロモーションの一時パスのトークンの範囲を制限するために使用されます。 |


### [REST API リファレンスに戻る ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
