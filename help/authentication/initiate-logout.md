---
title: ログアウトを開始
description: ログアウトを開始
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# ログアウトを開始 {#initiate-logout}

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

ストレージから AuthN および AuthZ トークンを削除します。


| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/logout | ストリーミングアプリ</br></br>または</br></br>プログラマーサービス | (1) 請求者</br>2.  deviceId（必須）</br>3.  device_info/X-Device-Info （必須）</br>4.  _deviceType_</br> 5.  _deviceUser_ （廃止）</br>6.  _appId_ （廃止） | DELETE | なし | 204 |


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| deviceId | デバイス ID バイト。 |
| device_info/</br></br>X-Device-Info | デバイス情報のストリーミング。</br></br>**注意**：この INFO は、URL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGETURL の長さの制限により、HTTP ヘッダーで X-Device-Info として渡す必要があります。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br>このパラメータが正しく設定されている場合、ESM は以下の指標を提供します。 [デバイスタイプ別に分類](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) クライアントレスを使用する場合に、Roku、AppleTV、Xbox など、様々な種類の分析を実行できます。</br></br>詳しくは、 [パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**:device_info がこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスのユーザー ID。</br></br>**注意**：使用する場合、deviceUser は [登録コードを作成](/help/authentication/registration-code-request.md) リクエスト。 |
| _appId_ | アプリケーション ID/名前。 </br></br>**注意**:device_info がこのパラメーターを置き換えます。 使用する場合、 `appId` は、 [登録コードを作成](/help/authentication/registration-code-request.md) リクエスト。 |

>[!IMPORTANT]
> 
>ログアウト呼び出しには現在、次の制限があります。AuthN および AuthZ トークンをストレージからクリアします ( つまり、プログラマー/Adobe Pass認証側 )。 **次の値と等しくない** MVPD ログアウトエンドポイントを呼び出します。
