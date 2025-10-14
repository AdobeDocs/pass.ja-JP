---
title: MVPDリストを指定
description: MVPDリストを指定
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 2%

---

# （従来の）MVPDリストの提供 {#provide-mvpd-list}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

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

/config サーブレットに対する既存のMVPD XML 応答と同様

メモ：Platform SSO を使用するように設定されたすべての MVPD では、対応するノード（JSON/XML）内に次の追加のプロパティが含まれます。

* **enablePlatformServices （boolean）：このMVPDが Platform SSO で統合されるかどうかを示す** フラグ
* **boardingStatus （string）:MVPDが Platform SSO を完全にサポートしているか（サポート対象）、MVPDがプラットフォームピッカーにのみ表示されるかを示す** フラグ
* **displayInPlatformPicker （ブール値）：このMVPD** プラットフォームピッカーに表示されるかどうか
* **platformMappingId （string）:** プラットフォームで認識されるこのMVPDの識別子
* **requiredMetadataFields （文字列配列）:** ログインが成功した場合に使用できるユーザーメタデータフィールド
