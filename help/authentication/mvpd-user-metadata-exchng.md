---
title: MVPD ユーザメタデータ交換
description: MVPD ユーザメタデータ交換
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 0%

---

# MVPD ユーザメタデータ交換

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#intro-user-metadata-exchange}

MVPDs は、場合によっては Programmers と共有される顧客に関するユーザ固有のメタデータを維持します。 Adobe Pass Authentication の目的は、この「ユーザーメタデータ」の交換を仲介することですが、交換に関するあらゆる種類のルールを実施することではありません。 交換規則は、MVPD がプログラマーパートナーと連携するためのものです。

Exchange で現在使用できるユーザーメタデータタイプは次のとおりです。

* 郵便番号
* 最大評価（VChip または MPAA）
* ユーザー ID
* 世帯 ID
* チャネル ID

この機能を使用して、MVPD およびプログラマーは、親の制御などの特殊な使用例を実装できます。 例えば、MVPD は親の格付けデータをプログラマーに渡し、そのデータを使用して、ユーザーに対して使用可能な表示選択をフィルタリングすることができます。

ユーザーメタデータのキーポイント：

* MVPD は、認証および承認フロー中に、ユーザメタデータをプログラマのアプリケーションに渡します
* Adobe Pass認証は、メタデータ値を AuthN および AuthZ トークンに保存します。
* Adobe Pass認証では、異なる形式のユーザーメタデータを提供する MVPD の値を正規化できます
* 一部のパラメータは、プログラマーのキーを使用して暗号化できます
* 特定の値は、設定の変更を通じてAdobeが使用できるようになります。

>[!NOTE]
>
>ユーザーメタデータは、Adobe Pass認証で以前に使用可能だった静的メタデータ（認証トークン TTL、認証トークン TTL、デバイス ID）の拡張です。

## 例 {#example-mvpd-user-metadata-exch}

### 親の制御 {#example-parental-control}

この例では、次の変更を示します。

* [MVPD メタデータ交換に対するプログラマー](#progr-mvpd-metadata-exch)

* [MVPD からプログラマーメタデータ交換フロー](#mvpd-progr-exchange-flow)

### MVPD メタデータ交換に対するプログラマー {#progr-mvpd-metadata-exch}

現在、Programmer API、Adobe Pass Authentication、および MVPD Authorizers はすべて、チャネルレベルの認証のみをサポートしています。 このチャネルは、プログラマーの getAuthorization() API 呼び出しでプレーンテキスト文字列として指定されます。 この文字列は、MVPD の認証バックエンドにすべて伝播されます。

プログラマーのアプリまたはサイトから、ユーザーは XACML 対応の MVPD （この例では「TNT」）を選択します。 XACML について詳しくは、 [拡張アクセス制御マークアップ言語](https://en.wikipedia.org/wiki/XACML){target=_blank}.
プログラマーのアプリは、リソースとそのメタデータを含む AuthZ リクエストを作成します。  この例では、チャネル要素のメディア属性に「pg」の MPAA 評価が含まれています。

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

Adobe Pass認証は、MVPD とプログラマーの両方でサポートされている場合、アセットレベルまで、より詳細な認証を実際にサポートします。 リソースとそのメタデータはAdobeに対して不透明です。目的は、リソース ID とメタデータを正規化された方法で指定し、リソース ID を別の MVPD に送信するための標準形式を確立することです。

>[!NOTE]
>
>ユーザーがチャネルのみに対応する MVPD を選択した場合、Adobe Pass認証は、チャネルタイトル（上記の例では「TNT」）のみを抽出し、タイトルのみを MVPD に渡します。

### MVPD からプログラマーメタデータ交換フロー {#mvpd-progr-exchange-flow}

Adobe Pass認証では、次のことを前提としています。

* MVPD は、SAML 応答の一部として最大評価を送信します。
* この情報は、認証トークンの一部として保存されます
* プログラマーがこの情報を取得できるように、Adobe Pass Authentication によって API が提供されています
* プログラマーは、サイトやアプリにこの機能を実装します（例えば、ユーザーの最大評価を超えるビデオを非表示にする場合）。

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

**リソースの正規化と検証。** リソース ID は、プレーン文字列または MRSS 文字列として渡すことができます。 プログラマーは、プレーン文字列形式または MRSS のどちらを使用するかを決定できますが、MVPD がそのリソースの処理方法を知るために、MVPD との事前の合意が必要になります。

**リソース ID とメタデータの指定。** Adobe Pass認証では、RSS 標準とメディア RSS 拡張機能を使用して、リソースとそのメタデータを指定します。 Adobe Pass認証は、メディア RSS 拡張機能と組み合わせて、保護者による制限など、様々なメタデータをサポートします（を介して）。 `<media:rating>`) または geolocation (`<media:location>`) をクリックします。

また、Adobe Pass認証は、RSS を必要とする MVPD に対して、従来のチャネル文字列から対応する RSS リソースへの透明な変換をサポートすることもできます。 一方、Adobe Pass認証では、RSS+MRSS からプレーンチャネルタイトルへの変換（チャネルのみの MVPD）がサポートされています。

**Adobe Pass認証は、既存の統合との完全な後方互換性を確保します。** つまり、チャネルレベルの認証を使用するプログラマーの場合、Adobe Pass Authentication は、その形式を認識する MVPD にチャネル ID を送信する前に、必要な形式にチャネル ID をパッケージ化するように配慮します。 この逆も同様です。プログラマーがすべてのリソースを新しい形式で指定した場合、Adobe Pass Authentication は、チャネルレベルの認証のみを行う MVPD に対して認証を行う場合、新しい形式を単純なチャネル文字列に変換します。

## ユーザーメタデータの使用例 {#user-metadata-use-cases}

法律上の調整や機能の追加を行う MVPD が増えるにつれ、使用例は常に変化し、拡大しています。 次に、使用できるユーザーメタデータの例を示します。

* [MVPD ユーザー ID](#mvpd-user-id)
* [世帯 ID](#household-user-id)
* [郵便番号](#zip-code)
* [最大評価（親による制御）](#max-rating-parental-control)
* [チャネルラインアップ](#channel-line-up)

### MVPD ユーザー ID {#mvpd-user-id}

* MVPD によって提供される
* MVPD によってハッシュ化されているので、ユーザーの実際のログイン情報ではありません。
* 特定のユーザーに関する問題を示すために、またはに関する問題を示すために使用できます。
* 暗号化済み
* MVPD のサポート：すべての MVPD

### 世帯ユーザー ID {#household-user-id}

* 良好な指標情報を実現
* 暗号化済み
* MVPD のサポート：一部の MVPD

### 郵便番号 {#zip-code}

* ユーザーの請求の郵便番号
* 主にスポーツイベントの凍結期間ルールの実施に使用されます
* 高速更新に対する AuthZ 応答を提供可能
* MVPD のサポート：一部の MVPD

### 最大評価（親による制御） {#max-rating-parental-control}

* AuthN は最初に、AuthZ の更新を追加
* UI からコンテンツをフィルタリング
* MPAA または VChip 評価
* MVPD のサポート：一部の MVPD

### チャネルラインアップ {#channel-line-up}

* MVPDs は、ユーザーが表示する権限を持つチャネルのリストを提供できます。
* UI のペイントをすばやく行うことができます。
* OLCA の仕様では、AuthN 応答で AttributeStatement としてこれを使用できます
* MVPD のサポート：一部の MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
