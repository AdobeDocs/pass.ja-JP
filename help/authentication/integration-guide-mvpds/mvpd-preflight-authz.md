---
title: MVPD Preflight Authorization
description: MVPD Preflight Authorization
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# MVPD Preflight Authorization

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#mvpd-preflight-authz-intro}

「プリフライト認証」は、複数のリソースに対する軽量の認証チェックです。 プログラマーは主に UI を装飾するためにこの機能を使用します（例えば、ロックやロック解除のアイコンを使用してアクセスステータスを示します）。

現在、Adobe Pass認証は、MVPD に対して、AuthN 応答属性またはマルチチャネル AuthZ リクエストの 2 つの方法でプリフライト認証をサポートできます。  次のシナリオは、プリフライト認証を実装する様々な方法のコストとメリットを説明しています。

* **ベストケースのシナリオ** - MVPDは、承認フェーズ（マルチチャネル AuthZ）で事前承認されたリソースのリストを提供します。
* **最悪のシナリオ** - MVPDが複数リソース認証をまったくサポートしていない場合、Adobe Pass認証サーバーはリソースリストの各リソースに対してMVPDへの認証コールを実行します。 このシナリオは、プリフライト認証リクエストの応答時間に（リソース数に比例して）影響を与えます。 Adobe サーバーとMVPD サーバーの両方の負荷が増え、パフォーマンスの問題が発生する可能性があります。 また、実際に再生を行う必要なく、承認リクエスト/応答イベントが生成されます。
* **非推奨** - MVPDは、認証段階で事前承認済みリソースのリストを提供します。リストはクライアントにキャッシュされるので、プリフライトリクエストでさえも、ネットワーク呼び出しは必要ありません。

MVPD はプリフライト認証をサポートする必要はありませんが、以下の節では、上記の最悪のシナリオに戻る前に、Adobe Pass認証がサポートできるプリフライト認証方式について説明します。

## AuthN でのプリフライト {#preflight-authn}

このプリフライトシナリオは OLCA 互換（Cableabs）です。 「認証アサーション内の属性文」と題された認証と承認インタフェース 1.0 仕様の 7.5.2 節では、SAML 認証応答に事前承認されたリソースのリストを含める方法について説明しています。 IdP がこれをサポートしている場合、Adobe Pass Authentication Server は、認証時にプリフライトされたリソースリストを生成し、認証トークンと共にクライアントにキャッシュできます。 このメソッドは最良のシナリオも実現します。プログラマが checkPreauthorizedResources （）を呼び出しても、すべてがクライアント上に既に存在するので、ネットワークコールは実行されません。

### SAML Attribute ステートメントのカスタム・リソース・リスト {#custom-res-saml-attr}

IdP の SAML 認証応答には、AdobePass が許可する必要があるリソース名を含んだ AttributeStatement が含まれなければなりません。  一部のMVPDでは、これを次の形式で提供します。

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

上記の例は、事前認証済みの 2 つのリソース（「MMOD」と「Olympics 2012」）を含むリストを示しています。

これにより、最良のシナリオが効果的に実現され、プログラマが checkPreauthorizedResources （）を呼び出しても、すべてがクライアント上に既に存在するので、ネットワークコールは実行されません。

## AuthZ でのマルチチャネルプリフライト {#preflight-multich-authz}

このプリフライト実装も OLCA 互換（Cablelabs）です。  Authentication and Authorization Interface 1.0 Specification （7.5.3 および 7.5.4 節）に、SAML アサーションまたは XACML を使用してMVPDから認証情報をリクエストする方法を説明しています。 これは、認証フローの一部としてこれをサポートしない MVPD の認証ステータスを照会する場合に推奨される方法です。 Adobe Pass認証は、MVPDへの 1 回のネットワーク呼び出しを発行して、許可されたリソースのリストを取得します。


Adobe Pass認証は、プログラマーのアプリケーションからリソースのリストを受け取ります。 Adobe Pass認証のMVPD統合では、1 つの AuthZ 呼び出しにこれらすべてのリソースを含めることができ、応答を解析し、複数の permit/deny 決定を抽出できます。  マルチチャネル AuthZ を使用したプリフライトのシナリオのフローは、次のように機能します。

1. プログラマーのアプリは、preflight クライアント API を使用して、リソースのコンマ区切りリスト（例：&quot;TestChannel1,TestChannel2,TestChannel3&quot;）を送信します。
1. MVPD preflight AuthZ リクエスト呼び出しには、次の構造を持つ複数のリソースが含まれています。

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## 複数のリソースのカスタム認証 {#custom-authz}

一部の MVPD は、1 回のリクエストで複数のリソースの認証をサポートする認証エンドポイントを持っていますが、マルチチャネル AuthZ で説明されているシナリオには該当しません。 これらの特定の MVPD には、カスタム作業が必要です。

Adobeは、既存の実装を変更せずに複数チャネルの認証をサポートすることもできます。  このアプローチは、AdobeとMVPD テクニカルチームの間で見直し、期待どおりに機能することを確認する必要があります。

## プリフライト認証をサポートする MVPD {#mvpds-supp-preflight-authz}

次の表に、プリフライト認証をサポートする MVPD と、サポートするプリフライトのタイプおよび既知の制限事項を示します。

| プリフライトアプローチ | MVPD | 備考 |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| マルチチャネル AuthZ | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| ユーザーメタデータのチャネルラインアップ | Suddenlink HTC | すべての Synacor 直接統合もこのアプローチをサポートできます。 |
| 分岐と結合 | 上記に記載されていないその他すべて | チェックされるリソースのデフォルトの最大数= 5。 |

