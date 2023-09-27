---
title: MVPD Preflight Authorization
description: MVPD Preflight Authorization
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# MVPD Preflight Authorization

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#mvpd-preflight-authz-intro}

「プリフライト認証」は、複数のリソースに対する軽量な認証チェックです。 プログラマーは主に、UI を修飾するために使用します（例えば、ロックアイコンとロック解除アイコンを使用してアクセスステータスを示します）。

Adobe Pass認証では、現在、AuthN 応答属性またはマルチチャネル AuthZ 要求を介して、MVPD のプリフライト認証を 2 つの方法でサポートしています。  次のシナリオでは、プリフライト認証を実装する様々な方法のコストとメリットについて説明します。

* **ベストケースシナリオ** - MVPD は、承認フェーズ中に事前に認証されたリソースのリストを提供します（マルチチャネル AuthZ）。
* **最悪のケースのシナリオ** - MVPD が何らかの形式の複数のリソース認証をサポートしていない場合、Adobe Pass認証サーバーは、リソースリスト内の各リソースに対して MVPD への認証呼び出しを実行します。 このシナリオは、（リソースの数に比例して）プリフライト認証リクエストの応答時間に影響を与えます。 パフォーマンスの問題を引き起こすAdobeと MVPD の両方のサーバーの負荷を増やすことができます。 また、実際に再生を必要とせずに、認証リクエスト/応答イベントを生成します。
* **非推奨** - MVPD は、認証フェーズ中に事前に許可されたリソースのリストを提供するので、リストがクライアント上でキャッシュされるので、プリフライトリクエストも含め、ネットワーク呼び出しは不要です。

MVPD はプリフライト認証をサポートする必要はありませんが、以下の節では、上記の最悪のシナリオにフォールバックする前に、Adobe Pass Authentication がサポートできるプリフライト認証方法について説明します。

## AuthN でのプリフライト {#preflight-authn}

このプリフライトシナリオは、OLCA 互換 (Cableabs) です。 Authentication and Authorization Interface 1.0 Specification の 7.5.2 では、SAML 認証応答に事前に認証されたリソースのリストを含める方法について説明します。 IdP がこれをサポートしている場合、Adobe Pass認証サーバーは、認証時に事前に設定されたリソースリストを生成し、認証トークンと共にクライアント上でキャッシュできます。 このメソッドはまた、最良のシナリオを達成し、プログラマーが checkPreauthorizedResources() を呼び出すと、ネットワーク呼び出しは実行されません。すべてが既にクライアント上にあるからです。

### SAML 属性ステートメントのカスタムリソースリスト {#custom-res-saml-attr}

IdP の SAML 認証応答には、AdobePass が承認する必要のあるリソース名を含む AttributeStatement が含まれている必要があります。  一部の MVPD では、次の形式でこれを提供しています。

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

上記のサンプルは、「MMOD」と「Olinpics2012」の 2 つの事前認証済みリソースを含むリストを示しています。

これは効果的に最良のシナリオを達成し、すべてが既にクライアント上にあるので、プログラマーが checkPreauthorizedResources() を呼び出すと、ネットワーク呼び出しは実行されません。

## AuthZ でのマルチチャネルプリフライト {#preflight-multich-authz}

このプリフライト実装は、OLCA 互換 (Cablelabs) でもあります。  認証および認証インターフェイス 1.0 の仕様（7.5.3 と 7.5.4 節）では、SAML アサーションまたは XACML を使用して MVPD から認証情報を要求する方法について説明します。 認証フローの一部として、これをサポートしない MVPD の認証状態を照会する場合は、この方法をお勧めします。 Adobe Pass認証は、MVPD に対する 1 回のネットワーク呼び出しを発行して、許可されたリソースのリストを取得します。


Adobe Pass認証は、プログラマーのアプリケーションからリソースのリストを受け取ります。 次に、Adobe Pass認証の MVPD 統合は、これらのすべてのリソースを含む 1 つの AuthZ 呼び出しをおこない、応答を解析して複数の許可/拒否の決定を抽出できます。  マルチチャネル AuthZ シナリオでのプリフライトのフローは、次のように機能します。

1. プログラマーのアプリは、プリフライトクライアント API を介して、リソースのコンマ区切りリストを送信します。例：「TestChannel1,TestChannel2,TestChannel3」。
1. MVPD プリフライト AuthZ 要求呼び出しには、複数のリソースが含まれ、次の構造を持ちます。

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

一部の MVPD には、1 回のリクエストで複数のリソースに対する認証をサポートする認証エンドポイントがありますが、マルチチャネル AuthZ で説明されているシナリオには該当しません。 これらの特定の MVPD は、カスタム作業を必要とします。

Adobeは、既存の実装に変更を加えることなく、複数のチャネル認証をサポートすることもできます。  このアプローチは、Adobeと MVPD 技術チームの間でレビューし、期待どおりに動作することを確認する必要があります。

## プリフライト認証をサポートする MVPD {#mvpds-supp-preflight-authz}

次の表に、プリフライト認証をサポートする MVPD と、それらがサポートするプリフライトのタイプおよび既知の制限を示します。

| プリフライトアプローチ | MVPD | メモ |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| マルチチャネル AuthZ | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| ユーザメタデータのチャネルラインアップ | 突然の HTC のリンク | すべての Synacor の直接統合も、この方法をサポートできます。 |
| 分岐と結合 | 上記以外のすべて | チェックされたリソースのデフォルトの最大数= 5。 |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
