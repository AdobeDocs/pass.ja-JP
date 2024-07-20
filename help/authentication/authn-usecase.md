---
title: MVPD 認証
description: MVPD 認証
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# MVPD 認証 {#mvpd-authn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#mvpd-authn-overview}

実際のサービスプロバイダー（SP）の役割はプログラマーが保持しますが、Adobe Pass認証はそのプログラマーの SP プロキシとして機能します。 Adobe Pass認証を仲介役として使用すると、MVPD とプログラマーの両方が、エンタイトルメントプロセスをケースバイケースでカスタマイズする必要がなくなります。

次の手順では、SAML をサポートする MVPD からプログラマーが認証をリクエストする際に、Adobe Pass認証を使用してイベントのシーケンスを示します。 Adobe Pass Authentication Access Enabler コンポーネントは、ユーザーまたはサブスクライバのクライアント上でアクティブになります。 そこから、アクセス・イネーブラにより、認証フローのすべてのステップが容易になります。

1. ユーザーが保護されたコンテンツへのアクセスを要求すると、Access Enabler はプログラマ（SP）に代わって認証（AuthN）を開始します。
1. SP のアプリは、有料 TV プロバイダー（MVPD）を取得するために、ユーザーに「MVPD ピッカー」を提示します。 次に、SP は選択された MVPD の ID プロバイダ（IdP）サービスにユーザーのブラウザをリダイレクトします。  これは「**プログラマが開始したログイン**」です。  MVPD は、IdP の応答をAdobeの SAML アサーションコンシューマーサービスに送信し、そこで処理されます。
1. 最後に、Access Enabler はブラウザを SP サイトにリダイレクトし、AuthN リクエストのステータス（成功/失敗）を SP に通知します。

## 認証リクエスト {#authn-req}

前述の手順に従って、AuthN フローでは、MVPD は SAML ベースの AuthN 要求を受け入れ、SAML AuthN 応答を送信する必要があります。

[ オンライン・コンテンツ・アクセス（OLCA）認証および承認インタフェースの仕様 ](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} は、標準の AuthN 要求および応答を示します。 Adobe Pass Authentication では、MVPD はこの標準に基づいて使用権限メッセージを行う必要はありませんが、仕様を調べると、AuthN トランザクションに必要な主要な属性に関するインサイトを得ることができます。

>[!NOTE]
>
>MVPD がAdobe Pass Authentication で受け取る AuthN リクエストには、デジタル署名が含まれています。 ただし、次の例では、簡潔にするために、署名は表示されません。 デジタル署名の表示例については、以降の節の [ 認証応答 ](#authn-response) の例を参照してください。

SAML 認証リクエストの例：

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

次の表は、認証リクエストで必要な属性とタグを、デフォルトの期待値で説明しています。

**SAML 認証リクエストの詳細**

| samlp:AuthnRequest | &lt;AuthnRequest> がサービス プロバイダから ID プロバイダに発行されました。 |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | これは、後続の応答で使用するAdobeエンドポイントです。 デフォルト値：**http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| 宛先 | このリクエストが送信されたアドレスを示す URI 参照。 これは、意図しない受信者へのリクエストの悪意のある転送を防ぐのに役立ちます。これは、一部のプロトコルバインディングで必要とされる保護です。 存在する場合、実際の受信者は、URI 参照がメッセージの受信場所を識別していることを確認する必要があります。 そうでない場合、リクエストは破棄する必要があります。 一部のプロトコル バインディングでは、この属性を使用する必要があります。 |
| ForceAuthn | ForceAuthn 属性が true の値を持つ場合、この属性は、プリンシパルとの既存のセッションに依存するのではなく、ID プロバイダーに対してこの ID を新たに確立するように義務付けます。 |
| ID | リクエストの識別子。 詳しくは、[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} の 1.3.4 の節を参照してください。 |
| パッシブである | ブール値。 「true」の場合、ID プロバイダーおよびユーザーエージェント自体は、要求者からユーザーインターフェイスを目に見えて制御し、プレゼンターと目立つ方法でやり取りしてはなりません。 値を指定しない場合、デフォルトは「false」です。 |
| 問題の即時 | 応答の発行時刻。 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} の 1.3.3 節で説明されているように、時間値は UTC でエンコードされます。 |
| ProtocolBinding | &lt;Response> メッセージを返す際に使用される SAML プロトコルバインディングを識別する URI 参照。 プロトコルバインディングと URI 参照の定義について詳しくは、[SAMLBind] を参照してください。 デフォルト値：urn:oasis:names:tc:SAML:2.0:bindings:HTTP POST |
| バージョン | このリクエストのバージョン。 |
| saml:Issuer | 応答メッセージを生成したエンティティを識別します。 （この要素について詳しくは、SAML core 2.0-os のセクション 2.2.5 を参照してください） |
| ds:Signature | SAML core 2.0-os のセクション 5 で説明されているように、アサーションの整合性を保護し、アサーションの発行者を認証する XML 署名 |
| samlp:NameIDPolicy | 要求されたサブジェクトを表すために使用される名前識別子に関する制約を指定します。 |
| AllowCreate | ID プロバイダーがリクエストの実行中に、プリンシパルを表す新しい識別子を作成することを許可するかどうかを示すために使用されるブール値。 デフォルト：true |
| 形式 | 名前識別子の形式に対応する URI 参照を指定しますデフォルト：urn:oasis:names:tc:SAML:2.0:nameid-format:transientAdobeの推奨：urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | 必要に応じて、アサーション主体の識別子が、リクエスター以外のサービスプロバイダーの名前空間で返される（または作成される）ように指定します。 デフォルト :http://saml.sp.adobe.adobe.com |

## 認証応答 {#authn-response}

認証要求を受信して処理したら、MVPD は認証応答を送信する必要があります。

**SAML 認証応答のサンプル**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


上記のサンプルでは、AdobeSP は、Subject/NameId からユーザー ID を取得することを想定しています。 AdobeSP は、カスタム定義属性からユーザー ID を取得するように構成できます。レスポンスには、次のようなエレメントが含まれている必要があります。

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**SAML 認証応答の詳細**
| samlp:Response               |Adobe Pass Authentication から受信した応答。                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|宛先                  |このリクエストが送信されたアドレスを示す URI 参照。 これは、意図しない受信者へのリクエストの悪意のある転送を防ぐのに役立ちます。これは、一部のプロトコルバインディングで必要とされる保護です。 存在する場合、実際の受信者は、URI 参照がメッセージの受信場所を識別していることを確認する必要があります。 そうでない場合、リクエストは破棄する必要があります。 一部のプロトコル バインディングでは、この属性を使用する必要があります。 |
| ID                           | リクエストの識別子。 タイプは xs:ID であり、識別子の一意性について [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} の 1.3.4 節で指定されている要件に従う必要があります。 リクエストの ID 属性と対応する応答の InResponseTo 属性の値は一致する必要があります。                                                                                                                                                                                         |
|応答                 |証明エンティティがアサーションを提示する際に応答する SAML プロトコル メッセージの ID。 値は、認証リクエストで送信される ID 属性の値と等しい必要があります。 SAML core 2.0-os を参照                                                                                                                                                                                                                                                      |
| IssueInstant                 |要求の発行時刻。                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| バージョン                      | リクエストのバージョン。                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| saml:Issuer                  |は、リクエストメッセージを生成したエンティティを識別します。 （この要素について詳しくは、2.2.5 節を参照してください。SAML core 2.0-os）                                                                                                                                                                                                                                                                                                                                     |
| samlp:Status                 |対応するリクエストのステータスを表すコード。                                                                                                                                                                                                                                                                                                                                                                                                               |
| samlp:StatusCode             |対応するリクエストに応答して実行されたアクティビティのステータスを表すコード。                                                                                                                                                                                                                                                                                                                                                                       |
| saml:Assertion               |この型は、すべてのアサーションに共通する基本情報を指定します。                                                                                                                                                                                                                                                                                                                                                                                                        |
| ID                           |このアサーションの識別子。                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| バージョン                      |このアサーションのバージョン。                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| IssueInstant                 |要求の発行時刻。                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ds:Signature                 | SAML core 2.0-os のセクション 5 で説明されているように、アサーションの整合性を保護し、アサーションの発行者を認証する XML 署名                                                                                                                                                                                                                                                                                                                                 |
| ds:SignedInfo                | SignedInfo の構造には、正規化アルゴリズム、署名アルゴリズム、および 1 つ以上の参照が含まれます。 SignedInfo 要素には、他の署名やオブジェクトから参照できるようにするオプションの ID 属性を含めることができます。 XML 署名の構文と処理を参照してください                                                                                                                                                                           |
| ds:CanonicalizationMethod    | CanonicalizationMethod は、署名計算を実行する前に SignedInfo 要素に適用される正規化アルゴリズムを指定する必須要素です。 XML 署名の構文と処理を参照してください                                                                                                                                                                                                                                                           |
| ds:SignatureMethod           | SignatureMethod は、署名の生成と検証に使用するアルゴリズムを指定する必須要素です。 このアルゴリズムは、署名操作に関係するすべての暗号化機能（ハッシュ、公開鍵アルゴリズム、MAC、パディングなど）を識別します。 XML 署名の構文と処理を参照してください                                                                                                                                                              |
| ds:Reference                 |参照は、1 回以上発生する可能性のある要素です。 ダイジェストアルゴリズムとダイジェスト値を指定し、必要に応じて、署名するオブジェクトの識別子、オブジェクトのタイプ、ダイジェスト前に適用する変換のリストを指定します。 XML 署名の構文と処理を参照してください                                                                                                                                                                         |
| ds：変換                | オプションの変換要素には、変換要素の順序付きリストが含まれます。これらのリストは、署名者がダイジェストされたデータオブジェクトをどのように取得したかを説明します。 各トランスフォームの出力は、次のトランスフォームへの入力として機能します。 最初の Transform への入力は、参照要素の URI 属性を逆参照した結果です。 最後の変換からの出力は、DigestMethod アルゴリズムの入力です。 XML 署名の構文と処理を参照してください          |
| ds:DigestMethod              | DigestMethod は、署名されたオブジェクトに適用されるダイジェスト アルゴリズムを識別する必須要素です。 XML 署名の構文と処理を参照してください                                                                                                                                                                                                                                                                                                                        |
| ds:DigestValue               | DigestValue は、ダイジェストのエンコードされた値を含む要素です。 ダイジェストは、常に base64 を使用してエンコードされます。 XML 署名の構文と処理を参照してください                                                                                                                                                                                                                                                                                                               |
| ds:SignatureValue            | SignatureValue 要素には、デジタル署名の実際の値が含まれます。この値は、常に base64 を使用してエンコードされます。 XML 署名の構文と処理を参照してください                                                                                                                                                                                                                                                                                                                 |
| ds:KeyInfo                   | KeyInfo は、署名の検証に必要なキーを受信者が取得できるようにするオプションの要素です。 XML 署名の構文と処理を参照してください                                                                                                                                                                                                                                                                                                                   |
| ds:X509 データ                  | KeyInfo 内の X509Data 要素には、キーまたは X509 証明書の 1 つ以上の識別子が含まれています。 XML 署名の構文と処理を参照してください                                                                                                                                                                                                                                                                                                                                 |
| ds: X509 証明書          | X509Certificate 要素。base64 でエンコードされた [X509v3] 証明書が含まれます                                                                                                                                                                                                                                                                                                                                                                                          |
| saml:Subject                 | アサーション内のステートメントの件名。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| saml:NameID                  | &lt;NameID> 要素は NameIDType 型（SAML core 2.0-os の 2.2.2 節を参照）であり、&lt;Subject> 要素や &lt;SubjectConfirmation> 要素などのさまざまな SAML アサーション構成や、様々なプロトコルメッセージで使用されます。                                                                                                                                                                                                                                           |
|形式                       |文字列ベースの識別子情報の分類を表す URI 参照。                                                                                                                                                                                                                                                                                                                                                                                    |
| SPNameQualifier              |さらに、サービスプロバイダー名またはプロバイダーの所属名で名前を修飾します。 この属性は、証明書利用者または関係者に基づいて名前を統合する追加の手段を提供します。                                                                                                                                                                                                                                                                      |
| saml:SubjectConfirmation     |被写体を確認できる情報。 複数のサブジェクト確認を提供する場合は、それらのいずれかを満たすことで、アサーションを適用する目的でサブジェクトを確認するのに十分です。                                                                                                                                                                                                                                                    |
| saml:SubjectConfirmationData |特定の確認方法で使用される追加の確認情報。 例えば、この要素の一般的なコンテンツは、XML 署名構文および処理仕様で定義されている <!--<ds:KeyInfo>--> 要素である可能性があります                                                                                                                                                                                                                                         |
| NotOnOrAfter                 |主体が確認できなくなったときの時間。                                                                                                                                                                                                                                                                                                                                                                                                            |
|受信者                    |証明するエンティティがアサーションを提示できるエンティティまたは場所を指定する URI。 例えば、この属性は、仲介者が別の場所にリダイレクトされるのを防ぐために、アサーションを特定のネットワークエンドポイントに配信する必要があることを示す場合があります。                                                                                                                                                                                   |
| saml：条件              | &lt;Condition> 要素は、新しい条件の拡張ポイントとして機能します。                                                                                                                                                                                                                                                                                                                                                                                                   |
| NotBefore                    | NotBefore 属性は、有効期間が開始される時刻を指定します。                                                                                                                                                                                                                                                                                                                                                                                  |
| saml:AudienceRestriction     | &lt;AudienceRestriction> 要素は、&lt;Audience> 要素で識別される 1 つ以上の特定のオーディエンスにアサーションが対処されることを指定します。                                                                                                                                                                                                                                                                                                                           |
| saml:Audience                |対象オーディエンスを識別する URI 参照。                                                                                                                                                                                                                                                                                                                                                                                                                      |
| saml:AuthnStatement          |認証文。                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| AuthnInstant                 |認証が行われた時刻を指定します。                                                                                                                                                                                                                                                                                                                                                                                                                 |
| SessionIndex                 |サブジェクトによって識別されるプリンシパルと認証機関の間の特定のセッションのインデックスを指定します。                                                                                                                                                                                                                                                                                                                                              |
| saml:AuthnContext            |このステートメントが生成された認証イベントを含む、認証機関が使用するコンテキスト。                                                                                                                                                                                                                                                                                                                                                 |
| saml:AuthnContextClassRef    |後続の認証コンテキスト宣言を記述する認証コンテキスト クラスを識別する URI 参照。                                                                                                                                                                                                                                                                                                                                            |
