---
title: 事前認証
description: JavaScriptを事前認証
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1527'
ht-degree: 0%

---

# （レガシー）事前認証 {#js-preauthorize}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#preauth-overview}

事前認証 API メソッドは、アプリケーションが 1 つ以上のリソースに対して事前認証の決定を取得するために使用します。 事前認証 API リクエストは、UI ヒントやコンテンツフィルタリングに使用する必要があります。 指定したリソースへのユーザーアクセスを許可する前に、実際の認証 API リクエストを行う必要があります。

事前認証 API リクエストがAdobe Pass Authentication Services によって処理される際に、予期しないエラー（ネットワークの問題、MVPD認証エンドポイントが利用できないなど）が発生した場合は、事前認証 API 応答の結果の一部として、影響を受けるリソースに関する 1 つ以上のエラー情報が個別に含まれます。

### public preauthorize （request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>）: void {#preauth-method}

**説明：** この手法は、アプリケーションの UI を修飾する主な目的で（例えば、ロックおよびロック解除アイコンを使用したアクセスステータスの表示）、認証されたユーザーの事前認証（情報）決定をAdobe Pass認証サービスから取得して、特定の保護されたリソースを表示するために使用します。

**提供：** v4.4.0 以降

**パラメーター：**

* `PreauthorizeRequest`：リクエストの定義に使用されるビルダーオブジェクト
* `AccessEnablerCallback`:API 応答を返すために使用されるコールバック
* `PreauthorizeResponse`:API 応答のコンテンツを返すために使用されるオブジェクト

### preauthorizeRequestBuilder クラス {#preath-req-builder-class}

#### setResources （resources: string[]）: PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* 事前認証の決定を取得するリソースのリストを設定します。
* 事前認証 API を使用するには、これを設定する必要があります。
* リストの各要素は、MVPDと合意する必要があるリソース ID 値またはメディア RSS フラグメントを表す文字列である必要があります。
* このメソッドは、現在の `PreauthorizeRequestBuilder` オブジェクトインスタンス（このメソッド呼び出しの受信者）のコンテキストでのみ情報を設定します。

* 実際の `PreauthorizeRequest` を作成するには、`PreauthorizeRequestBuilder` のメソッドを参照します。

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` リソース。 事前認証の決定を取得するリソースのリスト。
* `@returns {PreauthorizeRequestBuilder}` メソッド呼び出しの受信者である、同じ `PreauthorizeRequestBuilder` オブジェクトインスタンスへの参照。
* これは、メソッドの連鎖の作成を可能にするために行います。

#### disableFeatures （...features: string[]）: PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* 事前認証の決定を取得するときに無効にする機能を設定します。
* この関数は、現在の `PreauthorizeRequestBuilder` オブジェクトインスタンス（この関数呼び出しの受信者）のコンテキストでのみ情報を設定します。
* 実際の `PreauthorizeRequest` を作成するには、`PreauthorizeRequestBuilder` の関数を確認します。

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` 機能。 無効にする機能のセット。
* `@returns` 関数呼び出しの受信者である、同じ `PreauthorizeRequestBuilder` オブジェクトインスタンスへの参照。
* これは、関数連結の作成を可能にするために行います。

#### build （）: PreauthorizeRequest {#preauth-req}

* 新しい `PreauthorizeRequest` オブジェクト インスタンスの参照を作成および取得します。
* このメソッドは、呼び出されるたびに新しい `PreauthorizeRequest` オブジェクトをインスタンス化します。
* このメソッドは、このメソッド呼び出しの受信者である現在の `PreauthorizeRequestBuilder` オブジェクトインスタンスのコンテキストで、事前に設定された値を使用します。
* この方法では副作用が生じないことを覚えておいてください。
* したがって、このメソッド呼び出しの受信者であるSDKのステートまたは `PreauthorizeRequestBuilder` オブジェクトインスタンスのステートは変更されません。
* つまり、同じ受信者に対してこのメソッドを連続して呼び出すと、新しい `PreauthorizeRequest` オブジェクトのインスタンスは異なりますが、呼び出し間で値が変更されていない `PreauthorizeRequestBuilder` に設定されている場合は、同じ情報を持ちます。
* 指定された情報（リソースとキャッシュ）を更新する必要がない場合は、事前認証 API の複数回使用するために、PreauthorizeRequest インスタンスを再利用できます。
* `@returns {PreauthorizeRequest}`

### インターフェイス AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse （result: T）; {#on-response-result}

* 事前認証 API リクエストが満たされたときにSDKによって呼び出された応答コールバック。
* 結果は、成功、またはステータスを含むエラー結果です。
* `@param {T} result`

#### onFailure （result: T）; {#on-failure-result}

* 事前認証 API リクエストを処理できなかった場合に、SDKによって呼び出されるエラーコールバック。
* 結果は、ステータスを含む失敗結果です。
* `@param {T} result`

### クラス PreauthorizeResponse {#preauth-response-class}

#### パブリックステータス：ステータス； {#public-status}

* 戻り値：失敗した場合の追加のステータス（状態）情報。
* `null` 値を保持する可能性があります。

#### パブリック決定：決定 []; {#public-decisions}

* 戻り値：事前認証の決定のリスト。 リソースごとに 1 つの決定。
* 失敗した場合、リストは空の可能性があります。

### クラスステータス {#class-status}

#### パブリックステータス：数値； {#public-status-numbr}

* RFC 7231 に記載されている HTTP 応答ステータスコード。
* `Status` がAdobe Pass Authentication Services ではなくSDKから送信される場合は、0 である可能性があります。

#### パブリックコード：number; {#public-code-numbr}

* 標準のAdobe Pass認証サービスエラーコード。
* 空の文字列または `null` の値を保持できます。

#### パブリックメッセージ：string; {#public-msg-string}

* 詳細なメッセージ。MVPD認証エンドポイントによって提供される場合もあれば、プログラマーの劣化規則によって提供される場合もあります。
* 空の文字列または `null` の値を保持できます。

#### パブリックの詳細：文字列； {#public-details-strng}

* 場合によっては、MVPD認証エンドポイントまたはプログラマー劣化ルールによって提供される詳細なメッセージを保持します。
* 空の文字列または `null` の値を保持できます。


#### public helpUrl: string; {#public-help-url-string}

* この状態/エラーが発生した理由と考えられる解決策に関する詳細情報にリンクする URL。
* 空の文字列または `null` の値を保持できます。

#### パブリックトレース：string; {#public-trace-string}

* この応答の一意の ID で、より複雑なシナリオで特定の問題を特定するためにサポートに連絡する際に使用できます。
* 空の文字列または `null` の値を保持できます。

#### パブリックアクション：string; {#public-action-string}

* 状況を改善するための推奨アクション。
   * **なし**：残念ながら、この問題を修正するための事前定義済みのアクションはありません。 これは、パブリック API の呼び出しが正しくない可能性があります
   * **設定**:TVE ダッシュボードを使用するか、サポートに連絡して、設定を変更する必要があります。
   * **application-registration**：アプリケーションは、自分自身を再度登録する必要があります。
   * **認証**：ユーザーは認証または再認証する必要があります。
   * **authorization**：ユーザーは、特定のリソースに対して認証を取得する必要があります。
   * **degradation**：何らかの形の最適化を適用する必要があります。
   * **retry**: リクエストを再試行すると、問題が解決する場合があります
   * **retry-after**：指定された期間が経過した後にリクエストを再試行すると、問題が解決する場合があります。
* 空の文字列または `null` の値を保持できます。

### クラスの決定 {#class-decision}

#### パブリック id：文字列； {#public-id-string}

* 決定が取得されたリソース ID。

#### public authorized：ブール値； {#public-auth-boolean}

* 決定が成功したかどうかを示すフラグの値。

#### パブリックエラー：ステータス； {#public-error-status}

* エラーが発生した場合の追加のステータス（状態）情報。 `null` 値を保持する可能性があります。

## クライアントの実装例 {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## シナリオの例 {#scenario-examples}

### シナリオ 1：要求されたすべてのリソースが許可された {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### シナリオ 2：リクエストされた一部のリソースが承認されました。 {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

    &quot;&#39;JavaScript
    
    &lbrace;
    &quot;decisions&quot;: &lbrack;
    &lbrace;
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    &rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    &rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    &rbrace;
    &rbrack;
    &rbrace;
    
    &quot;&#39;

</td>
  </tr>

<tr>
    <td>Enabled</td>
    <td>

    &quot;&#39;JavaScript
    &lbrace;
    &quot;decisions&quot;: &lbrack;
    &lbrace;
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    &rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: &lbrace;
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPDは、指定されたリソースの事前認証をリクエストする際に、\&quot;Deny\&quot;判断をを返しました。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot; &quot;none&quot;
    &rbrace;
    &rbrace;,
    &lbrace;
     
     
     
     
     
    
     
&quot;id&quot;: &quot;RES03&quot;,&quot;authorized&quot;: trueID&rbrace;，次の値を使用します
</td>
  </tr>
</tbody>


### シナリオ 3：リクエストされたリソースがすべて承認されなかった。 {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

    &quot;&#39;JavaScript
    
    &lbrace;
    &quot;decisions&quot;: &lbrack;
    &lbrace;
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false
    &rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    &rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false
    &rbrace;
    &rbrace;
    &rbrace;
    
    &quot;&#39;

</td>
  </tr>

<tr>
    <td>Enabled</td>
    <td>

    &quot;&#39;JavaScript
    
    &lbrace;
    &quot;decisions&quot;: &lbrack;
    &lbrace;
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;:false,
    &quot;error&quot;: &lbrace;
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPDは、指定されたリソースの事前認証をリクエストする際に\&quot;Deny\&quot;決定を返しました。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    &rbrace;
    &rbrace;&rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: &lbrace;
    &quot;status&quot;: 403,
     
     
     
     
     
     
     
     
     
     
     
     
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,and&quot;message&quot;: &quot;MVPDは、指定されたリソースの事前認証をリクエストする際に、\&quot;Deny\&quot;決定を返しました。&quot;,&quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,&quot;action&quot;: &quot;none&quot;403&quot;,error&quot;: &lbrace;preauthorization&quot;status&quot;: 403,correct&quot;code&quot;: &quot;maximum_execution_time_exceeded&quot;,&quot;message&quot;: &quot;リクエストが最大許容時間内に完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    &rbrace;
    &rbrace;
    &rbrace;
    &rbrace;
    
    &quot;&#39;

</td>
  </tr>
</tbody>


### シナリオ 4：無効なクライアントリクエスト – リソースが指定されていません。 {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    &quot;&#39;JavaScript
    &lbrace;
    &quot;status&quot;: &lbrace;
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;内部エラーが原因でリクエストが失敗しました。&quot;,
    &quot;details&quot;: &quot;Required String[] パラメーター&#39;resource&#39;が存在しません&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    &rbrace;,
    &quot;decisions&quot;: []
    &rbrace;
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>

### シナリオ 5：無効なクライアントリクエスト – 空のリソースが指定されました。 {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    &quot;&#39;JavaScript
    &lbrace;
    &quot;status&quot;: &lbrace;
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;The resource parameter is missing&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    &rbrace;,
    &quot;decisions&quot;: []
    &rbrace;
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>

### シナリオ 6：ネットワークエラー。 {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Enabled</td>
    <td>

    &quot;&#39;JavaScript
    &lbrace;
    &quot;decisions&quot;: &lbrack;
    &lbrace;
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: &lbrace;
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;関連するパートナーサービスから応答を取得する際に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    &rbrace;
    &rbrace;,
    &lbrace;
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: &lbrace;
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;関連するパートナーサービスから応答を取得する際に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    &rbrace;
    &rbrace;
    &rbrace;
    &rbrace;
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>

### シナリオ 7：有効な AuthN セッションなしで、事前承認フローが呼び出された。

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    &quot;&#39;JavaScript
    &lbrace;
    &quot;status&quot;: &lbrace;
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;このリクエストに関連付けられた認証セッションを取得できませんでした。 続行するには、サポートされているMVPDで再認証する必要があります。&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    &rbrace;,
    &quot;decisions&quot;: []
    &rbrace;
    
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>



### シナリオ 8:setRequestor 呼び出しが完了する前に、事前承認フローが呼び出された

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    &quot;&#39;JavaScript
    &lbrace;
    &quot;status&quot;: &lbrace;
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;setRequestor API 以外の API を使用するための前提条件である、リクエスターがまだ設定されていません。&quot;,
    &quot;action&quot;: &quot;retry&quot;
    &rbrace;,
    &quot;decisions&quot;: []
    &rbrace;
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>
