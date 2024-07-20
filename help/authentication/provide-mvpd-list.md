---
title: MVPD リストの提供
description: MVPD リストの提供
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# MVPD リストの提供 {#provide-mvpd-list}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

リクエスターに設定された MVPD のリストを返します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br> 例：</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass 認証 | 1.依頼者 </br>    （パスコンポーネント） </br>_2.  deviceType （非推奨）_ | GET | MVPD のリストを含む XML または JSON。 | 200 |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --------------- | ------------------------------------------------------------- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| *deviceType* | デバイスタイプ。 |

{style="table-layout:auto"}

### 応答のサンプル {#sample-response}

/config サーブレットへの既存の MVPD XML 応答と同じ

メモ：Platform SSO を使用するように設定されたすべての MVPD では、対応するノード（JSON/XML）内に次の追加のプロパティが含まれます。

* **enablePlatformServices （boolean）：この MVPD が Platform SSO 経由で統合されているかどうかを示す** フラグ
* **boardingStatus （string）:** が MVPD が Platform SSO を完全にサポートしているか（サポート対象）、または MVPD がプラットフォームピッカーにのみ表示されているかを示すフラグ
* **displayInPlatformPicker （boolean）：この MVPD がプラットフォームピッカーに表示される** 要があります
* **platformMappingId （string）:** プラットフォームで認識されるこの MVPD の識別子
* **requiredMetadataFields （文字列配列）:** ログインが成功した場合に使用できるユーザーメタデータフィールド
