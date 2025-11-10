---
title: Adobe Pass認証の監視
description: Adobe Pass認証の監視
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# （従来の）Adobe Pass認証の監視 {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#intro}

[Nagios](http://www.nagios.org) やその他のツールを使用して、Adobe Pass認証がアップまたはダウンしているかどうかを確認できます。

## エンドポイントの監視 {#monitoring-endpoints}

### 監視可能なエンドポイント {#endpoints-to-monitor}

* すべてのプラットフォームの設定エンドポイント：`https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]` - HTTP または HTTPS で使用できます（コンテンツプロバイダーの開発者の選択によって異なります）。 このエンドポイントがない場合は、コンテンツがすべてのプラットフォームとすべての MVPD で使用できるわけではありません。 クライアントレス REST API の場合、エンドポイント `https://api.auth.adobe.com/adobe-services/config your-config-ID]` もあります。

* 次のエンドポイントは、Adobe Pass認証 web SDKの一部です。  ない場合は、すべてのプログラマーおよびすべての web プロパティに対して有料 TVpass がダウンしていることを意味します。

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### 監視すべきでないエンドポイント {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  このエンドポイントにはMVPD SAML 応答が必要なので、常に 503 エラーが発生します。

* その他の使用権限エンドポイント - `adobe-services/1.0/authenticate/`、`adobe-services/1.0/deviceShortAuthorize`、`adobe-services/1.0/authorize`

これらのエンドポイントは、関連する返信のペイロードが必要なので、監視できません。
