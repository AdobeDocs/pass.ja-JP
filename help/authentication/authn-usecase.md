---
title: MVPD 認証
description: MVPD 認証
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1908'
ht-degree: 0%

---

# MVPD 認証 {#mvpd-authn}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#mvpd-authn-overview}

実際のサービスプロバイダ (SP) の役割はプログラマが保持しますが、Adobe Pass認証は、そのプログラマの SP プロキシとして機能します。 Adobe Pass認証を仲介として使用すると、MVPD とプログラマーは、使用権限プロセスをケースバイケースでカスタマイズする必要がなくなります。

以下の手順は、SAML をサポートする MVPD からプログラマーが認証をリクエストする場合に、Adobe Pass認証を使用してイベントのシーケンスを示します。 Adobe Pass Authentication Access Enabler コンポーネントは、ユーザーの/サブスクライバのクライアント上でアクティブになっていることに注意してください。 ここから、Access Enabler は、認証フローのすべての手順を容易にします。

1. ユーザーが保護されたコンテンツへのアクセスを要求すると、Access Enabler は、プログラマ (SP) に代わって認証 (AuthN) を開始します。
1. SP のアプリは、ユーザーに「MVPD ピッカー」を提示して、ペイテレビプロバイダー (MVPD) を取得します。 次に、SP は、選択した MVPD の ID プロバイダ (IdP) サービスにユーザーのブラウザをリダイレクトします。  これは、**プログラマが開始したログイン**&quot;.  MVPD は、IdP の応答をAdobeの SAML アサーションコンシューマーサービスに送信し、そこで処理します。
1. 最後に、Access Enabler はブラウザを SP サイトにリダイレクトし、AuthN 要求のステータス（成功/失敗）を SP に通知します。

## 認証リクエスト {#authn-req}

上記の手順で示したように、AuthN フロー中、MVPD は SAML ベースの AuthN リクエストを受け入れ、SAML AuthN 応答を送信する必要があります。

The [オンラインコンテンツアクセス (OLCA) 認証および認証インターフェイスの仕様](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} は、標準の AuthN 要求と応答を示します。 Adobe Pass認証では、この標準に基づく権利付与メッセージをベースにする MVPD は必要ありませんが、仕様を見て、AuthN トランザクションに必要な主要属性に対する洞察を提供できます。

>[!NOTE]
>
>Adobe Pass認証で MVPD が受け取った AuthN 要求には、デジタル署名が含まれています。 ただし、以下の例では、簡潔にするために署名を示していません。 電子署名の例については、 [認証応答](#authn-response) 」を参照してください。

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

次の表に、認証リクエストに含める必要がある属性とタグ、およびデフォルトの期待値を示します。

**SAML 認証リクエストの詳細**

| samlp:AuthnRequest | &lt;authnrequest> は、サービスプロバイダーから ID プロバイダーに対して発行されました。 |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | これは、後続のAdobeで使用する応答エンドポイントです。 デフォルト値： **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| 宛先 | このリクエストが送信されたアドレスを示す URI 参照。 これは、意図しない受信者に対する要求の悪意のある転送を防ぐのに役立ちます。これは、一部のプロトコルバインディングで必要となる保護です。 実際の受信者は、メッセージが存在する場合、URI 参照でメッセージが受信された場所が識別されていることを確認する必要があります。 そうでない場合は、リクエストを破棄する必要があります。 一部のプロトコルバインディングでは、この属性を使用する必要が生じる場合があります。 |
| ForceAuthn | ForceAuthn 属性が存在する値が true の場合、ID プロバイダーはプリンシパルとの既存のセッションに依存するのではなく、この ID を新たに確立する必要があります。 |
| ID | リクエストの識別子。 詳しくは、 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} 詳細は 1.3.4 節を参照。 |
| IsPassive | ブール値。 「true」の場合、ID プロバイダーとユーザーエージェント自体は、リクエスターからユーザーインターフェイスを可視的に制御し、発表者と目に見える方法でやり取りしないでください。 値を指定しない場合、デフォルトは「false」です。 |
| IssueInstant | 応答の発行時間。 時刻の値は、 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} セクション 1.3.3. |
| ProtocolBinding | を返す際に使用する SAML プロトコルバインディングを識別する URI 参照 &lt;response> メッセージ。 詳しくは、 [SAMLBind] を参照してください。 デフォルト値：urn:oasis:名前:tc:SAML:2.0:bindings:HTTP-POST |
| バージョン | このリクエストのバージョン。 |
| saml:Issuer | 応答メッセージを生成したエンティティを識別します。 （この要素について詳しくは、 SAML コア 2.0-os セクション 2.2.5 を参照してください）。 |
| ds:Signature | SAML core 2.0-os のセクション 5 に説明されているように、アサーションの発行者の整合性を保護し、認証する XML 署名。 |
| samlp:NameIDPolicy | 要求された件名を表すために使用する名前識別子に対する制約を指定します。 |
| AllowCreate | ID プロバイダーがリクエストの処理中に、プリンシパルを表す新しい識別子を作成することを許可するかどうかを示すために使用される Boolean 値です。 デフォルト： true |
| 形式 | 名前識別子の形式に対応する URI 参照を指定します。デフォルト： urn:oasis:名前:tc:SAML:2.0:nameid-format:一時的なAdobeが推奨： urn:oasis:名前:tc:SAML:2.0:nameid-format:永続的 |
| SPNameQualifier | 要求者以外のサービスプロバイダーの名前空間でアサーション主体の識別子を返す（または作成する）ように指定する（オプション）。 デフォルト：http://saml.sp.adobe.adobe.com |

## 認証応答 {#authn-response}

認証要求を受信して処理した後、MVPD は認証応答を送信する必要があります。

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


上記の例では、AdobeSP は、Subject/NameId からユーザー ID を取得する必要があります。 AdobeSP は、カスタム定義属性からユーザー ID を取得するように構成できます。応答には、次のような要素を含める必要があります。

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
| samlp:Response | Adobe Pass Authentication が受信した応答。                                                                                                                                                                                                                                                                                                                                                                                                                       | |—|—| |宛先 |このリクエストが送信されたアドレスを示す URI 参照。 これは、意図しない受信者に対する要求の悪意のある転送を防ぐのに役立ちます。これは、一部のプロトコルバインディングで必要となる保護です。 実際の受信者は、メッセージが存在する場合、URI 参照でメッセージが受信された場所が識別されていることを確認する必要があります。 そうでない場合は、リクエストを破棄する必要があります。 一部のプロトコルバインディングでは、この属性を使用する必要が生じる場合があります。 | | ID |リクエストの識別子。 これは xs:ID 型であり、MUST は、の 1.3.4 節で規定されている要件に従う必要があります。 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} ID の一意性。 リクエストの ID 属性の値と、対応するレスポンスの InResponseTo 属性の値が一致する必要があります。                                                                                                                                                                                         | | InResponseTo |アサーションを提示できるアテストエンティティに応答する SAML プロトコルメッセージの ID。 の値は、認証リクエストで送信される ID 属性の値と等しい必要があります。 SAML core 2.0-os を参照してください。 | | IssueInstant |リクエストの発行時間。                                                                                                                                                                                                                                                                                                                                                                                                                                  | |バージョン |リクエストのバージョン。                                                                                                                                                                                                                                                                                                                                                                                                                                                | | saml:Issuer |リクエストメッセージを生成したエンティティを識別します。 ( この要素の詳細は、2.2.5 の節を参照してください。SAML core 2.0-os ) | | samlp:Status |対応するリクエストのステータスを表すコード。                                                                                                                                                                                                                                                                                                                                                                                                               | | samlp:StatusCode |対応するリクエストに応じて実行されるアクティビティのステータスを表すコード。                                                                                                                                                                                                                                                                                                                                                                       | | saml:Assertion |この型は、すべてのアサーションに共通の基本情報を指定します。                                                                                                                                                                                                                                                                                                                                                                                                        | | ID |このアサーションの識別子。                                                                                                                                                                                                                                                                                                                                                                                                                                         | |バージョン |このアサーションのバージョン。                                                                                                                                                                                                                                                                                                                                                                                                                                             | | IssueInstant |リクエストの発行時間。                                                                                                                                                                                                                                                                                                                                                                                                                                  | | ds:Signature | SAML core 2.0-os のセクション 5 に記載されているように、アサーションの発行者の整合性を保護し、認証する XML 署名 | | ds:SignedInfo | SignedInfo の構造には、正規化アルゴリズム、署名アルゴリズム、および 1 つ以上の参照が含まれます。 SignedInfo 要素には、他の署名やオブジェクトから参照できるオプションの ID 属性を含めることができます。 XML-Signature Syntax and Processing を参照してください。 | | ds:CanonicalizationMethod | CanonicalizationMethod は、署名の計算を実行する前に SignedInfo 要素に適用される正規化アルゴリズムを指定する必須の要素です。 XML-Signature Syntax and Processing を参照してください。 | | ds:SignatureMethod | SignatureMethod は、署名の生成と検証に使用するアルゴリズムを指定する必須の要素です。 このアルゴリズムは、署名操作に関係するすべての暗号化関数（ハッシュ、公開鍵アルゴリズム、MAC、パディングなど）を識別します。 XML-Signature Syntax and Processing を参照してください。 | | ds:Reference |参照は、1 回または複数回発生する要素です。 ダイジェストアルゴリズムとダイジェスト値を指定し、必要に応じて、署名されるオブジェクトの識別子、オブジェクトのタイプ、ダイジェスト前に適用される変換のリストを指定します。 XML-Signature Syntax and Processing を参照してください。 | | ds:Transforms |オプションの Transforms 要素には、Transform 要素の順序付きリストが含まれます。これらは、署名者が消化されたデータオブジェクトを取得する方法を示します。 各変換の出力は、次の変換への入力として機能します。 最初のトランスフォームへの入力は、Reference 要素の URI 属性を逆参照した結果です。 最後の変換の出力は、DigestMethod アルゴリズムの入力です。 XML-Signature Syntax and Processing を参照してください。 | | ds:DigestMethod | DigestMethod は、署名されたオブジェクトに適用するダイジェストアルゴリズムを識別する必須の要素です。 XML-Signature Syntax and Processing を参照してください。 | | ds:DigestValue | DigestValue は、ダイジェストのエンコードされた値を格納する要素です。 ダイジェストは、常に base64 を使用してエンコードされます。 XML-Signature Syntax and Processing を参照してください。 | | ds:SignatureValue | SignatureValue 要素には、デジタル署名の実際の値が含まれます。常に base64 を使用してエンコードされます。 XML-Signature Syntax and Processing を参照してください。 | | ds:KeyInfo | KeyInfo は、受信者が署名の検証に必要なキーを取得できるようにする、オプションの要素です。 XML-Signature Syntax and Processing を参照してください。 | | ds:X509Data | KeyInfo 内のX509Data 要素には、キーまたは X509 証明書の 1 つ以上の識別子が含まれています。 XML-Signature Syntax and Processing を参照してください。 | | ds: X509Certificate | X509Certificate 要素（base64 エンコードされたを含む） [X509v3] 証明書 | | saml:Subject |アサーションにおけるステートメントの件名。                                                                                                                                                                                                                                                                                                                                                                                                                          | | saml:NameID | &lt;nameid> 要素の型は NameIDType（SAML コア 2.0-os の 2.2.2 節を参照）で、次のような様々な SAML アサーション構成で使用されます。 &lt;subject> および &lt;subjectconfirmation> 要素、および様々なプロトコルメッセージ内の。                                                                                                                                                                                                                                           | |形式 |文字列ベースの識別子情報の分類を表す URI 参照です。                                                                                                                                                                                                                                                                                                                                                                                    | | SPNameQualifier |さらに、サービスプロバイダーの名前またはプロバイダーの所属を持つ名前を認定します。 この属性は、証明書利用者に基づいて名前を統合する追加の手段を提供します。                                                                                                                                                                                                                                                                      | | saml:SubjectConfirmation |件名を確認できる情報。 複数の件名の確認が提供されている場合は、いずれか 1 つを満たせば、アサーションを適用する目的で件名を確認するのに十分です。                                                                                                                                                                                                                                                    | | saml:SubjectConfirmationData |特定の確認方法で使用する追加の確認情報。 例えば、この要素の一般的なコンテンツは、 <!--<ds:KeyInfo>--> XML 署名構文および処理仕様で定義される要素 | | NotOnOrAfter |件名が確認できなくなった瞬間。                                                                                                                                                                                                                                                                                                                                                                                                            | |受信者 |証明エンティティがアサーションを提示できるエンティティまたは場所を指定する URI。 例えば、この属性は、仲介者が別の場所にリダイレクトするのを防ぐために、アサーションが特定のネットワークエンドポイントに配信される必要があることを示す場合があります。                                                                                                                                                                                   | | saml:Conditions | &lt;condition> 要素は、新しい条件の拡張ポイントとして機能します。                                                                                                                                                                                                                                                                                                                                                                                                   | | NotBefore | NotBefore 属性は、有効期間が開始される時刻を指定します。                                                                                                                                                                                                                                                                                                                                                                                  | | saml:AudienceRestriction | &lt;audiencerestriction> element は、アサーションが、 &lt;audience> 要素。                                                                                                                                                                                                                                                                                                                           | | saml:Audience |対象のオーディエンスを識別する URI 参照。                                                                                                                                                                                                                                                                                                                                                                                                                      | | saml:AuthnStatement |認証文。                                                                                                                                                                                                                                                                                                                                                                                                                                               | | AuthnInstant |認証が行われた時刻を指定します。                                                                                                                                                                                                                                                                                                                                                                                                                 | | SessionIndex |主体が識別するプリンシパルと認証機関との間の特定のセッションのインデックスを指定します。                                                                                                                                                                                                                                                                                                                                              | | saml:AuthnContext |この文を生成した認証イベントまでの認証機関が使用するコンテキスト。                                                                                                                                                                                                                                                                                                                                                 | | saml:AuthnContextClassRef |次に示す認証コンテキスト宣言を記述する認証コンテキストクラスを識別する URI 参照。 |
