---
title: プロキシ MVPD Web サービス
description: プロキシ MVPD Web サービス
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: f8cef3c41fb7132204c4fa499301c3010f62ca14
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# プロキシ MVPD Web サービス {#proxy-mvpd-wbservice}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。
>プロキシ MVPD Web サービスを使用するには、次の操作が必要です。
>- 登録したアプリケーションのソフトウェアに関する説明をサポートチームに問い合わせます
>- に基づいたアクセストークンの取得 [動的なクライアント登録](dynamic-client-registration.md)
> 

>[!NOTE]
>
>プロキシ MVPD Web サービスを使用するには、次の操作が必要です。
>- 登録したアプリケーションのソフトウェアに関する説明をサポートチームに問い合わせます
>- に基づいたアクセストークンの取得 [動的なクライアント登録](dynamic-client-registration.md)
> 

## 概要 {#overview-proxy-mvpd-webserv}

「Proxy MVPD」は MVPD であり、Adobe Pass Authentication との独自の連携の管理に加えて、関連する「Proxy MVPD」のグループの代わりに使用権限プロセスも管理します。 この配置は、プログラマーに対して透過的です。

ProxyMVPD 機能を実装するために、Adobe Pass認証は、ProxyMVPD が ProxyedMVPD のリストを送信および取得できる RESTful web サービスを提供します。 このパブリック API に使用されるプロトコルは REST HTTP で、次の前提があります。

- Proxy MVPD は、HTTP GET方式を使用して、現在の統合 MVPD の一覧を取得します。
- プロキシ MVPD は、HTTP POST方式を使用して、サポートされる MVPD の一覧を更新します。

## プロキシ MVPD サービス {#proxy-mvpd-services}

- [プロキシされた MVPD の取得](#retriev-proxied-mvpds)
- [プロキシされた MVPD の送信](#submit-proxied-mvpds)

### プロキシされた MVPD の取得 {#retriev-proxied-mvpds}

特定されたプロキシ MVPD と統合された、プロキシ化された MVPD の現在のリストを取得します。

| エンドポイント | 呼び出し元 | リクエストパラメーター | リクエストヘッダー | HTTP メソッド | HTTP 応答 |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;fqdn>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | 認証（必須） | GET | <ul><li> 200 （ok） – リクエストが正常に処理され、応答に XML 形式の ProxyedMVPD のリストが含まれる</li><li>401 （未認証） – 次のいずれかを示します。<ul><li>クライアントは、新しい access_token を要求する必要があります。</li><li>リクエストの発信元の IP アドレスが許可リストに存在しません</li><li>トークンが無効です</li></ul></li><li>403 （禁止） – 指定されたパラメーターで操作がサポートされていないか、プロキシ MVPD がプロキシとして設定されていないか、見つからないことを示します</li><li>405 （許可されていないメソッド） - GETまたはPOST以外の HTTP メソッドが使用されました。 この特定のエンドポイントでは、HTTP メソッドが一般にサポートされていないか、サポートされていません。</li><li>500 （内部サーバーエラー） – リクエストプロセス中に、サーバーサイドでエラーが発生しました。</li></ul> |

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

特定されたプロキシ MVPD と統合された MVPD の配列をプッシュします。

| エンドポイント | 呼び出し元 | リクエストパラメーター | リクエストヘッダー | HTTP メソッド | HTTP 応答 |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | 認証（必須）プロキシ化された mvpds （必須） | POST | <ul><li>201 （作成） – プッシュは正常に処理されました</li><li>400 （無効なリクエスト） – サーバーはリクエストの処理方法を把握していません：<ul><li>受信 XML はこの仕様で公開されているスキーマに準拠していません。</li><li>プロキシされた mvpid に一意の ID がありません</li><li>プッシュされた requestorIds は、応答コード 400 の他のサーブレットコンテナの理由は存在しません</li></ul><li>401 （未認証） – 次のいずれかを示します。<ul><li>クライアントは、新しい access_token を要求する必要があります。</li><li>リクエストの発信元の IP アドレスが許可リストに存在しません</li><li>トークンが無効です</li></ul></li><li>403 （禁止） – 指定されたパラメーターで操作がサポートされていないか、プロキシ MVPD がプロキシとして設定されていないか、見つからないことを示します</li><li>405 （許可されていないメソッド） - GETまたはPOST以外の HTTP メソッドが使用されました。 この特定のエンドポイントでは、HTTP メソッドが一般にサポートされていないか、サポートされていません。</li><li>500 （内部サーバーエラー） – リクエストプロセス中に、サーバーサイドでエラーが発生しました。</li></ul> |

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

Adobeでは、公開 web サービスとの間でプロキシ化された MVPD を公開/取得するために、以下に示す許可されたフォーマットを定義しています。

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

-   `id` （必須） – プロキシ化された MVPD ID は、次の文字のいずれかを使用して、MVPD の名前に関連する文字列である必要があります（トラッキング目的でプログラマーに公開されます）。– 英数字、アンダースコア（&quot;_&quot;）、ハイフン（&quot;-&quot;）。
- idID は、次の正規表現に準拠している必要があります。
`(a-zA-Z0-9((-)|_)*)`

    したがって、少なくとも 1 つの文字を含め、文字で始まり、文字、数字、ダッシュ、アンダースコアで続ける必要があります。

-   `iframeSize` （オプション） – iframeSize 要素はオプションで、MVPD 認証ページが iFrame にあると想定される場合に、iFrame のサイズを定義します。 そうしないと、iframeSize 要素が存在しない場合、認証はブラウザーの完全なリダイレクトページで発生します。
-   `requestorIds` （任意） – requestorIds の値はAdobeによって指定されます。 プロキシ化された MVPD が 1 つ以上の requestorId と統合されている必要があります。 「requestorIds」タグがプロキシ化された MVPD 要素に存在しない場合、プロキシ化された MVPD は、プロキシ MVPD に統合されたすべての利用可能なリクエスタと統合されます。
-   `ProviderID` （オプション） – ProviderID 属性が id 要素に存在する場合、ProviderID の値は、SAML 認証リクエストで、プロキシ化された MVPD/SubMVPD ID として（id 値ではなく）プロキシ化された MVPD に送信されます。 この場合、id の値は、プログラマーページに表示される MVPD ピッカー内、およびAdobe Pass Authentication 内部でのみ使用されます。 ProviderID 属性の長さは 1 ～ 128 文字にする必要があります。

## セキュリティ {#security}

リクエストを有効と見なすには、次のルールを遵守する必要があります。

- リクエストヘッダーには、からのセキュリティ Oauth2 アクセストークンが含まれている必要があります [動的なクライアント登録](dynamic-client-registration.md).
 – このリクエストは、許可されている特定の IP アドレスから送信される必要があります。
- リクエストは、SSL プロトコルを使用して送信する必要があります。

リクエストヘッダーに存在し、上記に示されていないすべてのパラメーターは無視されます。

Curl の例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/proxiedMvpds"`

## Adobe Pass Authentication Environments のプロキシ MVPD Web サービスエンドポイント {#proxy-mvpd-wevserv-endpoints}

- **実稼動 URL:** https://mgmt.auth.adobe.com/control/v3/proxiedMvpds - **ステージング URL:** https://mgmt.auth-staging.adobe.com/control/v3/proxiedMvpds - **実稼動前 URL:** https://mgmt-prequal.auth.adobe.com/control/v3/proxiedMvpds - **ステージング前 URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
