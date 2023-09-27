---
title: MVPD ログアウト
description: MVPD ログアウト
exl-id: a2b57d02-9688-48e3-beff-1012cd361d0c
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 0%

---

# MVPD ログアウト

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

ログアウトの使用例は、IdP に送信される SAML ログアウトリクエストで実装するか、呼び出されるカスタムログアウトエンドポイントで実装できます。  以下のリクエストと応答の例は、SAML ログアウト実装のサンプルを示しています。

## ログアウト要求のサンプル {#sample-logout-request}

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:LogoutRequest
        Destination="https://idpcom/slo" ID="_b18de1a2-5bc2-4866-88e3-97a9cbb663ca"
        IssueInstant="2010-08-18T07:18:22.479Z"
        Reason="urn:oasis:names:tc:SAML:2.0:logout:user"
        Version="2.0"
        xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer>https://saml.sp.auth.adobe.com</saml:Issuer>
    <saml:NameID
        Format="urn:oasis:names:tc:SAML:2.0:nameid format:transient">
            DkCwM54kzVs5coXH0R0IFhPSA9a
    </saml:NameID>
    <samlp:SessionIndex>L4EQmlLCIS-5hHr71z1VfOCEYWk</samlp:SessionIndex>
</samlp:LogoutRequest>
```

## ログアウト応答のサンプル {#sample-logout-response}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<samlp:LogoutResponse
        Destination="https://sp.auth-staging.adobe.com/sp/saml/LogoutServiceHTTPRedirectResponse"
        ID="XpsdJNdPp7PEnNxfeBubP.w4e3r"
        InResponseTo="_b18de1a2-5bc2-4866-88e3-97a9cbb663ca"
        IssueInstant="2010-08-18T07:18:24.969Z" Version="2.0"
        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer  
        xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://idp.com/slo
    </saml:Issuer>
    <samlp:Status>
        <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
</samlp:LogoutResponse>
```

<!--
>[!RELATEDINFORMATION]
>* [Content Metadata Exchange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [Preflight Authorization](/help/authentication/mvpd-preflight-authz.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
-->
