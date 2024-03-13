---
title: MVPD リストを提供
description: MVPD リストを提供
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# MVPD リストを提供 {#provide-mvpd-list}

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

リクエスト元の設定済み MVPD のリストを返します。

| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>例：</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestorId | Adobe Pass 認証 | 1.請求者</br>    （パスコンポーネント）</br>_2.  deviceType（非推奨）_ | GET | MVPD のリストを含む XML または JSON。 | 200 |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --------------- | ------------------------------------------------------------- |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| *deviceType* | デバイスタイプ。 |

{style="table-layout:auto"}

### レスポンスのサンプル {#sample-response}

既存の MVPD XML 応答/config サーブレットと同じ

注意： Platform SSO を使用するように設定されたすべての MVPD は、対応するノード (JSON/XML) 内に次の追加のプロパティを持ちます。

* **enablePlatformServices(boolean):** この MVPD が Platform SSO を介して統合されているかどうかを示すフラグ
* **boardingStatus （文字列）:** MVPD が Platform SSO(SUPPORTED) を完全にサポートするか、MVPD がプラットフォームピッカー (PICKER) にのみ表示されるかを示すフラグ
* **displayInPlatformPicker（ブール値）:** この MVPD がプラットフォームピッカーに表示される場合
* **platformMappingId（文字列）:** プラットフォームで知られている、この MVPD の識別子
* **requiredMetadataFields（文字列配列）:** ログインが成功した場合に使用できるユーザーメタデータフィールド
