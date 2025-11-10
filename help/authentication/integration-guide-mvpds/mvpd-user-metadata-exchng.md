---
title: MVPD User Metadata Exchange
description: MVPD User Metadata Exchange
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# MVPD User Metadata Exchange

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#intro-user-metadata-exchange}

MVPD は、顧客に関するユーザー固有のメタデータを保持し、プログラマーと共有される場合もあります。 Adobe Pass Authentication の目的は、この「ユーザーメタデータ」の交換を仲介することですが、交換に関するあらゆる種類の規則を実施することではありません。 交換ルールは、MVPD がプログラマのパートナーと協力するためのものです。

Exchange で利用可能なユーザー・メタデータ・タイプには、現在、次のものが含まれます。

* 郵便番号
* 最大定格（VChip または MPAA）
* ユーザー ID
* 世帯 ID
* チャネル ID

この機能を使用すると、MVPD とプログラマーは、ペアレンタルコントロールなどの特別なユースケースを実装できます。 例えば、MVPDは、保護者によるレーティングのデータをプログラマーに渡し、プログラマーはそのデータを使用して、ユーザーが利用できる視聴オプションをフィルタリングできます。

ユーザーメタデータの重要なポイント：

* MVPDは、認証および承認フロー中に、ユーザーメタデータをプログラマーアプリケーションに渡します
* Adobe Pass認証では、メタデータ値を AuthN および AuthZ トークンに保存します
* Adobe Pass認証では、様々な形式のユーザーメタデータを提供する MVPD の値を正規化できます
* 一部のパラメーターは、プログラマーのキーを使用して暗号化できます
* 設定を変更すると、特定の値がAdobeで使用可能になります

>[!NOTE]
>
>ユーザーメタデータは、Adobe Pass Authentication で以前に使用可能だった静的メタデータ（認証トークン TTL、認証トークン TTL、デバイス ID）を拡張したものです。

## 例 {#example-mvpd-user-metadata-exch}

### 保護者による制限 {#example-parental-control}

この例では、次の交換を示します。

* [MVPD メタデータ交換のプログラマ](#progr-mvpd-metadata-exch)

* [MVPDからプログラマーへのメタデータ交換フロー](#mvpd-progr-exchange-flow)

### MVPD メタデータ交換のプログラマ {#progr-mvpd-metadata-exch}

現在、プログラマー API、Adobe Pass認証、MVPD認証機能はすべて、チャネルレベルの認証のみをサポートしています。 チャネルは、プログラマーの getAuthorization （） API 呼び出しで、プレーンテキストの文字列として指定されます。 この文字列は、MVPD認証バックエンドに最終的に生成されます。

プログラマーのアプリまたはサイトから、XACML 対応のMVPD（この例では「TNT」）を選択します。 XACML について詳しくは、「[eXtensible Access Control Markup Language](https://en.wikipedia.org/wiki/XACML){target=_blank}」を参照してください。
プログラマーのアプリが、リソースとそのメタデータを含む AuthZ リクエストを作成します。  この例では、チャネル要素の media 属性に「pg」の MPAA レーティングを含めています。

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

Adobe Pass認証は、実際には、MVPDとプログラマーの両方でサポートされる場合、アセットレベルに至るまで、より詳細な認証をサポートします。 リソースとそのメタデータはAdobeに対して不透明です。その目的は、リソース ID とメタデータを正規化された方法で指定する標準形式を確立し、リソース ID を異なる MVPD に送信することです。

>[!NOTE]
>
>ユーザーがチャンネルのみのMVPDを選択した場合、Adobe Pass認証は、チャネルタイトル（上記の例では「TNT」）のみを抽出し、タイトルのみをMVPDに渡します。

### MVPDからプログラマーへのメタデータ交換フロー {#mvpd-progr-exchange-flow}

Adobe Pass認証では、次のような前提があります。

* MVPDは、SAML 応答の一部として最大評価を送信します
* この情報は、認証トークンの一部として保存されます
* API は、プログラマーがこの情報を取得できるようにするためにAdobe Pass Authentication によって提供されます
* プログラマーは、この機能をサイトまたはアプリに実装します（例えば、ユーザーの最高評価を超えるビデオを非表示にするため）

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

### 備考 {#notes-mvpd-progr-metadata-exch-flow}

**リソースの正規化と検証。** リソース ID は、プレーン文字列または MRSS 文字列として渡すことができます。 プログラマーは、プレーンストリング形式または MRSS のいずれかを使用することにできますが、MVPDがそのリソースの処理方法を把握できるように、MVPDとの事前の合意が必要になります。

**リソース ID とメタデータの仕様。Adobe Pass認証**、RSS 標準とメディア RSS 拡張機能を使用して、リソースとそのメタデータを指定します。 Adobe Pass認証は、メディア RSS 拡張機能と組み合わせて、ペアレンタルコントロール（`<media:rating>` 経由）やジオロケーション（`<media:location>` 経由）など、様々なメタデータをサポートしています。

また、Adobe Pass認証では、RSS を必要とする MVPD の対応する RSS リソースへの、従来のチャネル文字列からの透過的な変換もサポートされます。 一方、Adobe Pass認証では、チャンネル専用の MVPD について、RSS+MRSS からプレーンチャネルタイトルへの変換をサポートしています。

**Adobe Pass認証により、既存の統合との完全な後方互換性が確保されます。** まり、チャンネルレベルの認証を使用するプログラマーの場合、Adobe Pass認証は、そのフォーマットを理解できるMVPDにチャンネル ID を送信する前に、必要なフォーマットでチャンネル ID をパッケージ化します。 逆も適用されます。プログラマーがすべてのリソースを新しいフォーマットで指定する場合、Adobe Pass認証は、チャネルレベルの認証のみを行うMVPDに対して認証を行う場合、新しいフォーマットを単純なチャネル文字列に変換します。

## ユーザーメタデータのユースケース {#user-metadata-use-cases}

法的取り決めや機能の追加を行う MVPD が増えるにつれ、ユースケースは変化し続け、拡大しています。 ユーザーメタデータの使用目的の例を以下に示します。

* [MVPD ユーザー ID](#mvpd-user-id)
* [世帯 ID](#household-user-id)
* [郵便番号](#zip-code)
* [最大レーティング （保護者による制限）](#max-rating-parental-control)
* [チャネルラインアップ](#channel-line-up)

### MVPD ユーザー ID {#mvpd-user-id}

* MVPDによって提供される
* MVPDによってハッシュ化されるので、ユーザーの実際のログイン情報ではありません
* 特定のユーザーに関するまたはの問題を示すために使用できます
* 暗号化
* MVPD サポート：すべての MVPD

### 世帯ユーザー ID {#household-user-id}

* 適切な指標情報を利用できます
* 暗号化
* MVPD サポート：一部の MVPD

### 郵便番号 {#zip-code}

* ユーザーの請求先郵便番号
* 主にスポーツイベント凍結期間ルールの実施に使用
* 迅速なアップデートのために、AuthZ 応答で提供可能
* MVPD サポート：一部の MVPD

### 最大レーティング （保護者による制限） {#max-rating-parental-control}

* 最初は AuthN、さらに AuthZ の更新
* UI からのコンテンツのフィルタリング
* MPAA または VChip の評価
* MVPD サポート：一部の MVPD

### チャネルラインアップ {#channel-line-up}

* MVPD は、ユーザーが閲覧できるチャネルのリストを提供できます
* クイック UI ペインティングを可能にします
* OLCA の仕様では、これを AuthN 応答の AttributeStatement として使用できます
* MVPD のサポート：一部の MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
