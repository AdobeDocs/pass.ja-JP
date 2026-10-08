---
title: ユーザーメタデータ
description: ユーザーメタデータ
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%
---
# （レガシー） ユーザーメタデータ {#user-metadata}

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

`<REGGIE_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

MVPDが認証済みユーザーについて共有したメタデータを取得します。


| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者</br>2。  deviceId （必須） </br>3。  device_info/X-Device-Info （必須） </br>4.  deviceType</br>5。  deviceUser （非推奨） </br>6。  appId （非推奨） | GET | 失敗した場合のユーザーのメタデータまたはエラーの詳細を含むXMLまたはJSON。 | 200 – 成功<p>404 - メタデータが見つかりません<p>412 – 無効なAuthN トークン （期限切れのトークンなど） |


| 入力パラメーター | 説明 |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| device_info/<p>X-Device-Info | ストリーミング デバイス情報。</br></br> **注意：**&#x200B;これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、http ヘッダーのX-Device-Infoとして渡す必要があります。</br></br> 詳しくは、[&#x200B; デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| _deviceType_ | デバイスの種類（Roku、PCなど）。</br></br> このパラメーターが正しく設定されている場合、ESMはクライアントレスを使用する際にデバイスタイプ [&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics)ごとに分割された指標を提供します。これにより、Roku、AppleTV、Xboxなどのさまざまなタイプの分析を実行できます。</br></br> 「[&#x200B; パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br>」を参照してください **注：** `device_info`はこのパラメーターに置き換わります。 |
| _deviceUser_ | デバイス ユーザーID。</br></br> **メモ：**&#x200B;使用する場合、`deviceUser`は[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。</br></br> **注：** `device_info`はこのパラメーターに置き換わります。 使用する場合、`appId`は、[登録コードの作成](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求と同じ値を持つ必要があります。 |

>[!NOTE]
> 
>ユーザーメタデータ情報は、認証フローが完了した後に使用できますが、MVPDおよびメタデータタイプに応じて、認証フローで更新できます。




## 応答サンプル {#sample-response}

呼び出しが成功すると、サーバーはXML （デフォルト）またはJSON オブジェクトを使用して、以下のような構造で応答します。


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

オブジェクトのルートには3つのノードがあります。

* *updated*: メタデータが最後に更新された時刻を表すUNIX タイムスタンプを指定します。 このプロパティは、認証フェーズでメタデータを生成する際に、サーバーによって最初に設定されます。 その後の呼び出し（メタデータが更新された後）は、タイムスタンプが増分されます。
* *data*：実際のメタデータ値が含まれています。
* *encrypted*：暗号化されたプロパティを一覧表示する配列。 特定のメタデータ値を復号するには、プログラマはメタデータに対してBase64復号を実行し、その結果の値に対して独自の秘密鍵を使用してRSA復号を適用する必要があります（Adobeは、プログラマの公開証明書を使用してサーバ上のメタデータを暗号化します）。

エラーが発生した場合、サーバーは詳細なエラーメッセージを指定するXMLまたはJSON オブジェクトを返します。

詳しくは、[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)を参照してください。

[REST API リファレンスに戻る](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
