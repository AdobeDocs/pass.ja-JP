---
title: MVPD 認証
description: MVPD 認証
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# MVPD 認証

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#mvpd-authz-overview}

認証（AuthZ）は、Adobeがホストするバックエンドサーバーと MVPD AuthZ エンドポイントの間で、バックチャネル（サーバー間）通信を介して行われます。

AuthZ リクエストの場合、認証エンドポイントは少なくとも次のパラメーターを処理できる必要があります。

* **Uid**。 認証手順から受信したユーザー ID。

* **リソース ID**。 特定のコンテンツリソースを識別する文字列。 このリソース ID はプログラマによって指定され、MVPD はこれらのリソースに関するビジネス ルールを強化する必要があります（たとえば、ユーザーが特定のチャネルを購読していることを確認するなど）。

応答には、ユーザーが認証されているかどうかを判断する以外に、この認証の有効期間（TTL）（つまり認証の有効期限）を含める必要があります。 TTL が設定されていない場合、AuthZ リクエストは失敗します。  このため、MVPD がリクエストに TTL を含めない場合に対応するために、**TTL はAdobe Pass認証側の必須の設定です**。

## 承認リクエスト {#authz-req}

AuthZ リクエストには、リクエストを行う主体、主体がアクセスしようとしているリソース、主体がリソースに対して実行しようとしているアクション、および操作が実行されようとしている環境が含まれている必要があります。 Adobe Pass認証の特定のケースでは、これらの要素は次に対応します。

| XACML 要素 | 次に対応 |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| 件名 | 認証済みセッションで識別されるプリンシパル。SAML アサーションの「subject-token」 AttributeValue で参照されます。 |
| Resource | 保護されたリソースの URI。 |
| アクション | 表示。 |
| 0.5511122 | SP から見た要求元クライアントの IP アドレスが含まれます。 |



この時点で SP は、XACML Authorization DecisionQuery を準備し、（HTTPPOST経由で） IdP の（事前に合意された） Policy Decision Point （PDP; ポリシー決定ポイント）に送信する必要があります。 次に、単純な XACML リクエストの例を示します（XACML コア仕様を参照）。

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


AuthZ 要求を受信した後、MVPD の PDP は要求を評価し、要求されたアクションをリソースで実行することをサブジェクトに許可するかどうかを決定します。 次に、MVPD は、決定、ステータスコード、およびメッセージを含む応答を返します。以下の認証応答を参照してください。

## 承認応答 {#authz-response}

AuthZ 要求への応答は、MVPD が要求を評価し、要求されたビジネスルールを適用して、サブジェクトが要求されたアクションをリソースに対して実行することが許可されるかどうかを判断した後になります。 返されたAdobe Pass認証への応答は、SP が Policy Enforcement Point （PEP; ポリシー強制ポイント）として持つ Decision、Status code、message、および Oblidations を含む XACML コア仕様に従って再度表されます。 応答の例を次に示します。

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

以下は、Adobe Pass認証がサポートし、プログラマーが履行できる DENY 義務のリストです。

* **urn:tve:xacml:2.0:obligations:restrict-pc** - サブスクライバはペアレンタルコントロールチェックに失敗しました。SP はこのコンテンツへのアクセスを制限するために適切な措置を講じる必要があります。

* **urn:tve:xacml:2.0:obligations:upgrade** - サブスクライバーに適切なサブスクリプション レベルがありません。  コンテンツにアクセスするには、サブスクリプションをアップグレードする必要があります。

Adobe Pass認証は、次の **PERMIT** 義務をサポートし、プログラマーがそれらを履行できるようにします。

* **urn:cablelabs:olca:1.0:obligations:log** - Adobe Passは、トランザクションをログに記録し、同意されたレポートメカニズムを通じて利用できるようにします。

* **urn:cablelabs:olca:1.0:obligations:re-authz** - Adobe Pass Authentication は、n 秒で認証内容を再度更新します（XACML AttributeAssignment を介した債務引数として指定されます – XACML コア仕様の 5.46 節を参照）。

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
