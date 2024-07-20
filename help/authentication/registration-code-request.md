---
title: 登録ページ
description: 登録ページ
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# 登録ページ {#registration-page}

## REST API エンドポイント {#clientless-endpoints}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) という制限があります。

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#create-reg-code-svc}

ランダムに生成された登録コードとログインページ URI を返します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode</br> 例：</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | ストリーミングアプリ </br> プログラマ </br> サービス | 1.依頼者 </br>    （パスコンポーネント） </br>2.  deviceId （ハッシュ化）   </br>    （必須） </br>3.  device_info/X-Device-Info （必須） </br>4.  mvpd （オプション） </br>5.  ttl （オプション） </br>6.  _deviceType_</br> 7.  _deviceUser_ （非推奨） </br>8。  _appId_ （非推奨） | POST | 失敗した場合に登録コードおよび情報またはエラーの詳細を含む XML または JSON。 以下のスキーマとサンプルを参照してください。 | 201 |

{style="table-layout:auto"}

| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| device_info/</br>X-Device-Info | ストリーミングデバイス情報。</br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 </br> 詳しくは、「デバイスと接続情報の受け渡し [ を参照してください ](/help/authentication/passing-client-information-device-connection-and-application.md)。 |
| mvpd | この操作が有効な MVPD ID。 |
| ttl | このリグレコードの有効期間（秒）。</br>**メモ**:ttl に許可されている最大値は 36000 秒（10 時間）です。 値を大きくすると、400 HTTP 応答（無効なリクエスト）が返されます。 `ttl` を空のままにすると、Adobe Pass Authentication はデフォルト値の 30 分を設定します。 |
| _deviceType_ | デバイスタイプ（Roku、PC など）。</br> このパラメーターを正しく設定すると、ESM では、クライアントレスの使用時に [ デバイスタイプごとに分類 ](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) される指標を提供するので、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できます。</br> 参照 [ パス指標でクライアントレスデバイスタイプパラメーターを使用するメリット ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意**:device_info はこのパラメーターを置き換えます。 |
| _deviceUser_ | デバイスユーザー識別子。 |
| _appId_ | アプリケーション ID/名前。 </br>**注意**：このパラメーターは device_info に置き換えられます。 |

{style="table-layout:auto"}


>[!CAUTION]
>
>**ストリーミングデバイスの IP アドレス**
></br>
>クライアントからサーバーへの実装の場合、ストリーミングデバイスの IP アドレスは、この呼び出しで暗黙的に送信されます。  **regcode** 呼び出しが、ストリーミングデバイスではなくプログラマーサービスとして行われるサーバー間実装の場合、ストリーミングデバイスの IP アドレスを渡すには次のヘッダーが必要です。
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>ここで、`<streaming\_device\_ip>` はストリーミングデバイスのパブリック IP アドレスです。
></br></br>
>例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### 応答 XML スキーマ {#xml-schema}


#### 登録コード XSD {#registration-code-xsd}

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

</br>

| 要素名 | 説明 |
| --------------- | ------------------------------------------------------------------------------------ |
| id | 登録コードサービスで生成された UUID |
| コード | 登録コードサービスで生成された登録コード |
| 要求者 | 要求者 ID |
| mvpd | Mvpd ID |
| 生成日時 | 登録コード作成タイムスタンプ（1970 年 1 月 1 日（PT）からのミリ秒単位） |
| expires | 登録コードの有効期限が切れる際のタイムスタンプ（1970 年 1 月 1 日（GMT）からのミリ秒単位） |
| deviceId | 一意のデバイス ID （または XSTS トークン） |
| deviceType | デバイスタイプ |
| deviceUser | デバイスにログインしているユーザー |
| appId | アプリケーション Id |
| appVersion | アプリケーション バージョン |
| registrationurl | エンドユーザーに表示されるログイン Web アプリの URL |

{style="table-layout:auto"}


</br>

### エラーメッセージ XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### 応答のサンプル {#sample-response}

**XML:**

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

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
