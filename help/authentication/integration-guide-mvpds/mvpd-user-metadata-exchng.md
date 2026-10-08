---
title: MVPD User Metadata Exchange
description: MVPD User Metadata Exchange
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 0%
---
# MVPD User Metadata Exchange

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#intro-user-metadata-exchange}

MVPDは、顧客に関するユーザー固有のメタデータを保持し、場合によってはプログラマーと共有されます。 Adobe Pass Authenticationの目的は、この「ユーザーメタデータ」の交換を仲介することですが、交換に関するいかなる種類のルールも適用しません。 交換ルールは、MVPDがプログラマーパートナーと協力して作業するためのものです。

現在、Exchangeで使用できるユーザーメタデータタイプには、次のものが含まれます。

* 郵便番号
* 最大評価（VChipまたはMPAA）
* ユーザー ID
* 世帯ID
* チャネル ID

この機能を使用すると、MVPDとプログラマーは、ペアレンタルコントロールなどの特別なユースケースを実装できます。 例えば、MVPDでは、保護者の評価データをプログラマーに渡し、そのデータを使用してユーザーの利用可能な視聴の選択肢をフィルタリングできます。

ユーザーメタデータのキーポイント：

* MVPDは、認証フローと承認フロー中に、プログラマーのアプリケーションにユーザーメタデータを渡します
* Adobe Pass Authenticationは、AuthNおよびAuthZ トークンにメタデータ値を保存します
* Adobe Pass認証では、異なる形式でユーザーメタデータを提供するMVPDの値を正規化できます
* 一部のパラメーターは、プログラマーのキーを使用して暗号化できます
* Adobeでは、設定の変更により特定の値を使用できます

>[!NOTE]
>
>ユーザーメタデータは、以前にAdobe Pass Authenticationで利用可能だった静的メタデータ（認証トークン TTL、認証トークン TTL、およびデバイス ID）の拡張機能です。

## 例 {#example-mvpd-user-metadata-exch}

### 保護者の管理 {#example-parental-control}

次の例は、次のやりとりを示しています。

* [MVPD Metadata Exchangeへのプログラマ](#progr-mvpd-metadata-exch)

* [MVPDからプログラマへのメタデータ交換フロー](#mvpd-progr-exchange-flow)

### MVPD Metadata Exchangeへのプログラマ {#progr-mvpd-metadata-exch}

現在、Programmer API、Adobe Pass Authentication、MVPD Authorizersはすべて、チャネルレベルの認証のみをサポートしています。 チャネルは、プログラマのgetAuthorization （） API呼び出しでプレーンテキスト文字列として指定されます。 この文字列は、MVPDの承認バックエンドに至るまで反映されます。

プログラマーのアプリまたはサイトから、ユーザーはXACML対応のMVPD（この例では「TNT」）を選択します。 XACMLについて詳しくは、[拡張可能アクセス制御マークアップ言語](https://en.wikipedia.org/wiki/XACML){target=_blank}を参照してください。
プログラマーのアプリは、リソースとそのメタデータを含むAuthZ リクエストを形成します。  この例では、channel要素のmedia属性に「pg」というMPAA評価が含まれています。

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

Adobe Pass Authenticationは、MVPDとプログラマーの両方でサポートされている場合、アセットレベルまで、より詳細な認証をサポートします。 リソースとそのメタデータはAdobeに対して不透明です。その目的は、リソース IDとメタデータを正規化された方法で指定するための標準フォーマットを確立し、リソース IDを異なるMVPDに送信することです。

>[!NOTE]
>
>ユーザーがチャンネル専用のMVPDを選択した場合、Adobe Pass Authenticationはチャンネルタイトル（上記の例では「TNT」）のみを抽出し、タイトルのみをMVPDに渡します。

### MVPDからプログラマへのメタデータ交換フロー {#mvpd-progr-exchange-flow}

Adobe Pass認証では、次の前提が満たされます。

* MVPDは、SAML応答の一部として最大評価を送信します
* この情報は、認証トークンの一部として保存されます
* Adobe Pass Authenticationでは、プログラマーがこの情報を取得できるようにするためのAPIを提供しています
* プログラマーは、この機能をサイトまたはアプリに実装します（例えば、ユーザーの最大評価を超えるビデオを非表示にするため）

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### メモ {#notes-mvpd-progr-metadata-exch-flow}

**リソースの正規化と検証。** リソース IDは、プレーン文字列またはMRSS文字列として渡すことができます。 プログラマは、プレーン文字列書式またはMRSSのいずれかを使用できますが、MVPDがそのリソースの処理方法を把握できるように、MVPDとの事前の契約が必要です。

**リソース IDとメタデータの指定。** Adobe Pass認証では、Media RSS拡張機能を使用してRSS標準を使用し、リソースとそのメタデータを指定します。 Media RSS拡張機能と組み合わせることで、Adobe Pass Authenticationは、ペアレンタルコントロール（`<media:rating>`経由）や位置情報（`<media:location>`）など、様々なメタデータをサポートしています。

Adobe Pass Authenticationは、RSSを必要とするMVPDのレガシーチャネル文字列から対応するRSS リソースへの透過的な変換もサポートできます。 一方、Adobe Pass Authenticationでは、チャネルのみのMVPDに対して、RSS+MRSSからプレーンチャネルタイトルへの変換がサポートされます。

**Adobe Pass Authenticationは、既存の統合との完全な後方互換性を保証します。** つまり、チャンネルレベルの認証を使用するプログラマーの場合、Adobe Pass認証は、そのフォーマットを理解しているMVPDに送る前に、必要なフォーマットでチャネル IDをパッケージ化するように注意します。 その逆も同様です。プログラマが新しいフォーマットですべてのリソースを指定した場合、Adobe Pass Authenticationは、チャネルレベルの認証のみを行うMVPDに対して認証を行う場合、新しいフォーマットを単純なチャネル文字列に変換します。

## ユーザーメタデータの使用例 {#user-metadata-use-cases}

Mvpdが法的措置や機能追加を行うケースが増えるにつれて、ユースケースは常に変化し、拡大しています。 ユーザーメタデータの使用例を次に示します。

* [MVPD ユーザーID](#mvpd-user-id)
* [世帯ID](#household-user-id)
* [郵便番号](#zip-code)
* [最大評価（ペアレンタルコントロール）](#max-rating-parental-control)
* [チャネルラインナップ](#channel-line-up)

### MVPD ユーザーID {#mvpd-user-id}

* MVPDの提供する
* MVPDによってハッシュ化されるので、実際のログイン情報ではなく
* 特定のユーザーに関する問題を示すために使用できます
* 暗号化
* MVPDのサポート：すべてのMVPD

### 世帯ユーザーID {#household-user-id}

* 適切な指標を設定できる
* 暗号化
* MVPD サポート：一部のMVPD

### 郵便番号 {#zip-code}

* ユーザーの請求郵便番号
* 主に、スポーツイベントのフリーズ期間ルールを強制するために使用されます
* 迅速な更新のためにAuthZ応答を提供できます
* MVPD サポート：一部のMVPD

### 最大評価（ペアレンタルコントロール） {#max-rating-parental-control}

* 最初にAuthNを行い、AuthZを更新する
* UIからコンテンツをフィルタリング
* MPAAまたはVChip評価
* MVPD サポート：一部のMVPD

### チャネルラインナップ {#channel-line-up}

* MVPDは、ユーザーが表示できるチャネルのリストを提供できます
* クイック UI ペインティングを可能にする
* OLCA仕様では、これをAuthN応答のAttributeStatementとして使用できます
* MVPDのサポート：一部のMVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
