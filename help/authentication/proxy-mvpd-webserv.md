---
title: プロキシ MVPD Web サービス
description: プロキシ MVPD Web サービス
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# プロキシ MVPD Web サービス {#proxy-mvpd-wbservice}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview-proxy-mvpd-webserv}

「プロキシ MVPD」は、Adobe Pass認証との独自の統合を管理するだけでなく、関連する「プロキシ化 MVPD」のグループの代わりにエンタイトルメントプロセスを管理する MVPD です。 この配置は、プログラマーに対して透明です。

ProxyMVPD 機能を実装するために、Adobe Pass認証は RESTful Web サービスを提供します。ProxyMVPDs は、ProxyedMVPDs のリストを送信し、取得できます。 このパブリック API で使用されるプロトコルは REST HTTP ですが、次の前提があります。

* プロキシ MVPD は、HTTPGETメソッドを使用して、現在の統合 MVPD のリストを取得します。
* Proxy MVPD は、HTTPPOSTメソッドを使用して、サポートされる MVPD のリストを更新します。

## プロキシ MVPD サービス {#proxy-mvpd-services}

* [プロキシ化された MVPD を取得](#retriev-proxied-mvpds)
* [プロキシ化された MVPD を送信](#submit-proxied-mvpds)

### プロキシ化された MVPD を取得 {#retriev-proxied-mvpds}

apikey パラメータで識別される ProxyMVPD の、プロキシ化された MVPD の現在のリストを取得します。

| エンドポイント | 呼び出し元 | リクエストヘッダー | HTTP メソッド | HTTP 応答 |
|---|---|---|---|---|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey（必須） | GET | <ul><li> 200 (ok) — リクエストが正常に処理され、応答に XML 形式の ProxiedMVPDs のリストが含まれます。</li><li>401 （未認証） — 指定された資格情報に対してユーザー認証が必要か、認証が許可されていません。  次のいずれかを示します。<ul><li>リクエストヘッダーに apikey トークンが存在しません</li><li>リクエスト元は、許可リストに存在しない IP アドレスです</li><li>トークンが無効です</li></ul></li><li>403 (forbidden) — 指定されたパラメーターに対して操作がサポートされていないか、プロキシ MVPD がプロキシとして設定されていないか、見つからないことを示します。</li><li>405（許可されていないメソッド） -GETまたはPOST以外の HTTP メソッドが使用されました。 HTTP メソッドは、通常はサポートされていないか、この特定のエンドポイントではサポートされていません。</li><li>500（内部サーバーエラー） — リクエストプロセス中に、サーバー側でエラーが発生しました。</li></ul> |

Curl の例：

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`


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

### プロキシ化された MVPD を送信 {#submit-proxied-mvpds}

apikey パラメータで識別される Proxy MVPD と統合された MVPD の配列をプッシュします。

| エンドポイント | 呼び出し元 | リクエストヘッダー | HTTP メソッド | HTTP 応答 |
|:------------------------------:|:---------:|:--------------------------------------------:|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey （必須） proxied-mvpds （必須） | POST | <ul><li>201（作成済み） — プッシュは正常に処理されました</li><li>400（無効なリクエスト） — サーバーはリクエストの処理方法を知りません。<ul><li>受信 XML は、この仕様でパブリッシュされたスキーマに準拠していません</li><li>プロキシ化された mvpds に一意の ID がありません</li><li>プッシュされた requestorIds は、応答コード 400 に対する「その他のサーブレット」コンテナの理由が存在しません</li></ul><li>401 （未認証） - apikey が無効か、呼び出し元の IP が許可リスト上にありません</li><li>403 (forbidden) — 指定されたパラメーターに対して操作がサポートされていないか、プロキシ MVPD がプロキシとして設定されていないか、見つからないことを示します。</li><li>405（許可されていないメソッド） -GETまたはPOST以外の HTTP メソッドが使用されました。 HTTP メソッドは、通常はサポートされていないか、この特定のエンドポイントではサポートされていません。</li><li>500（内部サーバーエラー） — リクエストプロセス中に、サーバー側でエラーが発生しました。</li></ul> |

Curl の例：

`curl -X POST -H "apikey: <API_KEY>" "https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XML の例：

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

Adobe Pass認証では、ProxyMVPDs は、以前のプッシュからの変更がある場合にのみ、ProxyedMVPDs のリストをプッシュすることをお勧めします。

### プロキシ化された MVPD を削除中 {#delete-proxied-freqency}

ProxyMVPD が空の ProxyedMVPDs リストを持つ XML レコードをプッシュした場合、その空のリストは他のリストと同様にシステムに保存されるので、前のリストを効果的に削除します。



## XSD 形式 {#xsd-format}

Adobeは、パブリック Web サービスとの間でプロキシ化された MVPD を投稿/取得するために、以下の受け入れ可能な形式を定義しました。

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

* `id` （必須） — プロキシ化された MVPD ID は、MVPD の名前に関連する文字列で、次の文字のいずれかを使用する必要があります（トラッキング目的で Programmers に公開されるため）。
   * 任意の英数字、アンダースコア (「_」) およびハイフン (「 — 」)。
   * idID は、次の正規表現に準拠している必要があります。
     `(a-zA-Z0-9((-)|_)*)`

     したがって、文字を少なくとも 1 つ含み、文字で始まり、任意の文字、数字、ダッシュ、アンダースコアを続ける必要があります。

* `iframeSize` （オプション） - iframeSize 要素はオプションです。MVPD 認証ページが iFrame 内にあると想定される場合の iFrame のサイズを定義します。 それ以外の場合、iframeSize 要素が存在しない場合、認証はブラウザーの完全なリダイレクトページでおこなわれます。
* `requestorIds` （オプション） - requestorIds 値はAdobeで提供されます。 プロキシ化された MVPD を少なくとも 1 つの requestorId と統合する必要があるという要件があります。 プロキシ化された MVPD 要素に「requestorIds」タグが存在しない場合、そのプロキシ化された MVPD は、プロキシ MVPD の下に統合された、使用可能なすべてのリクエスタと統合されます。
* `ProviderID` （オプション） - ProviderID 属性が id 要素に存在する場合、ProviderID の値は、プロキシ MVPD / SubMVPD ID としてプロキシ MVPD に SAML 認証リクエストで送信されます（id 値の代わりに）。 この場合、 id の値は、プログラマーページに表示される MVPD ピッカーでのみ使用され、内部的にはAdobe Pass Authentication によって使用されます。 ProviderID 属性の長さは 1 ～ 128 文字にする必要があります。

## セキュリティ {#security}

リクエストが有効であると見なされるためには、次のルールに従う必要があります。

* リクエストヘッダーには、セキュリティ API キーパラメーターが含まれている必要があります。 （これは、プロキシ MVPD の呼び出しを一意に識別するアプリケーションキーです）。
* リクエストは、許可されている特定の IP アドレスから送信される必要があります。
* リクエストは、SSL プロトコルを使用して送信する必要があります。

Adobeはトークンの（静的な）値を提供します。 この値は、認証および承認プロセスで使用されます。  リクエストヘッダーに存在し、上記のリストにないパラメーターは無視されます。

Curl の例：

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`

## Adobe Pass認証環境用のプロキシ MVPD Web サービスエンドポイント {#proxy-mvpd-wevserv-endpoints}

* **実稼動 URL :** https://mgmt.auth.adobe.com/control/v1/proxiedMvpds
* **ステージング URL:** https://mgmt.auth-staging.adobe.com/control/v1/proxiedMvpds
* **実稼動前の URL:** https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds
* **事前ステージング URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
