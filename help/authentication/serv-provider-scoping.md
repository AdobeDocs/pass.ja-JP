---
title: サービスプロバイダーのスコーピング
description: サービスプロバイダーのスコーピング
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# サービスプロバイダーのスコーピング {#service-provoider-scoping}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

MVPD を使用したAdobe Pass Authentication Integration のデフォルトの実装は、**OLCA 仕様** に基づいています。 OLCA 仕様（6.5、サブジェクト識別子）の「認証要件」セクションには、サブジェクト識別子のサービスプロバイダー（SP）のスコープを示すことが可能であることが記載されています。 （サブジェクト識別子は、難読化された MVPD が SP に返すユーザー ID です。）  Adobe Pass Authentication Integration では、MVPD によって SP Authentication 要求のスコーピングが可能になっている必要があります。

Adobe Pass Authentication がプログラマーの SP の役割を担う場合、認証リクエストの SP スコーピングを有効にするカスタマイズを実装する必要があります。  これは、MVPD が、SAML アサーションで MVPD の ID プロバイダー（IdP）に渡されたネットワークブランドを識別できるようにするために行う必要があります。  スコーピングは、次の節で説明する 2 つの方法のいずれかで実装できます。

## サービスプロバイダーのスコーピング {#service-provider-scoping}

Adobe Pass Authentication は、Authentication リクエストの SP スコーピングを有効にする次の 2 つの方法をサポートしています。

* **SAML 発行者アプローチ。** このアプローチでは、「リクエスター ID」が SAML 認証リクエストの SAML 発行者文字列に追加されます。

* **カスタムスコーピングプロパティアプローチ。** このアプローチでは、「リクエスター ID」が SAML 認証リクエストのカスタム「スコーピング」プロパティとして明示的に含まれます。

>[!NOTE]
>
>「リクエスター ID」は、Adobe Pass Authentication がプログラマーのネットワークブランドをどのように参照するかです（例：「CNN」は Turner Network のブランドの 1 つです）。

### SAML 発行者アプローチ {#saml-issuer-approach}

このアプローチでは、次のスニペットに示すように、SAML 認証リクエストで SAML `<Issuer>` 要素を使用します。

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### カスタムスコーピングプロパティアプローチ {#custom-scoping-property-approach}

このアプローチでは、「Scoping」という名前のカスタムプロパティを使用します（SAML 認証リクエストのこのスニペットを参照）。

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
