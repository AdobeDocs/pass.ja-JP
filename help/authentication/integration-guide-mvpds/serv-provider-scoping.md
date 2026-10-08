---
title: サービスプロバイダースコーピング
description: サービスプロバイダースコーピング
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%
---
# サービスプロバイダースコーピング {#service-provoider-scoping}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

MVPDとのAdobe Pass Authentication統合のデフォルトの実装は、**OLCA Specification**&#x200B;に基づいています。 OLCA仕様（6.5、Subject Identifier）の「認証要件」セクションでは、Subject Identifierに対するサービスプロバイダー（SP）の範囲を示すことが可能であると述べています。 （件名IDは、MVPDがSPに返す難読化されたユーザーIDです）。  Adobe Pass認証の統合では、MVPDでSP認証リクエストのスコープを有効にする必要があります。

Adobe Pass認証がプログラマのSPの役割を引き受ける場合、認証リクエストのSP スコープを有効にするカスタマイズを実装する必要があります。  これは、MVPDがMVPDのID プロバイダー（IdP）にSAML アサーションで渡されるネットワークブランドを識別できるように行う必要があります。  スコーピングは、次の節で説明する2つの方法のいずれかで実装できます。

## サービスプロバイダースコーピング {#service-provider-scoping}

Adobe Pass Authenticationでは、次の2つの方法でAuthentication リクエストのSP スコーピングを有効にできます。

* **SAML イシュア アプローチ。**  この方法では、SAML認証リクエストのSAML イシュア文字列に「依頼者ID」が追加されます。

* **カスタムスコーププロパティのアプローチ。**  このアプローチでは、「依頼者ID」は、SAML認証リクエストのカスタム「スコーピング」プロパティとして明示的に含まれます。

>[!NOTE]
>
>「依頼者ID」は、Adobe Pass認証がプログラマーのネットワークブランドを指す方法です（例：「CNN」はターナーネットワークのブランドの1つです）。

### SAML発行者アプローチ {#saml-issuer-approach}

このアプローチでは、次のスニペットに示すように、SAML認証リクエストのSAML `<Issuer>`要素を使用します。

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### カスタムスコーププロパティアプローチ {#custom-scoping-property-approach}

このアプローチでは、SAML認証リクエストのこのスニペットに示すように、「スコーピング」という名前のカスタムプロパティを使用します。

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
