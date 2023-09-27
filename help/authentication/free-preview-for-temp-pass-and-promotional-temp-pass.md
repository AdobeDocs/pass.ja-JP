---
title: 一時パスとプロモーション一時パスのフリープレビュー
description: 一時パスとプロモーション一時パスのフリープレビュー
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 1%

---

# 一時パスとプロモーション一時パスのフリープレビュー {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

2 つ目の画面を使用せずに、Temp Pass と Promotion Temp Pass の認証トークンを作成できます。


| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | ストリーミングアプリ</br></br>または</br></br>プログラマーサービス | 1. requestor_id（必須）</br>    </br>2.  deviceId（必須）</br>    </br>3.  mso_id（必須）</br>    </br>4.  domain_name（必須）</br>    </br>5.  device_info/X-Device-Info （必須）</br>6.  deviceType</br>    </br>7.  deviceUser （非推奨）</br>    </br>8.  appId （非推奨）</br>    </br>9.  generic_data（オプション） | POST | 成功した応答は「204 No Content」になり、トークンが正常に作成され、authz フローで使用する準備が整ったことを示します。 | 204 — コンテンツなし   </br>400 — 無効なリクエスト |

<div>


| 入力パラメーター | 説明 |
| --- | --- |
| requestor_id | この操作が有効な ProgrammerRequestorId。 |
| deviceId | デバイス ID バイト。 |
| mso_id | この操作が有効な MVPD ID です。 |
| domain_name | トークンを付与するドメイン名。 認証トークンが付与されている場合、これはサービスプロバイダーのドメインと比較されます。 |
| device_info/</br></br>X-Device-Info | デバイス情報のストリーミング。</br></br>**注意**：この INFO は、URL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGETURL の長さの制限により、HTTP ヘッダーで X-Device-Info として渡す必要があります。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br>このパラメータが正しく設定されている場合、ESM は以下の指標を提供します。 [デバイスタイプ別に分類](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) クライアントレスを使用する場合に、Roku、AppleTV、Xbox など、様々な種類の分析を実行できます。</br></br>詳しくは、 [クライアントレスのデバイスタイプパラメータを使用するメリット&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**:device_info がこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスのユーザー ID。</br></br>**注意**：使用する場合、deviceUser は [登録コードを作成](/help/authentication/registration-code-request.md) リクエスト。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**:device_info がこのパラメーターを置き換えます。 使用する場合、 `appId` は、 [登録コードを作成](/help/authentication/registration-code-request.md) リクエスト。 |
| generic_data | プロモーション一時パスのトークンの範囲を制限するために使用します。 |


### [REST API リファレンスに戻る](/help/authentication/rest-api-reference.md)
