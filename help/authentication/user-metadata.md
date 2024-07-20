---
title: ユーザーメタデータ
description: ユーザーメタデータ
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# ユーザーメタデータ {#user-metadata}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

`<REGGIE_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

MVPD が認証済みユーザーに関して共有したメタデータを取得します。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1. リクエスタ </br>2.  deviceId （必須） </br>3.  device_info/X-Device-Info （必須） </br>4.  deviceType</br>5.  deviceUser （非推奨） </br>6.  appId （非推奨） | GET | 失敗した場合に、ユーザーメタデータまたはエラーの詳細を含む XML または JSON。 | 200 – 成功<p>404 - メタデータが見つかりません<p>412 – 無効な AuthN トークン（期限切れのトークンなど） |


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| device_info/<p>X-Device-Info | ストリーミングデバイス情報。</br></br> **注意：** これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br></br> 詳しくは、「デバイスと接続の情報を渡す [ を参照してください ](/help/authentication/passing-client-information-device-connection-and-application.md)。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br></br> このパラメーターが正しく設定されている場合、ESM では、クライアントレスの使用時に [ デバイスタイプごとに分類 ](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) される指標を提供し、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できるようにします。</br></br> しくは、[ パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) を参照してください </br></br> **メモ：** このパラメーターは `device_info` で置き換えられます。 |
| _deviceUser_ | デバイスユーザー ID。</br></br> **メモ：** 使用する場合、`deviceUser` は [ 登録コードの作成 ](/help/authentication/registration-code-request.md) リクエストと同じ値を持つ必要があります。 |
| _appId_ | アプリケーション ID/名前。</br></br> **メモ：** このパラメーターは `device_info` で置き換えられます。 `appId` を使用する場合は、[ 登録コードの作成 ](/help/authentication/registration-code-request.md) リクエストと同じ値を指定する必要があります。 |

>[!NOTE]
> 
>ユーザーメタデータ情報は、認証フローが完了した後で使用できますが、MVPD およびメタデータタイプに応じて、認証フローで更新できます。




## 応答のサンプル {#sample-response}

呼び出しが成功すると、サーバーは、以下に示すような構造を持つ XML （デフォルト）または JSON オブジェクトで応答します。


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

オブジェクトのルートには、次の 3 つのノードがあります。

* *updated*：メタデータが最後に更新された時刻を表す UNIX タイムスタンプを指定します。 このプロパティは、認証フェーズでメタデータを生成するときに、サーバーによって最初に設定されます。 （メタデータを更新した後の）後続の呼び出しでは、タイムスタンプが増分されます。
* *data*：実際のメタデータ値を格納します。
* *encrypted*：暗号化されたプロパティをリストする配列。 特定のメタデータ値を復号化するには、プログラマーはメタデータに対して Base64 デコードを実行し、結果の値に対して独自の秘密鍵を使用して RSA 復号化を適用する必要があります（Adobeは、プログラマーの公開証明書を使用してサーバー上のメタデータを暗号化します）。

エラーが発生した場合、サーバーは詳細なエラーメッセージを指定する XML または JSON オブジェクトを返します。

詳しくは、[ ユーザーメタデータ ](/help/authentication/user-metadata-feature.md) を参照してください。

[REST API リファレンスに戻る](/help/authentication/rest-api-reference.md)
