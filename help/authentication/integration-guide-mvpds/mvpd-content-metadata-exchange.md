---
title: MVPD Content Metadata Exchange
description: MVPD Content Metadata Exchange
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%
---
# MVPD Content Metadata Exchange

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#content-metadat-exchange-overview}

このページでは、Adobe Pass Authenticationが認証リクエストで構造化データをMVPDに送信するために使用する2つの標準実装について説明します。  構造化データは、リクエストを行うリソース（プログラマー）と、場合によってはコンテンツレーティングなどの追加データを表します。

プログラマー側では、Adobe Pass Authenticationは構造化MRSS データリソースを次のようにサポートしています。

1. プログラマは、リソースをMRSS文字列として送信します。 Adobe Pass Authenticationは、web デバイスまたはネイティブデバイスのクライアントサイドではエンコードしません。 MRSSは、通常の文字列としてAdobe Pass Authentication Serverに送信されます。
1. サーバー側では、MRSSが定義済みのスキーマ（http://search.yahoo.com/mrss/）に対して検証されます。  検証が合格すると、Adobe Pass AuthenticationはMRSS フィールドから次のような情報を抽出します。
   * チャネルタイトル
   * 項目タイトル
   * リソース識別子
   * 評価の値と種類
1. MRSSから抽出された値は、MVPDに渡される認証リクエストを構築するために使用されます。

Adobe Pass Authenticationでは、MRSSをMVPDでサポートされる形式に変換する際に、次の2つのアプローチがサポートされています。

* **XACML**。  1つ目のアプローチは、OLCA標準と一致しています。  これは、MRSS値を抽出して、MRSS要素にマッピングする属性を持つXACMLResourceを構築するXACMLを使用します。  これをMVPDに渡します。
* **REST**。  2つ目のアプローチは、REST ベースのアプローチです。  MRSSはbase64でエンコードされ、REST呼び出しのURL パラメーターとして渡されます。

どちらのアプローチでも、MVPDは、抽出された値を独自の論理フロー内に含め、認証リクエストを処理し、認証レスポンスを返します。

## 統合の詳細 {#integration-details}

* OLCA ベースのXACML構造化リソース
* REST ベースの構造化リソース

### OLCA ベースのXACML構造化リソース {#olca-based-xacml-struc-resource}

ほとんどのケーブル指向MVPDは、XACML ベースのアプローチを使用していますが、完全な構造化データアプローチはまだサポートしていません。  XACMLをサポートするその他のMVPDは、チャネルタイトルを取得し、それをResourceID属性に受け入れます。 次の例は、構造化XACML ベースの完全なアプローチを示しています。 Adobe Pass Authentication チームは、XACMLを使用しているが、まだペアレンタルコントロールなどの機能をサポートしていないMVPDの場合、XACML統合を次の例に適応させることをお勧めします。

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

一部のMVPDは、次のREST ベースのプロトコルで認証のために標準化されています。 このアプローチは、XACML アプローチと同様にフル機能ですが、「軽量」実装を提供します。

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
