---
title: MVPD Authentication
description: MVPD Authentication
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1908'
ht-degree: 0%
---
# MVPD Authentication {#mvpd-authn}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#mvpd-authn-overview}

実際のサービスプロバイダー（SP）の役割はプログラマーが保持しますが、Adobe Pass認証はそのプログラマーのSP プロキシとして機能します。 Adobe Pass Authenticationを仲介者として使用することで、MVPDとプログラマーの両方が使用権限プロセスをケースバイケースでカスタマイズする必要がなくなります。

次の手順では、プログラマーがSAMLをサポートするMVPDから認証をリクエストする際に、Adobe Pass認証を使用して一連のイベントを示します。 Adobe Pass Authentication Access Enabler コンポーネントは、ユーザー/サブスクライバーのクライアントでアクティブであることに注意してください。 そこから、Access Enablerによって認証フローのすべての手順が容易になります。

1. ユーザーが保護されたコンテンツへのアクセスを要求すると、Access Enablerはプログラマー（SP）に代わって認証（AuthN）を開始します。
1. SPのアプリは、有料テレビ事業者（MVPD）を取得するために、利用者に「MVPD ピッカー」を表示します。 次に、SPは、ユーザーのブラウザーを、選択したMVPD ID プロバイダー（IdP）サービスにリダイレクトします。  これは「**プログラマー主導のログイン**」です。  MVPDは、IdPのレスポンスをAdobeのSAML アサーションコンシューマーサービスに送信し、処理されます。
1. 最後に、Access EnablerはブラウザーをSP サイトにリダイレクトし、AuthN リクエストのステータス（成功/失敗）をSPに通知します。

## 認証リクエスト {#authn-req}

上記の手順で説明したように、AuthN フロー中に、MVPDはSAML ベースのAuthN リクエストを受け入れ、SAML AuthN レスポンスを送信する必要があります。

[Online Content Access （OLCA）認証および認証インターフェイス仕様](https://www.cablelabs.com/specifications/search?query=&category=&subcat=&doctype=&content=false&archives=false){target=_blanck}では、標準のAuthN リクエストと応答が表示されます。 Adobe Pass Authenticationでは、MVPDがこの標準に基づいてエンタイトルメントメッセージを送信する必要はありませんが、仕様を調べると、AuthN トランザクションに必要な主要属性にinsightを提供することができます。

>[!NOTE]
>
>Adobe Pass認証でMVPDが受け取る認証リクエストには、デジタル署名が含まれています。 ただし、以下の例では、簡潔な理由から、署名は表示されません。 デジタル署名を示す例については、次の節の「[認証応答](#authn-response)」の例を参照してください。

SAML認証リクエストの例：

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

次の表では、認証リクエストに含める必要がある属性とタグと、デフォルトの期待値について説明します。

**SAML認証要求の詳細**

| samlp:AuthnRequest | &lt;AuthnRequest>は、サービスプロバイダーからID プロバイダーに発行されます。 |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | これは、その後の応答で使用するAdobe エンドポイントです。 デフォルト値：**http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| 宛先 | このリクエストが送信されたアドレスを示すURI参照。 これは、一部のプロトコルのバインディングで必要とされる保護である、意図しない受信者へのリクエストの悪意のある転送を防ぐのに役立ちます。 存在する場合、実際の受信者は、URI参照がメッセージを受信した場所を識別することを確認する必要があります。 そうでない場合は、リクエストを破棄する必要があります。 一部のプロトコル バインディングでは、この属性を使用する必要があります。 |
| ForceAuthn | 値がtrueの場合、ForceAuthn属性は、ID プロバイダーに対して、プリンシパルに対する既存のセッションに依存するのではなく、このIDを新たに確立することを義務付けます。 |
| ID | リクエストの識別子。 詳しくは、[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} セクション 1.3.4を参照してください。 |
| IsPassive | ブール値。 「true」の場合、ID プロバイダーとユーザーエージェント自体は、リクエスターからユーザーインターフェイスを目に見える形で制御し、プレゼンターと目立つ形で対話してはなりません。 値が指定されていない場合、デフォルトは「false」です。 |
| IssueInstant | 応答の発行の時刻です。 時刻の値は、[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} セクション 1.3.3で説明されているように、UTCでエンコードされます。 |
| ProtocolBinding | &lt;Response> メッセージを返す際に使用されるSAML プロトコルバインディングを識別するURI参照。 プロトコルのバインディングとURI参照について詳しくは、[SAMLBind]を参照してください。 デフォルト値：urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| バージョン | このリクエストのバージョン。 |
| saml:Issuer | 応答メッセージを生成したエンティティを識別します。 （この要素について詳しくは、SAML core 2.0-osの節2.2.5を参照してください）。 |
| ds:Signature | SAML core 2.0-osのセクション 5で説明されているように、アサーションの整合性を保護し、アサーションの発行者を認証するXML署名 |
| samlp:NameIDPolicy | 要求された被写体を表すために使用する名前識別子に対する制約を指定します。 |
| AllowCreate | ID プロバイダーがリクエストの処理中に、プリンシパルを表す新しい識別子を作成することを許可するかどうかを示すために使用されるブール値。 デフォルト：true |
| 書式設定 | 名前識別子フォーマットに対応するURI参照を指定します。デフォルト：urn:oasis:names:tc:SAML:2.0:nameid-format:一時的なAdobeが推奨：urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | オプションで、アサーションのサブジェクトの識別子をリクエスター以外のサービスプロバイダーの名前空間で返す（または作成する）ことを指定します。 デフォルト :http://saml.sp.adobe.adobe.com |

## 認証レスポンス {#authn-response}

認証要求を受信して処理した後、MVPDは認証応答を送信する必要があります。

**SAML認証応答のサンプル**

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


上記のサンプルでは、Adobe SPはSubject/NameIdからユーザーIDを取得することを想定しています。 Adobe SPは、カスタム定義された属性からユーザーIDを取得するように設定できます。応答には、次のような要素が含まれている必要があります。

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**SAML認証応答の詳細**

| samlp:Response | Adobe Pass認証で受信した応答。 |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 宛先 | このリクエストが送信されたアドレスを示すURI参照。 これは、一部のプロトコルのバインディングで必要とされる保護である、意図しない受信者へのリクエストの悪意のある転送を防ぐのに役立ちます。 存在する場合、実際の受信者は、URI参照がメッセージを受信した場所を識別することを確認する必要があります。 そうでない場合は、リクエストを破棄する必要があります。 一部のプロトコル バインディングでは、この属性を使用する必要があります。 |
| ID | リクエストの識別子。 タイプはxs:IDで、識別子の一意性については、[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank}のセクション 1.3.4で指定されている要件に従う必要があります。 リクエストのID属性と、対応する応答のInResponseTo属性の値は一致する必要があります。 |
| InResponseTo | アサーションをテストするエンティティがアサーションを提示できる応答のSAML プロトコルメッセージのID。 値は、認証リクエストで送信されるID属性の値と同じである必要があります。 SAML core 2.0-osを参照してください。 |
| IssueInstant | リクエストの発行の時刻。 |
| バージョン | リクエストのバージョン。 |
| saml:Issuer | リクエストメッセージを生成したエンティティを識別します。 （この要素について詳しくは、セクション 2.2.5を参照してください。 SAML コア 2.0-os） |
| samlp:Status | 対応するリクエストのステータスを表すコード。 |
| samlp:StatusCode | 対応するリクエストに応答して実行されるアクティビティのステータスを表すコード。 |
| saml:Assertion | このタイプは、すべてのアサーションに共通する基本情報を指定します。 |
| ID | このアサーションの識別子。 |
| バージョン | このアサーションのバージョンです。 |
| IssueInstant | リクエストの発行の時刻。 |
| ds:Signature | SAML core 2.0-osのセクション 5で説明されているように、アサーションの整合性を保護し、アサーションの発行者を認証するXML署名 |
| ds:SignedInfo | SignedInfoの構造には、正規化アルゴリズム、署名アルゴリズム、および1つ以上の参照が含まれます。 SignedInfo要素には、他の署名やオブジェクトで参照できるオプションのID属性が含まれている場合があります。 XML署名の構文と処理を参照してください |
| ds:CanonicalizationMethod | CanonicalizationMethodは、署名計算を実行する前にSignedInfo要素に適用される正規化アルゴリズムを指定する必須の要素です。 XML署名の構文と処理を参照してください |
| ds:SignatureMethod | SignatureMethodは、署名の生成と検証に使用するアルゴリズムを指定する必須の要素です。 このアルゴリズムは、署名操作に関係するすべての暗号化関数（ハッシュ、公開鍵アルゴリズム、MAC、パディングなど）を識別します。 XML署名の構文と処理を参照してください |
| ds:Reference | 参照は、1回以上発生する可能性のある要素です。 ダイジェストアルゴリズムとダイジェスト値を指定し、オプションで、署名するオブジェクトの識別子、オブジェクトのタイプ、ダイジェスト前に適用する変換のリストを指定します。 XML署名の構文と処理を参照してください |
| ds:Transforms | オプションのTransforms要素には、Transform要素の順序付きリストが含まれます。これらは、署名者がダイジェストされたデータオブジェクトをどのように取得したかを示します。 各Transformの出力は、次のTransformへの入力として機能します。 最初のTransformへの入力は、Reference要素のURI属性を逆参照した結果です。 最後のTransformの出力は、DigestMethod アルゴリズムの入力です。 XML署名の構文と処理を参照してください |
| ds:DigestMethod | DigestMethodは、署名済みオブジェクトに適用されるダイジェストアルゴリズムを識別する必須の要素です。 XML署名の構文と処理を参照してください |
| ds:DigestValue | DigestValueは、ダイジェストのエンコードされた値を含む要素です。 ダイジェストは常にbase64を使用してエンコードされます。 XML署名の構文と処理を参照してください |
| ds:SignatureValue | SignatureValue要素には、デジタル署名の実際の値が含まれます。これは常にbase64を使用してエンコードされます。 XML署名の構文と処理を参照してください |
| ds:KeyInfo | KeyInfoは、受信者が署名の検証に必要なキーを取得できるようにするオプションの要素です。 XML署名の構文と処理を参照してください |
| ds:X509Data | KeyInfo内のX509Data要素には、キーまたはX509証明書の1つ以上の識別子が含まれます。 XML署名の構文と処理を参照してください |
| ds: X509Certificate | Base64 エンコードされた[X509v3]証明書を含むX509Certificate要素 |
| saml:Subject | アサーション内のステートメントの件名。 |
| saml:NameID | &lt;NameID>要素はNameIDType タイプであり（SAML core 2.0-osのセクション 2.2.2を参照）、&lt;Subject>要素や&lt;SubjectConfirmation>要素などの様々なSAML アサーション構造や、様々なプロトコルメッセージで使用されます。 |
| 書式設定 | 文字列ベースの識別子情報の分類を表すURI参照。 |
| SPNameQualifier | さらに、サービスプロバイダーまたはプロバイダーの関連会社の名前を持つ名前を認定します。 この属性は、証明書利用者または関係者に基づいて、フェデレーション名に追加の手段を提供します。 |
| saml:SubjectConfirmation | 被験者を確認できる情報。 複数の被検者確認が提供された場合は、いずれかの被検者を満足すれば、アサーションを適用する目的で被検者を確認するのに十分である。 |
| saml:SubjectConfirmationData | 特定の確認方法で使用される追加確認情報。 例えば、この要素の一般的なコンテンツは、XML署名の構文と処理仕様で定義されている<!--<ds:KeyInfo>-->要素である場合があります |
| NotOnOrAfter | 被写体が確認できなくなった時点。 |
| 受信者 | アサーションを表示できるエンティティまたはアサーションをテストするエンティティの場所を指定するURI。 例えば、この属性は、仲介者が他の場所にアサーションをリダイレクトするのを防ぐために、アサーションを特定のネットワークエンドポイントに配信する必要があることを示している場合があります。 |
| saml:Conditions | &lt;Condition>要素は、新しい条件の拡張ポイントとして機能します。 |
| NotBefore | NotBefore属性は、有効区間が開始される時刻を指定します。 |
| saml:AudienceRestriction | &lt;AudienceRestriction>要素は、アサーションが&lt;Audience>要素によって識別された1つ以上の特定のオーディエンスに対応していることを指定します。 |
| saml:Audience | 対象オーディエンスを識別するURI参照。 |
| saml:AuthnStatement | 認証ステートメント。 |
| AuthnInstant | 認証が行われた時間を指定します。 |
| SessionIndex | 主体によって識別されたプリンシパルと認証機関との間の特定のセッションのインデックスを指定します。 |
| saml:AuthnContext | このステートメントを生成した認証イベントまで、認証機関が使用したコンテキスト。 |
| saml:AuthnContextClassRef | 次の認証コンテキスト宣言を記述する認証コンテキストクラスを識別するURI参照。 |
