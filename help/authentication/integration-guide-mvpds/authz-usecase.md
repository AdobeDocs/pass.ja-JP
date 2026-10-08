---
title: MVPD Authorization
description: MVPD Authorization
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%
---
# MVPD Authorization

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#mvpd-authz-overview}

認証（AuthZ）は、AdobeがホストするバックエンドサーバーとMVPD AuthZ エンドポイント間のバックチャネル（サーバー間）通信を介して実行されます。

AuthZ リクエストの場合、認証エンドポイントは少なくとも次のパラメーターを処理できる必要があります。

* **Uid**。 認証ステップから受信したユーザーID。

* **リソース ID**。 特定のコンテンツリソースを識別する文字列。 このリソース IDはプログラマによって指定され、MVPDはこれらのリソースに関するビジネスルールを強化する必要があります（例えば、ユーザーが特定のチャネルに登録されていることを確認することで）。

ユーザーが認証されているかどうかの判断に加えて、応答には、認証の有効期限である、この認証の有効期間（TTL）が含まれている必要があります。 TTLが設定されていない場合、AuthZ リクエストは失敗します。  このため、**TTLは、Adobe PassがリクエストにTTLを含まない場合をカバーするために、MVPD認証側**&#x200B;で必須の設定です。

## 認証リクエスト {#authz-req}

AuthZ リクエストには、リクエストを実行する主体、その主体がアクセスしようとしているリソース、その主体がリソースに対して実行しようとしているアクション、および操作を実行しようとしている環境が含まれている必要があります。 Adobe Pass認証の場合、これらの要素は次に対応します。

| XACML要素 | 対応先 |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| 件名 | SAML アサーションの「subject-token」 AttributeValueによって参照される、認証済みセッションで識別されるプリンシパル。 |
| リソース | 保護されたリソースのURI。 |
| アクション | ビュー： |
| 環境 | SPで表示される、リクエスト側のクライアントのIP アドレスが含まれます。 |



この時点でSPはXACML Authorization DecisionQueryを準備し、それを（HTTP POST経由で） IdPの（以前に合意された） Policy Decision Point （PDP）に送信する必要があります。 以下は、単純なXACML リクエストの例です（XACML コア仕様を参照）。

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


AuthZ リクエストを受け取った後、MVPDのPDPはリクエストを評価し、リクエストされたアクションをリクエストされたリソースに対して実行できるかどうかを判断します。 その後、MVPDは、以下の「承認応答」の説明に従って、決定、ステータスコード、およびメッセージを含む応答を返します。

## 認証の応答 {#authz-response}

AuthZ リクエストへの応答は、MVPDがリクエストを評価し、リクエストされたビジネスルールを適用して、件名がリクエストされたアクションをリソースで実行できるかどうかを判断した後に行われます。 Adobe Pass Authenticationに対して返される応答は、XACML コア仕様に従って、SPがPolicy Enforcement Point （PEP）として持つDecision、Status Code、Message、およびObligationsで再度表されます。 応答の例を次に示します。

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

次に、Adobe Pass Authenticationがサポートし、プログラマーが実行できるDENY Obligationsのリストを示します。

* **urn:tve:xacml:2.0:obligations:restrict-pc** – 加入者はペアレンタルコントロールのチェックに失敗しました。SPは、このコンテンツへのアクセスを制限するために適切な対策を講じる必要があります。

* **urn:tve:xacml:2.0:obligations: アップグレード** – 購読者に適切なサブスクリプションレベルがありません。  コンテンツにアクセスするには、サブスクリプションをアップグレードする必要があります。

Adobe Pass Authenticationでは、次の&#x200B;**PERMIT**&#x200B;の義務をサポートしており、プログラマーがそれらを満たすことができます。

* **urn:cablelabs:olca:1.0:obligations:log** - Adobe Passはトランザクションをログに記録し、合意されたレポートメカニズムを介して利用できるようにします。

* **urn:cablelabs:olca:1.0:obligations:re-authz** - Adobe Pass Authenticationは、認証をn秒以内に再度更新します（XACML AttributeAssignmentを介してObligationの引数として指定します – XACML コア仕様、セクション 5.46を参照）。

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
