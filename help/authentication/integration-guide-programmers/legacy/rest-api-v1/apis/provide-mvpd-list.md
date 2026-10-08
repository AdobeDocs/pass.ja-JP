---
title: MVPD リストの提供
description: MVPD リストの提供
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 2%
---
# （従来）MVPD リストの提供 {#provide-mvpd-list}

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

要求者に対して設定されたMVPDのリストを返します。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>例：</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass 認証 | &#x200B;1.  依頼者</br> （パスコンポーネント） </br>_2。  deviceType （非推奨）_ | GET | MVPDのリストを含むXMLまたはJSON。 | 200 |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --------------- | ------------------------------------------------------------- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| *deviceType* | デバイスタイプ： |

{style="table-layout:auto"}

### 応答サンプル {#sample-response}

/config サーブレットへの既存のMVPD XML Responseと同じ

注：Platform SSOを使用するように設定されたすべてのMVPDには、対応するノード（JSON/XML）内に次の追加プロパティが含まれます。

* **enablePlatformServices （ブール値）:** フラグは、このMVPDがPlatform SSO経由で統合されているかどうかを示します
* **boardingStatus （文字列）:** MVPDがPlatform SSO （サポート）を完全にサポートしているか、またはMVPDがPlatform ピッカー（ピッカー）にのみ表示されているかどうかを示すフラグ
* **displayInPlatformPicker （ブール値）:**&#x200B;このMVPDがプラットフォームピッカーに表示される場合
* **platformMappingId （文字列）:** プラットフォームで既知であるこのMVPDの識別子
* **requiredMetadataFields （文字列配列）:**&#x200B;正常なログインでユーザーメタデータフィールドが使用可能であることが期待されています
