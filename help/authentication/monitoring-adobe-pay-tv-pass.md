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
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#intro}

お客様は、 [ナギオス](http://www.nagios.org) Adobe Pass認証が有効か、無効かを確認する他のツール。

## エンドポイントの監視 {#monitoring-endpoints}

### 監視できるエンドポイント {#endpoints-to-monitor}

* すべてのプラットフォームの設定エンドポイント： `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]`- HTTP または HTTPS（コンテンツプロバイダーの開発者が選択した内容に応じて）で使用できます。 このエンドポイントが見つからない場合は、すべてのプラットフォームとすべての MVPD でコンテンツが使用できなくなります。 クライアントレス REST API の場合は、次のエンドポイントも持ちます。  `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* 次のエンドポイントは、Adobe Pass Authentication Web SDK に含まれています。  これが見つからない場合は、すべてのプログラマーとすべての Web プロパティに対して pay-TVass が停止します。

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### 監視する必要のないエンドポイント {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  このエンドポイントには MVPD SAML 応答が必要なので、常に 503 エラーが発生します。

* その他の使用権限エンドポイント — `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

これらのエンドポイントは、関連する応答のペイロードが必要なので、監視できません。
