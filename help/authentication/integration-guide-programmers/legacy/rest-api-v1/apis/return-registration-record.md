---
title: 登録レコードを返す
description: 登録レコードを返す
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 3%
---
# （従来）返品登録記録 {#return-registration-record}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!NOTE]
>
> REST APIの実装は[ スロットル メカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって制限されています

## REST API エンドポイント {#clientless-endpoints}

`<REGGIE_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## 説明 {#description}

登録コード UUID、登録コード、およびハッシュ化されたデバイス IDを含む登録コードレコードを返します。






| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>例：<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJCFK?format=xml | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者</br> （パスコンポーネント） </br>2。  登録コード </br> （パスコンポーネント） | GET | 登録コードと情報を含むXMLまたはJSON。 以下のスキーマとサンプルを参照してください。 | 200 |

{style="table-layout:auto"}




| 入力パラメーター | 説明 |
| --- | --- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| 登録コード | ストリーミングデバイスに表示される登録コードの値（認証フローに入力される）。 |




## 応答XML スキーマ {#response-xml-schema}

### 登録コード XSD

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

| 要素名 | 説明 |
| --- | --- |
| id | 登録コードサービスによって生成されたUUID |
| コード | 登録コードサービスによって生成された登録コード |
| 依頼者 | 依頼者ID |
| mvpd | MVPD ID |
| 生成日 | 登録コード作成タイムスタンプ（1970年1月1日GMTからのミリ秒単位） |
| 期限切れ | 登録コードの有効期限が切れるタイムスタンプ（1970年1月1日GMTからのミリ秒単位） |
| deviceId | 一意のデバイス ID （またはXSTS トークン） |
| deviceType | デバイスタイプ |
| deviceUser | ユーザーがこのデバイスにログインしました |
| appId | アプリケーション Id |
| appVersion | アプリケーションバージョン |
| registrationURL | エンドユーザーに表示されるログイン Web アプリのURL |

{style="table-layout:auto"}

### 応答サンプル {#sample-response}

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```
