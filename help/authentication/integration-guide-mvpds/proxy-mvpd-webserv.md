---
title: プロキシ MVPD Web サービス
description: プロキシ MVPD Web サービス
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# プロキシ MVPD Web サービス {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> プロキシMVPD web サービスを使用する前に、次の前提条件が満たされていることを確認してください。
>
> * [&#x200B; クライアント資格情報の取得 &#x200B;](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメントの説明に従って、クライアント資格情報を取得します。
> * [&#x200B; アクセストークンの取得 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、アクセストークンを取得します。
>
> 登録されたアプリケーションを作成してソフトウェアのステートメントをダウンロードする方法について詳しくは、[&#x200B; 動的クライアント登録の概要 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## 概要 {#overview-proxy-mvpd-webserv}

「プロキシMVPD」はMVPDの一種で、Adobe Pass Authentication との独自の連携の管理に加えて、関連する「プロキシ化された MVPD」のグループの代わりに使用権限プロセスも管理します。 この配置は、プログラマーに対して透過的です。

ProxyMVPD 機能を実装するために、Adobe Pass認証は、ProxyMVPD が ProxyedMVPD のリストを送信および取得できる RESTful web サービスを提供します。 このパブリック API に使用されるプロトコルは REST HTTP で、次の前提があります。

&#x200B;- プロキシMVPDは、HTTP GET メソッドを使用して、現在の統合 MVPD のリストを取得します。
&#x200B;- プロキシMVPDは、HTTP POST メソッドを使用して、サポートされる MVPD のリストを更新します。

## プロキシ MVPD サービス {#proxy-mvpd-services}

&#x200B;- [&#x200B; プロキシ化された MVPD の取得 &#x200B;](#retriev-proxied-mvpds)
&#x200B;- [&#x200B; プロキシ化された MVPD を送信 &#x200B;](#submit-proxied-mvpds)

### プロキシされた MVPD の取得 {#retriev-proxied-mvpds}

特定されたプロキシMVPDと統合された、プロキシ化された MVPD の現在のリストを取得します。

| エンドポイント | 呼び出し元 | リクエストパラメーター | リクエストヘッダー | HTTP メソッド | HTTP 応答 |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | 認証（必須） | GET | <ul><li> 200 （ok） – リクエストが正常に処理され、応答に XML 形式の ProxyedMVPD のリストが含まれる</li><li>401 （未認証） – 次のいずれかを示します。<ul><li>クライアントは、新しい access_token を要求する必要があります。</li><li>リクエストの発信元の IP アドレスが許可リストに存在しません</li><li>トークンが無効です</li></ul></li><li>403 （禁止） – 指定されたパラメーターで操作がサポートされていないか、プロキシ MVPDがプロキシとして設定されていないか、見つかりません</li><li>405 （許可されていない方法） - GETまたは POST 以外の HTTP メソッドが使用されました。 この特定のエンドポイントでは、HTTP メソッドが一般にサポートされていないか、サポートされていません。</li><li>500 （内部サーバーエラー） – リクエストプロセス中に、サーバーサイドでエラーが発生しました。</li></ul> |

Curl の例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


XML 応答の例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### プロキシされた MVPD の送信 {#submit-proxied-mvpds}

特定されたプロキシ MVPDと統合された MVPD の配列をプッシュします。

| エンドポイント | 呼び出し元 | リクエストパラメーター | リクエストヘッダー | HTTP メソッド | HTTP 応答 |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | 認証（必須）プロキシ化された mvpds （必須） | POST | <ul><li>201 （作成） – プッシュは正常に処理されました</li><li>400 （無効なリクエスト） – サーバーはリクエストの処理方法を把握していません：<ul><li>受信 XML はこの仕様で公開されているスキーマに準拠していません。</li><li>プロキシされた mvpid に一意の ID がありません</li><li>プッシュされた requestorIds は、応答コード 400 の他のサーブレットコンテナの理由は存在しません</li></ul><li>401 （未認証） – 次のいずれかを示します。<ul><li>クライアントは、新しい access_token を要求する必要があります。</li><li>リクエストの発信元の IP アドレスが許可リストに存在しません</li><li>トークンが無効です</li></ul></li><li>403 （禁止） – 指定されたパラメーターで操作がサポートされていないか、プロキシ MVPDがプロキシとして設定されていないか、見つかりません</li><li>405 （許可されていない方法） - GETまたは POST 以外の HTTP メソッドが使用されました。 この特定のエンドポイントでは、HTTP メソッドが一般にサポートされていないか、サポートされていません。</li><li>500 （内部サーバーエラー） – リクエストプロセス中に、サーバーサイドでエラーが発生しました。</li></ul> |

Curl の例：

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XML 例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### 転記頻度 {#posting-frequency}

Adobe Pass認証では、ProxyMVPD が ProxyedMVPD のリストをプッシュするのは、以前のプッシュから変更があった場合のみにすることをお勧めします。

### プロキシ化された MVPD の削除 {#delete-proxied-freqency}

ProxyMVPD が空の ProxyedMVPDs リストを持つ XML レコードをプッシュすると、その空のリストは他のリストと同様にシステムに保存され、以前のリストを効果的に削除します。



## XSD 形式 {#xsd-format}

Adobeでは、アドビのパブリック web サービスに対してプロキシ化された MVPD を公開/取得するために、以下の受け入れ可能な形式を定義しています。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**要素に関するメモ：**

&#x200B;- `id` （必須） – プロキシ化されたMVPD ID は、次のいずれかの文字を使用して、MVPDの名前に関連する文字列にする必要があります（トラッキング目的でプログラマーに公開されるため）。
&#x200B;- 英数字、アンダースコア（&quot;_&quot;）、ハイフン（&quot;-&quot;）のいずれかです。
&#x200B;- idID は、次の正規表現に準拠する必要があります。
`(a-zA-Z0-9((-)|_)*)`

     したがって、1 文字以上の文字で始まり、文字、数字、ダッシュ、アンダースコアで始まる必要があります 
。
&#x200B;- `iframeSize` （任意） - iframeSize 要素はオプションで、MVPD認証ページが iFrame 内にあると想定される場合に、iFrame のサイズを定義します。 そうしないと、iframeSize 要素が存在しない場合、認証はブラウザーの完全なリダイレクトページで発生します。
&#x200B;- `requestorIds` （任意） - requestorIds の値は、Adobeによって指定されます。 要件は、プロキシ化されたMVPDが 1 つ以上の requestorId と統合される必要があることです。 プロキシ化されたMVPD要素に「requestorIds」タグが存在しない場合、プロキシ化されたMVPDは、プロキシMVPDの下に統合された、使用可能なすべてのリクエスターと統合されます。
&#x200B;- `ProviderID` （オプション） - ProviderID 属性が id 要素に存在する場合、ProviderID の値が、SAML 認証リクエストで、（id 値ではなく）プロキシ化されたMVPD/SubMVPD ID として、プロキシMVPDに送信されます。 この場合、id の値は、プログラマーページに表示されるMVPD ピッカー、およびAdobe Pass Authentication 内部でのみ使用されます。 ProviderID 属性の長さは 1 ～ 128 文字にする必要があります。

## セキュリティ {#security}

リクエストを有効と見なすには、次のルールを遵守する必要があります。

&#x200B;- リクエストヘッダーには、[&#x200B; アクセストークンの取得 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントで説明されているように取得されたセキュリティ Oauth2 アクセストークンが含まれている必要があります。
 – このリクエストは、許可されている特定の IP アドレスから送信される必要があります。
&#x200B;- リクエストは、SSL プロトコルを使用して送信する必要があります。

リクエストヘッダーに存在し、上記に示されていないすべてのパラメーターは無視されます。

Curl の例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Adobe Pass認証環境用のプロキシ MVPD Web サービスエンドポイント {#proxy-mvpd-wevserv-endpoints}

&#x200B;- **実稼動 URL:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **ステージング URL:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **PreQual-Production URL:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **PreQual-Staging URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
