---
title: プリフライト認証
description: プリフライト認証
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# プリフライト認証 {#preflight-authorization}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

## 概要 {#overview}

この機能は、複数のリソースに対する軽量な認証チェックを提供します。 このライトウェイトチェックの目的は、UI を装飾することです（例えば、ロックアイコンとロック解除アイコンでアクセスステータスを示す）。 プリフライト認証は、できるだけ軽量で効率的なもので、1 回の API 呼び出しでリソースのリストの認証ステータスが生成されます。 この機能は、リソースの認証に関して権限を持つものではありません。

A `getAuthorization(resource)` または `checkAuthorization(resource)` 再生を許可する前に、呼び出しを実行する必要があります。

プリフライト認証は、別の使用例に対するサポートも提供します。この場合、プログラマーは、メディアコンテンツの 1 項目の再生を許可するために、複数のリソース ID に対する認証をリクエストする必要があります。 プログラマーは、必要なリソースに対して初期のプリフライトチェックを行うことができ、応答に応じて、ビジネス条件が満たされない場合は、早期に失敗する可能性があります。

プリフライト認証をサポートする MVPD のリストについては、 [MVPD Preflight Authorization](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) ページに貼り付けます。

>[!NOTE]
>
> 完全なプリフライト認証サポートを持たない MVPD に対するこの機能の使用は、Adobeおよび MVPD と事前に合意する必要があることに注意してください。 これらの MVPD でのプリフライト認証の使用は、説明されている「最悪のシナリオ」に該当します。 [ここ](/help/authentication/mvpd-preflight-authz.md#intro) とを使用すると、パフォーマンスの問題が発生し、応答時間が遅くなる可能性があります。 </br>
> また、 **Adobeによって 5 つ以上のリソースに対して明示的に合意が必要**.

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler は、プリフライト認証を実装するための API/コールバック関数ペアを公開します。 API 呼び出しは、リソースのリストから成る単一のパラメーターを受け取ります。 このコールバック関数は、実際に許可されているリソースを表す単一のパラメーターを受け取ります。 次の例はActionScriptですが、呼び出しは AccessEnabler のすべての機能で利用できます。

### checkPreauthorizedResources(Array:resources):void {#checkPreauthRes}

AccessEnabler オブジェクトでこの関数を呼び出して、リソースの一覧の認証状態を要求します。

resources パラメータは、承認を確認する必要があるリソースのリストです。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、 `getAuthorization()` 呼び出し、すなわち、プログラマーと MVPD の間に確立された値、またはメディア RSS フラグメントに基づいて合意される。 Adobe Pass認証は、MVPD が実際にサポートする内容に応じてリソース形式を変換できる薄い仲介レイヤーを除き、リソースを管理しません。

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

これは、プログラマの上位層アプリケーションに実装する必要があるコールバック関数です。 AccessEnabler は、認証済みリソースの一覧を計算した後にこの関数を呼び出します。


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

API 呼び出しは、現在のユーザーの許可されたリソースのキャッシュされたリストをクライアントのローカルストレージで検索しようとします。 キャッシュされたリストがない場合は、AdobePass サーバーに対して HTTPS 呼び出しがおこなわれ、リストが取得されます。

キャッシュメカニズムは、ネットワーク呼び出しを完全にスキップすることで、後続の呼び出しでのパフォーマンス時間を改善します。 また、認証プロセスの一環として、キャッシュされたリストを事前に設定することもできます。  ( このシナリオの設定について詳しくは、 [プリフライト認証の統合](/help/authentication/authz-usecase.md#preflight_authz_int) （『MVPD 統合ガイド』の「認証」セクション）。

また、キャッシュされたリソースのリストは、キャッシュされたリソースのリストが存在する場合、承認フローを最適化するために使用される可能性があります。 `checkAuthorization()` は、ネットワーク呼び出しを実行する前に確認できます。 リソースが事前に認証されたリソースのリストに含まれていない場合は、Adobe Pass認証サーバーを呼び出す必要なく、このチェックが失敗する可能性があります。


### ChannelID を使用したプリフライト {#preflight_using_channelID}

Adobe Pass Authentication 2.4.1 リリース以降、プリフライトフローは次のように機能します。

1. 認証時に、Adobe Pass Authentication は `channelIID` MVPD の SAML 応答からの要素。この値を使用して `authorizedResources` 要素を認証トークンに追加しました。
1. 内部 `checkPreauthorizedResources()` API 関数、Adobe Pass Authentication は、 `authorizedResources` 要素が設定されている。
1. 次の場合、 `authorizedResources` 要素が設定されている場合、Adobe Pass Authentication はその値を読み取り、 `authorizedResources` 要素と、から受け取ったリソースのリスト `checkPreauthorizedResources()` パラメーター。  この積集合の結果、事前に認証されたリソースの最後のリストが表示されます。
1. 次の場合、 `authorizedResources` 要素が設定されていない場合は、以前に実装されたフローを実行します。このフローでは、 `checkPreauthorizedResources()` パラメーターが PreAuthorizationServlet に渡されます。 このサーブレットは、MVPD エンドポイントへの認証呼び出しを実行し、事前に認証されたリソースのリストを返します。

### ChannelID を使用したプリフライトの例

次の例は、チャネルのラインアップの例を示しています。 チャネルのリストを送信する際に使用する属性の名前は、MVPD 間で異なる場合があることに注意してください。

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


The `authorizedResources` 認証要素の要素は、次のように表示されます。

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

プログラマーは、 `checkPreauthorizedResources()` 次のパラメーターのリストを渡す API 呼び出し。</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;
- &quot;fbc-fox&quot;

現在のプリフライト実装は、 `authorizedResources` 要素を含め、このリストを返します。

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;



**注意：** 積集合では大文字と小文字が区別されないことに注意してください。



### POST/事前認証 {#post}

この呼び出しは、キャッシュ、許可、リソース・リストが存在しない場合に、AccessEnabler によって自動的に実行されます。


#### リクエスト {#req}

| パラメーター | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `authentication_token` | 文字列 | はい | 認証トークン。 |
| `resource_id` | 文字列 | はい | 単一のリソース。 これは、 `checkPreauthorizedResources()` API 呼び出し。 |


**注意：** 要求されたリソースの最大数は設定可能です。
**デフォルト値の最大値は 5 です。**


#### 応答 {#resp}

事前認証サーブレットが返す応答の形式は次のとおりです。

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

AccessEnabler がサービス・プロバイダから取得する、事前に認証されたリソースのリスト。 次のリソースのリスト：

- AuthN および AuthZ トークンと共に保存されます。
- ユーザーが同じ Web サイト上にいる限り、または AuthN トークンの有効期限が切れるまで有効です。
- ユーザーが新しいAdobe Pass認証統合 Web サイトに到達するたびに再取得されます。

各エントリには、ユーザーが事前に認証されているリソース ID が含まれます。

例：


| リソース ID | 認証済み |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


このリストは、「事前認証キャッシュ」という名前です。

#### 流量 {#flow}

1. プログラマーのアプリ/サイトが `checkPreauthorizedResources(resourceList)` を呼び出します。
1. AccessEnabler は、認証済みリソースの認証トークンを検証します。
   1. 認証トークンに認証済みのリソースが含まれている場合。このリストは権限を持っており、この情報を取得するために呼び出しをおこなう必要はありません。 resourceList のリソースは、認証トークン上の許可されたリソースのリスト内で検索され、見つかったリソースのみが `preauthorizedResources()` コールバック。
   1. 認証トークンに認証済みリソースが含まれていない場合 — `resourceList` は、事前認証キャッシュ内のリソースのリストと比較されます。
      1. リストに同じリソースが含まれる場合（サーバーへの呼び出しが既におこなわれ、応答は既に事前認証キャッシュに含まれている）。 許可されたリソースのみが `preauthorizedResources()` コールバック。
      1. リストに同じリソースが含まれていない場合 — クライアントは、resourceList 内のリソースの認証状態を取得するために、サーバーを呼び出す必要があります。 応答は取得され、事前認証キャッシュに保存され、古いリソースが完全に置き換えられます。 許可されたリソースのみが `preauthorizedResources()` コールバック。


#### リスト取得 {#listRetrieve}

常に `checkPreauthorizedResources()` が呼び出されると、認証を検証するリソースのリストが事前認証キャッシュと照合されます。 リストに同じリソースのセットが含まれる場合、サービスプロバイダーは呼び出されません。これは、 `preauthorizedResources()` コールバックは既にキャッシュに存在します。


#### logout() {#logout}

事前認証キャッシュはログアウト時に空になります。


## 依存関係 {#depends}

プリフライト API のパフォーマンスは、特定の MVPD 実装に依存します。  実装オプションについては、 [プリフライト認証の統合](/help/authentication/authz-usecase.md#preflight_authz_int) （MVPD 統合ガイドの「認証」セクション）を参照してください。


## セキュリティ {#security}

クライアント API は、すべてのプログラマーが利用できます。

この実装はトランスポートとして HTTPS を使用しますが、より軽いコールを得るために、追加のセキュリティ対策は採用されません（署名なし、FAXS なし）。

**注意：** 保護されたリソースへのアクセスをユーザーに許可する必要があるかどうかを判断するために、この API を正式な方法で使用しないでください。 この API の目的は、UI 装飾やビジネス上の意思決定の事前設定です。 The `getAuthorization()` および `checkAuthorization()` の呼び出しは、再生を許可する前におこなう必要があります。


## 互換性 {#compat}

この機能は、AS、JS、AIR、iOS、Android、Xbox（2 次画面 AuthN フロー）など、AccessEnabler のすべての機能でサポートされています。

プリフライト認証では、CDATA セクションを含むリソースの事前認証はサポートされません。 現在のプリフライトシステムの焦点は、チャネルレベルのフィルタリングをサポートすることです。 CDATA セクションを持つリソースは、おそらくアセットレベルのリソースです。 プリフライトはシンプルをサポートしています `mrss` CDATA が含まれていない限り、チャネルレベルの事前認証のリソース。

## 他の機能との統合 {#integ_w_other_features}

リソースが事前に許可されたリソースのリストにない場合に失敗するために、承認フローで最適化が可能です。

この最適化では、サーバのプリフライトエンドポイントは、[ 低下 ] メカニズムと統合されます。

MVPD および Requestor に対して「AuthN All」ルールが設定されている場合、プリフライトエンドポイントは単にリクエストで受け取ったすべてのリソースをミラーバックします。

リクエストに存在するリソースの少なくとも 1 つに対して「AuthZ All」が有効になっている場合も、同じことが言えます。

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
