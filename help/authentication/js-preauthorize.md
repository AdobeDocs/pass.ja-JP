---
title: 事前認証
description: JavaScript の事前認証
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1499'
ht-degree: 0%

---

# 事前認証 {#js-preauthorize}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#preauth-overview}

事前認証 API メソッドは、1 つ以上のリソースの事前認証決定を取得するためにアプリケーションで使用されます。 UI ヒントやコンテンツのフィルタリングには、事前認証 API リクエストを使用する必要があります。 ユーザーが指定したリソースへのアクセスを許可する前に、実際の認証 API リクエストを実行する必要があります。

Adobe Pass Authentication サービスで Preauthorize API リクエストが処理されると予期しないエラー（ネットワークの問題、MVPD 認証エンドポイントが使用できないなど）が発生した場合、影響を受けるリソースに対して 1 つ以上の個別のエラー情報が Preauthorize API 応答の一部として含まれます。

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>):void {#preauth-method}

**説明：** この方法は、アプリケーションの UI を修飾する（例えば、アクセス状態をロックアイコンとロック解除アイコンで示す）ために、認証済みのユーザーの事前認証（情報）決定をAdobe Pass Authentication サービスから取得するために使用されます。

**可用性：** v4.4.0 以降

**パラメーター：**

* `PreauthorizeRequest`：リクエストの定義に使用するビルダーオブジェクト。
* `AccessEnablerCallback`:API 応答を返すために使用されるコールバック
* `PreauthorizeResponse`:API 応答コンテンツを返すために使用されるオブジェクト。

### クラス PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* 事前認証の決定を取得するリソースのリストを設定します。
* 事前認証 API の使用には設定する必要があります。
* リスト内の各要素は、リソース ID の値、または MVPD と合意する必要があるメディア RSS フラグメントを表す文字列型 ( String ) の値である必要があります。
* このメソッドは、現在の `PreauthorizeRequestBuilder` オブジェクトインスタンス（このメソッド呼び出しのレシーバー）。

* 実際の `PreauthorizeRequest` ご覧になれます `PreauthorizeRequestBuilder`&#39;s メソッド：

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` リソース。 事前認証の決定を取得するリソースのリスト。
* `@returns {PreauthorizeRequestBuilder}` 同じ `PreauthorizeRequestBuilder` オブジェクトインスタンス。メソッド呼び出しのレシーバーです。
* これは、メソッドチェーンを作成できるようにするためです。

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* 事前認証の決定を取得する際に、機能を無効にする機能を設定します。
* この関数は、現在の `PreauthorizeRequestBuilder` オブジェクトインスタンス。この関数呼び出しのレシーバーです。
* 実際の `PreauthorizeRequest` ご覧になれます `PreauthorizeRequestBuilder`&#39;s 関数：

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` 機能。 無効にする一連の機能。
* `@returns` 同じ `PreauthorizeRequestBuilder` オブジェクトインスタンス。関数呼び出しのレシーバーです。
* これは、関数のチェーンを作成できるようにするために実行されます。

#### build(): PreauthorizeRequest {#preauth-req}

* 新しいの参照を作成し、取得します `PreauthorizeRequest` オブジェクトインスタンス。
* このメソッドは、新しい `PreauthorizeRequest` オブジェクトを呼び出すたびに、オブジェクトを呼び出します。
* このメソッドは、現在の `PreauthorizeRequestBuilder` オブジェクトインスタンス（このメソッド呼び出しのレシーバー）。
* この方法は副作用を生み出さないことに留意してください。
* したがって、SDK の状態や `PreauthorizeRequestBuilder` オブジェクトインスタンス（このメソッド呼び出しのレシーバー）。
* つまり、同じレシーバーに対してこのメソッドを連続して呼び出すと、新しい呼び出しが異なります `PreauthorizeRequest` オブジェクトインスタンスですが、同じ情報を持ちます ( 値が `PreauthorizeRequestBuilder` 呼び出しの間に変更されない。
* 提供された情報（リソースおよびキャッシュ）を更新する必要がない場合は、事前認証 API を複数使用する際に PreauthorizeRequest インスタンスを再利用できます。
* `@returns {PreauthorizeRequest}`

### インタフェース AccessEnablerCallback&lt;t> {#interface-access-enablr-callback}

#### onResponse(result: T); {#on-response-result}

* 事前認証 API 要求が満たされたときに SDK によって呼び出される応答コールバック。
* 結果は、成功した結果か、ステータスを含むエラーの結果のどちらかです。
* `@param {T} result`

#### onFailure(result: T); {#on-failure-result}

* 事前認証 API 要求を処理できなかった場合に SDK が呼び出す失敗コールバック。
* 結果は、ステータスを含む失敗の結果です。
* `@param {T} result`

### PreauthorizeResponse クラス {#preauth-response-class}

#### public status: Status; {#public-status}

* 戻り値：失敗した場合の追加のステータス（状態）情報。
* Might hold a `null` の値です。

#### 公開上の決定：決定[]; {#public-decisions}

* 戻り値：事前認証の決定のリスト。 各リソースに対して 1 つの決定がおこなわれます。
* 失敗した場合は、リストが空になる可能性があります。

### クラスのステータス {#class-status}

#### public status: number; {#public-status-numbr}

* RFC 7231 に記載されている HTTP 応答ステータスコード。
* の場合は 0 になります。 `Status` は、Adobe Pass Authentication Services の代わりに SDK から取得されます。

#### public code: number; {#public-code-numbr}

* 標準のAdobe Pass Authentication Services エラーコードです。
* 空の文字列または `null` の値です。

#### public message: string; {#public-msg-string}

* 詳細なメッセージは、場合によっては MVPD 認証エンドポイントまたはプログラマーの劣化ルールによって提供されます。
* 空の文字列または `null` の値です。

#### public details: string; {#public-details-strng}

* 詳細なメッセージを保持します。このメッセージは、場合によっては MVPD 認証エンドポイントまたはプログラマーの劣化ルールによって提供されます。
* 空の文字列または `null` の値です。


#### public helpUrl: string; {#public-help-url-string}

* この状態やエラーが発生した理由と考えられる解決策の詳細にリンクする URL。
* 空の文字列または `null` の値です。

#### public trace: string; {#public-trace-string}

* この応答の一意の識別子。より複雑なシナリオで特定の問題を識別するためにサポートに連絡する際に使用できます。
* 空の文字列または `null` の値です。

#### public action: string; {#public-action-string}

* 状況の修正に推奨されるアクション。
   * **なし**：残念ながら、この問題を修正するための事前定義済みのアクションはありません。 これは、パブリック API の不適切な呼び出しを示している可能性があります
   * **設定**:TVE ダッシュボードを通じて、またはサポートに問い合わせて、設定を変更する必要があります。
   * **出願登録**：アプリケーションは再度登録する必要があります。
   * **認証**：ユーザーは認証するか、再認証する必要があります。
   * **認証**：ユーザーは、特定のリソースの認証を取得する必要があります。
   * **劣化**：何らかの形式の劣化を適用する必要があります。
   * **再試行**：リクエストを再試行すると、問題が解決する場合があります
   * **retry-after**：指定された時間が経過した後にリクエストを再試行すると、問題が解決する場合があります。
* 空の文字列または `null` の値です。

### クラスの決定 {#class-decision}

#### public id: string; {#public-id-string}

* 決定が取得されたリソース ID。

#### public authorized: boolean; {#public-auth-boolean}

* 決定が成功したかどうかを示すフラグの値。

#### public error: Status; {#public-error-status}

* エラーが発生した場合の追加のステータス（状態）情報。 Might hold a `null` の値です。

## クライアント実装の例 {#client-imp-example}

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

### シナリオ 1：要求されたすべてのリソースが承認されました {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>無効</td>
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


### シナリオ 2：リクエストされたリソースの一部が承認されました。 {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>無効</td>
    <td>

    ``JavaScript
    
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    }
    ]
    }
    
    &quot;&#39;

</td>
  </tr>

<tr>
    <td>有効</td>
    <td>

    ``JavaScript
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD は、指定されたリソースに対する事前認証をリクエストする際に、\&quot;拒否\&quot;の決定を返しました。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    },
    ]
    }
    
    &quot;&#39;

</td>
  </tr>
</tbody>


### シナリオ 3：リクエストされたリソースが認証されませんでした。 {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>無効</td>
    <td>

    ``JavaScript
    
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false
    }
    ]
    }
    
    &quot;&#39;

</td>
  </tr>

<tr>
    <td>有効</td>
    <td>

    ``JavaScript
    
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD は、指定されたリソースに対する事前認証をリクエストする際に、\&quot;拒否\&quot;の決定を返しました。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;MVPD は、指定されたリソースに対する事前認証をリクエストする際に、\&quot;拒否\&quot;の決定を返しました。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;maximum_execution_time_exceeded&quot;,
    &quot;message&quot;:&quot;許可された最大時間内にリクエストが完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    }
    ]
    }
    
    &quot;&#39;

</td>
  </tr>
</tbody>


### シナリオ 4：クライアントリクエストが正しくありません — リソースが指定されていません。 {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    ``JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;内部エラーが原因でリクエストが失敗しました。&quot;,
    &quot;details&quot;: &quot;必須の String[] パラメーター「resource」が存在しません&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisions&quot;: []
    }
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>

### シナリオ 5：無効なクライアントリクエスト — 指定されたリソースが空です。 {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    ``JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;リソースパラメータが見つかりません&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisions&quot;: []
    }
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>

### シナリオ 6：ネットワークエラー。 {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>有効</td>
    <td>

    ``JavaScript
    {
    &quot;decisions&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;関連するパートナーサービスから応答を取得する際に、読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;関連するパートナーサービスから応答を取得する際に、読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;retry&quot;
    }
    }
    ]
    }
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>

### シナリオ 7：有効な AuthN セッションがなく、フローを事前認証が呼び出されました。

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    ``JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;:&quot;このリクエストに関連付けられた認証セッションを取得できませんでした。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    },
    &quot;decisions&quot;: []
    }
    
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>



### シナリオ 8: setRequestor の呼び出しが完了する前に、事前承認フローが呼び出されました。

<table>
<thead>
  <tr>
    <th>エラーコードフラグの強化</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>無効/有効</td>
    <td>

    ``JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;リクエスト元は未設定で、setRequestor API 以外の API を使用するための前提条件です。&quot;,
    &quot;action&quot;: &quot;retry&quot;
    },
    &quot;decisions&quot;: []
    }
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>
