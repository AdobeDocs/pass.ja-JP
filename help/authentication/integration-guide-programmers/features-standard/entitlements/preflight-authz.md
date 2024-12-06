---
title: プリフライト認証
description: プリフライト認証
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# プリフライト認証 {#preflight-authorization}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

## 概要 {#overview}

この機能は、複数のリソースに対して軽量の認証チェックを提供します。 この軽量チェックの目的は、UI を装飾することです（例えば、ロックアイコンとロック解除アイコンでアクセスステータスを示すなど）。 プリフライト認証は、1 回の API 呼び出しでリソースのリストの認証ステータスが得られるなど、できるだけ軽量で効率的です。 この機能は、リソースの承認に関して権限がないことに注意してください。

再生を許可する前に、`getAuthorization(resource)` または `checkAuthorization(resource)` の呼び出しを行う必要があります。

プリフライト認証は、プログラマーが 1 つのメディアコンテンツ項目を再生するために複数のリソース ID の認証をリクエストする必要がある様々なユースケースにも対応しています。 プログラマーは必要なリソースに対して最初のプリフライトチェックを行うことができ、ビジネス条件が満たされない場合は、応答に応じて早期に失敗する可能性があります。

プリフライト認証をサポートする MVPD のリストについては、[MVPD Preflight Authorization](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight_support_list) ページを参照してください。

>[!NOTE]
>
> プリフライト認証の完全なサポートを持たない MVPD に対してこの機能を使用する場合は、事前にAdobeと MVPD に同意する必要があることに注意してください。 これらの MVPD でプリフライト認証を使用すると、[ こちら ](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#intro) で説明した「最悪のシナリオ」に分類され、パフォーマンスの問題が発生して応答時間が遅くなる可能性があります。</br>
> また、**5 つを超えるリソースのプリフライト認証の使用は、Adobeが明示的に同意する必要があります**。

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler は、プリフライト認証を実装するための API/コールバック関数ペアを公開します。 API 呼び出しは、リソースのリストで構成される 1 つのパラメーターを受け取ります。 コールバック関数は、実際に許可されたリソースを表す単一のパラメーターを受け取ります。 次の例はActionScript中ですが、呼び出しは AccessEnabler のすべてのフレーバーで使用できます。

### checkPreauthorizedResources （配列：リソース）:void {#checkPreauthRes}

AccessEnabler オブジェクトでこの関数を呼び出して、リソースのリストの認証ステータスを要求します。

resources パラメーターは、認証を確認する必要があるリソースのリストです。 リストの各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、`getAuthorization()` 呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマーと MVPD またはメディア RSS フラグメントの間で確立された値に対して合意されます。 Adobe Pass Authentication は、MVPD が実際にサポートする内容に応じてリソース形式を変換できるシンメディエーションレイヤーを除き、リソースを管理しません。

### preauthorizedResources （配列：authorizedResources） {#preauthRes}

これは、プログラマーの上位レイヤーアプリケーションに実装する必要があるコールバック関数です。 AccessEnabler は、許可されたリソース・リストを計算した後、この関数を呼び出します。


### JS の例

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## 実装情報 {#details}

- [ChannelID を使用したプリフライト](#preflight_using_channelID)
- [POST/事前認証](#post)
- [ストレージ](#storage)

API 呼び出しは、クライアントのローカルストレージ内の現在のユーザー用に許可されたリソースのキャッシュされたリストの検索を試みます。 キャッシュされたリストがない場合は、AdobePass サーバーに対して HTTPS 呼び出しが実行されて、リストが取得されます。

キャッシュメカニズムにより、ネットワーク呼び出しを完全にスキップすることで、以降の呼び出しのパフォーマンス時間が向上します。 また、キャッシュされたリストは、認証プロセスの一環として事前に入力できます。  （このシナリオのセットアップについては、『 MVPD 統合ガイド』の「認証」セクションの [ プリフライト認証統合 ](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int) を参照してください。

さらに、キャッシュされたリソースリストを使用すると、キャッシュされたリソースリストが存在する場合は、ネットワーク呼び出しを行う前に確認できるとい `checkAuthorization()` 意味で、認証フローを最適化できます。 リソースが事前承認済みリソースのリストに含まれていない場合、Adobe Pass Authentication Server を呼び出す必要なく、チェックが失敗する可能性があります。


### ChannelID を使用したプリフライト {#preflight_using_channelID}

Adobe Pass Authentication 2.4.1 リリース以降、プリフライトフローは次のように動作します。

1. 認証中、Adobe Pass Authentication は MVPD の SAML 応答から `channelIID` 要素を読み取り、この値を使用して認証トークンに `authorizedResources` 要素を設定します。
1. `checkPreauthorizedResources()` API 関数内で、Adobe Pass認証は `authorizedResources` 要素が設定されているかどうかを確認します。
1. `authorizedResources` 要素が設定されている場合、Adobe Pass認証はその値を読み取り、`authorizedResources` 要素のリソースリストとパラメーターから受信したリソースリストの間で intersect`checkPreauthorizedResources()` 実行します。  この積集合の結果が、事前承認されたリソースの最終リストになります。
1. `authorizedResources` 要素が設定されていない場合は、以前に実装されたフローを実行します。このフローで、`checkPreauthorizedResources()` のパラメーターから受け取ったリソースのリストが PreAuthorizationServlet に渡されます。 このサーブレットは、MVPD エンドポイントへの認証呼び出しを実行し、事前に許可されたリソースのリストを返します。

### ChannelID を使用したプリフライトの例

次の例に、チャネルのラインアップの例を示します。 チャネルのリストを送信するために使用される属性の名前は、MVPD によって異なる場合があることに注意してください。

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


認証要素の `authorizedResources` 要素は次のようになります。

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

プログラマーは、次のパラメーターのリストを渡して、`checkPreauthorizedResources()` API 呼び出しを実行します。</span>

- &quot;MSNBC&quot;
- 「FBN」
- &quot;TruTV&quot;
- 「fbc-fox」

現在の Preflight 実装は、`authorizedResources` 要素からのリソースリストと積集合を実行し、このリストを返します。

- &quot;MSNBC&quot;
- 「FBN」
- &quot;TruTV&quot;



**注：** 積集合では大文字と小文字が区別されないことに注意してください。



### POST/事前認証 {#post}

この呼び出しは、キャッシュされた承認済みリソース リストが存在しない場合に、AccessEnabler によって自動的に実行されます。


#### リクエスト {#req}

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `authentication_token` | string | はい | 認証トークン。 |
| `resource_id` | string | はい | 単一のリソース。 これは、`checkPreauthorizedResources()` API 呼び出しで指定されたリソース配列の要素ごとに 1 回ずつ、複数回指定できます。 |


**メモ：** リクエストされるリソースの最大数は設定可能です。
**デフォルト値の最大値は 5.** です


#### 応答 {#resp}

事前認証サーブレットが返す応答には、次の形式があります。

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### ストレージ {#storage}

AccessEnabler がサービス プロバイダから取得する事前承認済みリソースのリスト。 リソースのリスト：

- AuthN および AuthZ トークンと共に格納されます。
- は、ユーザーが同じ web サイト上にある限り、または
AuthN トークンの有効期限
- ユーザーが新しいAdobe Passにアクセスするたびに再取得されます。
認証統合 web サイト

各エントリには、ユーザーが事前に認証されているリソース ID が含まれています。

例：


| リソース ID | Authorized |
| ----------- | ---------- |
| CNN | true |
| TNT | 偽 |
| ... | ... |


このリストの名前は「事前認証キャッシュ」です。

#### フロー {#flow}

1. プログラマーのアプリ/サイトが `checkPreauthorizedResources(resourceList)` 呼び出しを行います。
1. AccessEnabler は、許可されたリソースの認証トークンを検証します。
   1. 認証トークンに承認済みリソースが含まれている場合、このリストは権限があるので、この情報を取得するために呼び出しを行う必要はありません。 resourceList のリソースは、認証トークン上の承認済みリソースのリスト内で検索され、見つかったリソースのみが `preauthorizedResources()` コールバックによって返されます。
   1. 認証トークンに承認済みリソースが含まれていない場合、`resourceList` は事前認証キャッシュ内のリソースのリストと比較されます。
      1. リストに同じリソースが含まれている場合 – サーバーへの呼び出しが既に行われ、応答が既に事前認証キャッシュ内にあることを意味します。 許可されたリソースのみが、`preauthorizedResources()` コールバックによって返されます。
      1. リストに同じリソースが含まれていない場合、クライアントは resourceList のリソースの認証状態を取得するためにサーバーを呼び出す必要があります。 応答が取得され、事前認証キャッシュに保存されるので、古いリソースが完全に置き換えられます。 許可されたリソースのみが、`preauthorizedResources()` コールバックによって返されます。


#### リスト取得 {#listRetrieve}

`checkPreauthorizedResources()` が呼び出されるたびに、認証を検証するリソースのリストが事前認証キャッシュと照合されます。 リストに同じ一連のリソースが含まれている場合、`preauthorizedResources()` コールバックのトリガーに必要なすべてのリソースが既にキャッシュに存在するので、サービスプロバイダーへの呼び出しは行われません。


#### logout （） {#logout}

事前認証キャッシュは、ログアウト時に空になります。


## 依存関係 {#depends}

Preflight API のパフォーマンスは、特定の MVPD 実装によって異なります。  実装オプションについては、『 MVPD 統合ガイド』の「認証」セクションの [Preflight Authorization Integration](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int) を参照してください。


## セキュリティ {#security}

クライアント API は、すべてのプログラマーが使用できます。

実装ではトランスポートとして HTTPS を使用しますが、呼び出しを軽くするために、追加のセキュリティ対策は採用されません（署名や FAXS は使用されません）。

**注意：** この API を、保護されたリソースへのアクセスをユーザーに許可するかどうかを決定する権限のある方法で使用しないでください。 この API の目的は、UI の装飾や、ビジネス上の決定の事前設定です。 `getAuthorization()` と `checkAuthorization()` の呼び出しは、再生を許可する前に行う必要があります。


## 互換性 {#compat}

この機能は、AccessEnabler のすべてのフレーバー（AS、JS、AIR、iOS、Android、Xbox （第 2 画面 AuthN フロー内）でサポートされます。

プリフライト認証では、CDATA セクションを含むリソースの事前認証はサポートされていません。 現在のプリフライトシステムの焦点は、チャネルレベルのフィルタリングをサポートすることです。 CDATA セクションを持つリソースは、アセットレベルのリソースである可能性が高いです。 プリフライトは、CDATA が含まれていない限り、チャネルレベルの事前認証のためのシンプルな `mrss` リソースをサポートします。

## 他の機能との統合 {#integ_w_other_features}

リソースが事前承認されたリソースのリストに含まれていない場合は、すばやく失敗するために、承認フローで最適化が行われる可能性があります。

この最適化では、サーバーのプリフライトエンドポイントが低下メカニズムと統合されます。

MVPD とリクエスターに「AuthN All」ルールが設定されている場合、プリフライトエンドポイントはリクエストで受信したすべてのリソースを単純にミラーバックします。

リクエストに存在する 1 つ以上のリソースに対して「AuthZ All」が有効になっている場合も同じことが当てはまります。

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
