---
title: MVPD Preflight Authorization
description: MVPD Preflight Authorization
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 0%
---
# MVPD Preflight Authorization

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#mvpd-preflight-authz-intro}

「プリフライト認証」は、複数のリソースに対する軽量の認証チェックです。 プログラマーは、主にUIを装飾するために使用します（例えば、ロックとロック解除のアイコンでアクセスステータスを示します）。

Adobe Pass認証では、現在、MVPDに対して、AuthN応答属性またはマルチチャネルのAuthZ リクエストを介して、2つの方法でプリフライト認証をサポートできます。  次のシナリオでは、プリフライト認証を実装できるさまざまな方法のコストとメリットについて説明します。

* **ベストケース シナリオ** - MVPDは、認証フェーズ（マルチチャネル AuthZ）中に事前承認済みリソースのリストを提供します。
* **最悪のケースのシナリオ** - MVPDが複数のリソース認証をサポートしていない場合、Adobe Pass認証サーバーは、リソースリストの各リソースに対してMVPDへの認証呼び出しを実行します。 このシナリオは、プリフライト認証リクエストの応答時間に（リソース数に比例して）影響を与えます。 AdobeとMVPDの両方のサーバーの負荷が増加し、パフォーマンスの問題が発生する可能性があります。 また、実際にプレイを必要とせずに承認リクエスト/応答イベントを生成します。
* **非推奨** - MVPDは、認証フェーズ中に事前承認済みリソースのリストを提供します。そのため、リストはクライアントにキャッシュされるため、プリフライトリクエストであっても、ネットワーク呼び出しは必要ありません。

MVPDはプリフライト認証をサポートする必要はありませんが、次の節では、上記の最悪のケースのシナリオにフォールバックする前に、Adobe Pass認証がサポートできるプリフライト認証方法について説明します。

## AuthNでのプリフライト {#preflight-authn}

このプリフライトシナリオは、OLCA互換（Cableabs）です。 Authentication and Authorization Interface 1.0仕様セクション 7.5.2 「認証アサーション内の属性ステートメント」では、SAML認証応答に事前承認済みリソースのリストを含める方法について説明しています。 IdPがこれをサポートしている場合、Adobe Pass認証サーバーは、認証時に事前定義済みのリソースリストを生成し、認証トークンと共にクライアントにキャッシュできます。 このメソッドは最適なケースのシナリオも実現します。すべてがクライアント上に既に存在するため、プログラマがcheckPreauthorizedResources （）を呼び出してもネットワーク呼び出しは実行されません。

### SAML属性ステートメントのカスタムリソースリスト {#custom-res-saml-attr}

IdPのSAML認証応答には、AdobePassが認証する必要のあるリソース名を含むAttributeStatementが含まれます。  一部のMVPDでは、次のフォーマットで提供されています。

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

上記のサンプルには、事前に許可された2つのリソースを含むリストが表示されます。「MMOD」と「Olympics2012」。

これにより、最適なシナリオを効果的に実現できます。すべてがクライアント上に既に存在するため、プログラマがcheckPreauthorizedResources （）を呼び出してもネットワーク呼び出しは実行されません。

## AuthZでのマルチチャネルプリフライト {#preflight-multich-authz}

このプリフライト実装は、OLCA Compatible （Cablelabs）でもあります。  Authentication and Authorization Interface 1.0 Specification （セクション 7.5.3および7.5.4）では、SAML AssertionsまたはXACMLを使用してMVPDから認証情報を要求する方法について説明します。 認証フローの一部としてこれをサポートしていないMVPDの認証ステータスをクエリする場合に推奨される方法です。 Adobe Pass Authenticationは、MVPDに1回のネットワーク呼び出しを発行して、許可されたリソースのリストを取得します。


Adobe Pass認証は、プログラマーアプリケーションからリソースのリストを受け取ります。 Adobe Pass認証のMVPD統合では、これらのリソースをすべて含む1つのAuthZ呼び出しを行い、応答を解析して複数の許可/拒否の決定を抽出できます。  マルチチャネル AuthZ シナリオを使用したプリフライトのフローは、次のように機能します。

1. プログラマーのアプリは、プリフライトクライアント APIを介してリソースのコンマ区切りリストを送信します（例：「TestChannel1,TestChannel2,TestChannel3」）。
1. MVPD プリフライト AuthZ リクエスト呼び出しは、複数のリソースを含み、次の構造を持ちます。

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

## 複数のリソースに対するカスタム承認 {#custom-authz}

一部のMVPDには、1つのリクエストで複数のリソースの認証をサポートする認証エンドポイントがありますが、マルチチャネル AuthZで説明されているシナリオには該当しません。 これらの特定のMVPDでは、カスタム作業が必要です。

Adobeでは、既存の実装を変更することなく、複数チャネルの認証をサポートすることもできます。  このアプローチが期待どおりに機能することを確認するために、AdobeとMVPDのテクニカルチームとの間で、このアプローチを見直す必要があります。

## プリフライト認証をサポートするMVPD {#mvpds-supp-preflight-authz}

次の表に、プリフライト認証をサポートするMVPDと、それらがサポートするプリフライトのタイプおよび既知の制限を示します。

| プリフライトアプローチ | MVPD | メモ |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| マルチチャネル AuthZ | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| ユーザーメタデータのチャネルラインアップ | Suddenlink HTC | Synacorのダイレクト統合は、同様にこのアプローチをサポートします。 |
| 分岐と結合 | 上記に記載されていないその他すべて | チェックされたリソースのデフォルトの最大数= 5。 |

