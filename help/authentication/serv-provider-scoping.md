---
title: サービスプロバイダー範囲
description: サービスプロバイダー範囲
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# サービスプロバイダー範囲 {#service-provoider-scoping}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

MVPD とのAdobe Pass Authentication 統合のデフォルトの実装は、 **OLCA 仕様**. OLCA 仕様（6.5、サブジェクト識別子）の認証要件の節では、サブジェクト識別子のサービスプロバイダ (SP) の範囲を示すことが可能であると述べています。 （件名識別子は、MVPD が SP に返す不明化されたユーザー ID です）。  Adobe Pass認証統合では、MVPDs が SP 認証要求のスコーピングを有効にする必要があります。

Adobe Pass Authentication がプログラマーの SP の役割を引き継ぐ場合は、SP の認証要求のスコープを有効にするカスタマイズを実装する必要があります。  これは、MVPD が SAML アサーションで MVPD の ID プロバイダー (IdP) に渡されるネットワークブランドを識別できるようにするために行う必要があります。  スコーピングは、次の節で説明する 2 つの方法のいずれかで実装できます。

## サービスプロバイダー範囲 {#service-provider-scoping}

Adobe Pass認証では、次の 2 つの方法で認証要求の SP スコープを有効にできます。

* **SAML 発行者のアプローチです。**  このアプローチでは、「リクエスト元 ID」が SAML 認証リクエストの SAML 発行者文字列に追加されます。

* **カスタムスコーピングプロパティのアプローチです。**  このアプローチでは、「Requestor ID」が SAML 認証リクエストのカスタム「Scoping」プロパティとして明示的に含まれます。

>[!NOTE]
>
>「要求者 ID」は、Adobe Pass認証がプログラマーのネットワークブランドを指す方法です（例えば、「CNN」はターナーネットワークのブランドの 1 つです）。

### SAML 発行者アプローチ {#saml-issuer-approach}

このアプローチでは SAML を使用します `<Issuer>` 要素を含める必要があります。

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### カスタムスコーピングプロパティのアプローチ {#custom-scoping-property-approach}

この方法では、SAML 認証要求の次のスニペットに示すように、「Scoping」という名前のカスタムプロパティを使用します。

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
