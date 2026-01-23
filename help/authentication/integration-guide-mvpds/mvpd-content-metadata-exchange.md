---
title: MVPD コンテンツメタデータ交換
description: MVPD コンテンツメタデータ交換
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# MVPD コンテンツメタデータ交換

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#content-metadat-exchange-overview}

ここでは、Adobe Pass Authentication が Authorization リクエストで構造化データを MVPD に送信する際に使用する 2 つの標準実装について説明します。  構造化されたデータは、リクエストを行うリソース（プログラマー）と、場合によっては、コンテンツ評価などの追加データを表します。

プログラマー側では、Adobe Pass認証は、次のような構造化 MRSS データリソースをサポートします。

1. プログラマが Resource を MRSS 文字列として送信します。 Adobe Pass認証では、web またはネイティブデバイス用に、クライアント側でエンコードされません。 MRSS は通常の文字列としてAdobe Pass Authentication Server に送信されます。
1. サーバー側では、MRSS は事前定義済みのスキーマ（http://search.yahoo.com/mrss/）に対して検証されます。  検証に合格すると、Adobe Pass認証によって MRSS フィールドから次のような情報が抽出されます。
   * チャネルタイトル
   * 項目のタイトル
   * リソース識別子
   * 評価値とタイプ
1. MRSS から抽出された値は、MVPDに渡される認証リクエストの作成に使用されます。

Adobe Pass認証で MRSS を MVPD でサポートされる形式に変換するには、次の 2 つの方法があります。

* **XACML**。  1 つ目のアプローチは OLCA 規格に準拠しています。  MRSS 値が抽出される XACML を使用して、MRSS 要素にマッピングされる属性を持つ XACMLResource を作成します。  これは、その後、MVPDに渡されます。
* **休む**。  2 つ目のアプローチは REST ベースです。  MRSS は base64 でエンコードされ、REST 呼び出しで URL パラメーターとして渡されます。

どちらの方法でも、MVPDは、抽出された値を独自の論理フロー内に含め、認証応答を返すことによって、認証リクエストを処理します。

## 統合の詳細 {#integration-details}

* OLCA ベースの XACML 構造化リソース
* REST ベースの構造化リソース

### OLCA ベースの XACML 構造化リソース {#olca-based-xacml-struc-resource}

ほとんどのケーブル指向 MVPD は XACML ベースのアプローチを使用していますが、まだ完全な構造化データアプローチをサポートしていません。  XACML をサポートする他の MVPD は、チャネルタイトルを受け取り、ResourceID 属性に受け入れます。 完全な構造化 XACML ベースのアプローチの例を以下に示します。 Adobe Pass Authentication チームは、XACML を使用しても、ペアレンタルコントロールなどの機能をまだサポートしていない MVPD の場合、XACML 統合を次の例に適合させることをお勧めします。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### REST ベースの構造化リソース {#rest-based-struct-resource}

一部の MVPD は、次の REST ベースのプロトコルに基づいて認証を標準化しています。 このアプローチは XACML アプローチと同様に完全な機能を備えていますが、「軽量」な実装を提供します。

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
