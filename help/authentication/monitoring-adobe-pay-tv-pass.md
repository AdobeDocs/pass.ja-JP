---
title: Adobe Pass認証の監視
description: Adobe Pass認証の監視
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Adobe Pass認証の監視 {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#intro}

[Nagios](http://www.nagios.org) やその他のツールを使用して、Adobe Pass認証がアップまたはダウンしているかどうかを確認できます。

## エンドポイントの監視 {#monitoring-endpoints}

### 監視可能なエンドポイント {#endpoints-to-monitor}

* すべてのプラットフォームの設定エンドポイント：`https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]` - HTTP または HTTPS で使用できます（コンテンツプロバイダーの開発者の選択によって異なります）。 このエンドポイントがない場合は、コンテンツがすべてのプラットフォームとすべての MVPD で使用できるわけではありません。 クライアントレス REST API の場合、エンドポイント `https://api.auth.adobe.com/adobe-services/config your-config-ID]` もあります。

* 次のエンドポイントは、Adobe Pass Authentication web SDK の一部です。  ない場合は、すべてのプログラマーおよびすべての web プロパティに対して有料 TVpass がダウンしていることを意味します。

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### 監視すべきでないエンドポイント {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  このエンドポイントには MVPD SAML 応答が必要なので、常に 503 エラーが発生します。

* その他の使用権限エンドポイント - `adobe-services/1.0/authenticate/`、`adobe-services/1.0/deviceShortAuthorize`、`adobe-services/1.0/authorize`

これらのエンドポイントは、関連する返信のペイロードが必要なので、監視できません。
