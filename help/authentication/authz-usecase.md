---
title: MVPD 認証
description: MVPD 認証
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# MVPD 認証

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#mvpd-authz-overview}

認証 (AuthZ) は、Adobeがホストするバックエンドサーバと MVPD AuthZ エンドポイントとの間のバックチャネル（サーバ間）通信を介して実行されます。

AuthZ 要求の場合、認証エンドポイントは、少なくとも次のパラメーターを処理できる必要があります。

* **Uid**. 認証手順で受け取ったユーザー ID。

* **リソース ID**. 特定のコンテンツリソースを識別する文字列。 このリソース ID はプログラマーによって指定され、MVPD は、これらのリソースに関するビジネスルールを強化する必要があります（例えば、ユーザーが特定のチャネルを購読しているかを確認するなど）。

ユーザーに対して承認を行うかどうかを決定するだけでなく、応答には、この認証の有効期間 (TTL)（認証が期限切れになる時点）を含める必要があります。 TTL が設定されていない場合、AuthZ 要求は失敗します。  この理由で **TTL は、Adobe Pass Authentication 側の必須の設定です** MVPD が要求に TTL を含めない場合に対応するために。

## 認証リクエスト {#authz-req}

AuthZ 要求には、要求がおこなわれる主体、主体がアクセスしようとしているリソース、主体がリソースで実行しようとしているアクション、操作がおこなわれる環境を含める必要があります。 Adobe Pass Authentication の場合、次の要素は次の要素に対応します。

| XACML 要素 | 対応する |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| 件名 | SAML アサーションの「subject-token」AttributeValue で参照される、認証済みセッションで識別されるプリンシパルです。 |
| リソース | 保護されたリソースの URI。 |
| アクション | 表示： |
| 環境 | SP によって確認される、要求クライアントの IP アドレスが含まれます。 |



この時点の SP は、XACML Authorization DecisionQuery を準備し、(HTTPPOSTを介して )IdP の（以前合意した）Policy Decision Point(PDP) に送信する必要があります。 単純な XACML 要求の例を以下に示します（XACML コア仕様を参照）。

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


AuthZ 要求を受け取った後、MVPD の PDP は、要求を評価し、リソースに対して要求されたアクションを実行することを主体に許可するかどうかを決定します。 次に、MVPD は、以下の「認証応答」で説明されているように、決定、ステータスコード、およびメッセージを含む応答を返します。

## 認証応答 {#authz-response}

AuthZ 要求への応答は、MVPD が要求を評価した後に行われ、要求されたビジネスルールを適用して、主体がリソースに対して要求されたアクションを実行できるかどうかを判断します。 返されるAdobe Pass認証への応答は、SP がポリシー強制ポイント (PEP) として持つ決定、ステータスコード、メッセージ、および義務を含む XACML コア仕様に従って再び表されます。 応答のサンプルを次に示します。

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

次に、Adobe Pass Authentication がサポートし、プログラマーが満たすための DENY 義務のリストを示します。

* **骨:tve:xacml:2.0:obligations:restrict-pc**  — サブスクライバが親の制御チェックに失敗し、SP はこのコンテンツへのアクセスを制限する適切な措置を講じる必要があります。

* **骨:tve:xacml:2.0:obligations:アップグレード**  — サブスクライバーに適切なサブスクリプションレベルがありません。  コンテンツにアクセスするには、購読をアップグレードする必要があります。

Adobe Pass Authentication では、次の機能をサポートしています **許可** プログラマーは、次のような義務を果たすことができます。

* **骨:cablelabs:olca:1.0:obligations:ログ** - Adobe Passはトランザクションを記録し、同意を得たレポートメカニズムを通じて利用できるようにします。

* **骨:cablelabs:olca:1.0:obligations:re-authz** - Adobe Pass認証は、認証を秒単位で再更新します (XACML AttributeAssignment を介して Beldint に対する引数として指定されます。 XACML コア仕様（セクション 5.46 を参照）。

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
