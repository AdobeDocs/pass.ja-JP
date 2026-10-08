---
title: Proxy MVPD Web Service
description: Proxy MVPD Web Service
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1057'
ht-degree: 0%
---

# Proxy MVPD web サービス {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> Proxy MVPD web サービスを使用する前に、次の前提条件が満たされていることを確認します。
>
> * 「[ クライアント資格情報の取得](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメント」の説明に従って、クライアント資格情報を取得します。
> * 「[ アクセストークンの取得](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメント」の説明に従って、アクセストークンを取得します。
>
> 登録アプリケーションの作成方法とソフトウェアステートメントのダウンロード方法について詳しくは、[動的クライアント登録の概要](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## 概要 {#overview-proxy-mvpd-webserv}

「プロキシMVPD」とは、Adobe Pass Authenticationとの独自の統合を管理するだけでなく、関連する「プロキシ化されたMVPD」のグループに代わって使用権限プロセスも管理するMVPDのことです。 この仕組みはプログラマーに対して透明です。

ProxyMVPD機能を実装するために、Adobe Pass AuthenticationはRESTful web サービスを提供し、ProxyMVPDがProxiedMVPDのリストを送信および取得できるようにします。 このパブリック APIに使用されるプロトコルはREST HTTPで、次の前提があります。

- Proxy MVPDは、HTTP GET メソッドを使用して、現在の統合MVPDのリストを取得します。
- Proxy MVPDは、HTTP POST メソッドを使用して、サポートされているMVPDのリストを更新します。

## Proxy MVPD サービス {#proxy-mvpd-services}

- [ プロキシ MVPDの取得](#retriev-proxied-mvpds)
- [ プロキシ MVPDを送信](#submit-proxied-mvpds)

### プロキシ MVPDの取得 {#retriev-proxied-mvpds}

特定されたプロキシ MVPDと統合されたプロキシ MVPDの現在のリストを取得します。

| エンドポイント | 呼び出し元 | リクエストパラメーター | リクエストヘッダー | HTTP メソッド | HTTP レスポンス |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | 認証（必須） | GET | <ul><li> 200 （ok） – リクエストは正常に処理され、応答にはXML形式のProxiedMVPDのリストが含まれています</li><li>401 （未認証） – 次のいずれかを示します。<ul><li>クライアントは新しいaccess_tokenをリクエストしなければなりません</li><li>リクエストは、許可リストに存在しないIP アドレスから送信されます</li><li>トークンが無効です</li></ul></li><li>403 （禁止） – 指定されたパラメーターに対して操作がサポートされていないか、プロキシ MVPDがプロキシとして設定されていないか、またはプロキシが見つからないことを示します</li><li>405 （メソッドは許可されていません） - GETまたはPOST以外のHTTP メソッドが使用されました。 HTTP メソッドは一般にサポートされていないか、この特定のエンドポイントではサポートされていません。</li><li>500 （内部サーバーエラー） – リクエストプロセス中にサーバー側でエラーが発生しました。</li></ul> |

Curlの例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


XML応答の例：

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

### プロキシ MVPDの送信 {#submit-proxied-mvpds}

特定されたプロキシ MVPDと統合されたMVPDの配列をプッシュします。

| エンドポイント | 呼び出し元 | リクエストパラメーター | リクエストヘッダー | HTTP メソッド | HTTP レスポンス |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Authorization （必須） proxied-mvpd （必須） | 投稿する | <ul><li>201 （作成済み） – プッシュは正常に処理されました</li><li>400 （不正なリクエスト） – サーバーはリクエストの処理方法を認識しません。<ul><li>受信XMLは、この仕様で公開されたスキーマに準拠しません</li><li>プロキシ mvpdに一意のIDがありません</li><li>プッシュされた要求者IDは、400応答コードの他のサーブレットコンテナ理由が存在しません</li></ul><li>401 （未認証） – 次のいずれかを示します。<ul><li>クライアントは新しいaccess_tokenをリクエストしなければなりません</li><li>リクエストは、許可リストに存在しないIP アドレスから送信されます</li><li>トークンが無効です</li></ul></li><li>403 （禁止） – 指定されたパラメーターに対して操作がサポートされていないか、プロキシ MVPDがプロキシとして設定されていないか、またはプロキシが見つからないことを示します</li><li>405 （メソッドは許可されていません） - GETまたはPOST以外のHTTP メソッドが使用されました。 HTTP メソッドは一般にサポートされていないか、この特定のエンドポイントではサポートされていません。</li><li>500 （内部サーバーエラー） – リクエストプロセス中にサーバー側でエラーが発生しました。</li></ul> |

Curlの例：

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XMLの例：

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


### 投稿頻度 {#posting-frequency}

Adobe Pass Authenticationでは、ProxyMVPDがProxiedMVPDのリストをプッシュするのは、前のプッシュからの変更がある場合のみにすることをお勧めします。

### プロキシ MVPDの削除 {#delete-proxied-freqency}

ProxyMVPDが空のProxiedMVPD リストを持つXML レコードをプッシュすると、その空のリストは任意のリストと同じようにシステムに保存され、前のリストが効果的に削除されます。



## XSD形式 {#xsd-format}

Adobeでは、パブリック web サービスとの間でプロキシ MVPDを投稿または取得するための次の使用可能なフォーマットを定義しています。

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

- `id` （必須） - Proxied MVPD IDは、次のいずれかの文字を使用して、MVPDの名前に関連する文字列である必要があります（トラッキング目的でプログラマーに公開されるため）。
 – 任意の英数字、アンダースコア（&quot;_&quot;）、ハイフン（&quot;-&quot;）。
- idIDは、次の正規表現に準拠している必要があります。
`(a-zA-Z0-9((-)|_)*)`

    したがって、1文字以上で始まり、文字、数字、ダッシュ、またはアンダースコアで続ける必要があります。

- `iframeSize` （オプション） - iframeSize要素はオプションで、MVPD認証ページがiFrame内にあるはずの場合はiFrameのサイズを定義します。 iframeSize要素が存在しない場合、認証は完全なブラウザーリダイレクトページで行われます。
- `requestorIds` （オプション） - requestorIds値はAdobeによって提供されます。 プロキシ化されたMVPDは、少なくとも1つのrequestorIdと統合する必要があります。 「requestorIds」タグがプロキシ化されたMVPD要素に存在しない場合、そのプロキシ化されたMVPDは、プロキシMVPDに統合されたすべての利用可能な依頼者と統合されます。
- `ProviderID` （オプション） - ProviderID属性がID要素に存在する場合、ProviderIDの値は、プロキシ MVPDに対するSAML認証リクエストで、プロキシ MVPD / SubMVPD IDとして（ID値の代わりに）送信されます。 この場合、idの値は、プログラマーページに表示されるMVPD ピッカーと、Adobe Pass認証によって内部的にのみ使用されます。 ProviderID属性の長さは、1 ～ 128文字である必要があります。

## セキュリティ {#security}

リクエストを有効と見なすには、次のルールを尊重する必要があります。

- リクエストヘッダーには、[ アクセストークンの取得](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って取得したセキュリティ Oauth2 アクセストークンが含まれている必要があります。
- リクエストは、許可されている特定のIP アドレスから取得する必要があります。
- リクエストはSSL プロトコル経由で送信する必要があります。

上記にリストされていないリクエストヘッダーに存在するパラメーターは無視されます。

Curlの例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Adobe Pass認証環境用のMVPD Web サービスエンドポイントのプロキシ {#proxy-mvpd-wevserv-endpoints}

- **実稼動URL:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **ステージング URL:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **プレクアル実稼動URL:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **プレクアルステージング URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
