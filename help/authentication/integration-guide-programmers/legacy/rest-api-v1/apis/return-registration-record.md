---
title: 登記記録を返還する
description: 登記記録を返還する
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 2%

---

# （レガシー）返品登録記録 {#return-registration-record}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

`<REGGIE_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## 説明 {#description}

登録コード UUID、登録コード、ハッシュ化されたデバイス ID を含む登録コードレコードを返します。






| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>例：<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJCFK?format=xml | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1.依頼者 </br>    （パスコンポーネント） </br>2.  登録コード </br>    （パスコンポーネント） | GET | 登録コードと情報を含む XML または JSON。 以下のスキーマとサンプルを参照してください。 | 200 |

{style="table-layout:auto"}




| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| 登録コード | ストリーミングデバイスに表示される（認証フローに入力される）登録コード値。 |




## 応答 XML スキーマ {#response-xml-schema}

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
| id | 登録コードサービスで生成された UUID |
| コード | 登録コードサービスで生成された登録コード |
| 要求者 | 要求者 ID |
| mvpd | MVPD ID |
| 生成日時 | 登録コード作成タイムスタンプ（1970 年 1 月 1 日（PT）からのミリ秒単位） |
| expires | 登録コードの有効期限が切れるタイムスタンプ（1970 年 1 月 1 日（GMT）からのミリ秒単位） |
| deviceId | 一意のデバイス ID （または XSTS トークン） |
| deviceType | デバイスタイプ |
| deviceUser | デバイスにログインしているユーザー |
| appId | アプリケーション Id |
| appVersion | アプリケーション バージョン |
| registrationurl | エンドユーザーに表示されるログイン Web アプリの URL |

{style="table-layout:auto"}

### 応答のサンプル {#sample-response}

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
