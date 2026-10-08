---
title: Adobe Pass認証の監視
description: Adobe Pass認証の監視
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%
---
# （レガシー）Adobe Pass認証の監視 {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#intro}

お客様は、[Nagios](http://www.nagios.org)またはその他のツールを使用して、Adobe Pass認証が有効か無効かを確認できます。

## エンドポイントの監視 {#monitoring-endpoints}

### 監視できるエンドポイント {#endpoints-to-monitor}

* すべてのプラットフォームの設定エンドポイント：`https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]`- HTTPまたはHTTPSのいずれかで使用できます（コンテンツプロバイダーの開発者が選択した内容に応じて異なります）。 このエンドポイントが見つからない場合は、すべてのプラットフォームとすべてのMVPDでコンテンツを利用できないことを意味します。 クライアントレス REST APIには、次のエンドポイントもあります：`https://api.auth.adobe.com/adobe-services/config your-config-ID]`。

* 以下のエンドポイントは、Adobe Pass Authentication web SDKの一部です。  もし見つからない場合は、有料TVpassがすべてのプログラマーとすべてのウェブプロパティに対してダウンしていることを意味します。

  * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
  * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### 監視できないエンドポイント {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  このエンドポイントにはMVPD SAML応答が必要なため、常に503 エラーが発生します。

* その他の使用権限エンドポイント - `adobe-services/1.0/authenticate/`、`adobe-services/1.0/deviceShortAuthorize`、`adobe-services/1.0/authorize`

これらのエンドポイントは、関連する返信にペイロードが必要なため、監視できません。
