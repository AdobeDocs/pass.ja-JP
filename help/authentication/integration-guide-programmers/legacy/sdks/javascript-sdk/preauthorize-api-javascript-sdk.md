---
title: 事前承認
description: JavaScriptの事前認証
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 7208b16831e1c6c4cbb37bf925a798d931ab8ea3
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 0%

---

# （レガシー）事前認証 {#js-preauthorize}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#preauth-overview}

Preauthorize API メソッドは、アプリケーションが1つ以上のリソースの事前承認決定を取得するために使用します。 Preauthorize API リクエストは、UI ヒントやコンテンツフィルタリングに使用する必要があります。 指定されたリソースへのユーザーアクセスを許可する前に、実際の認証API リクエストを行う必要があります。

Preauthorize API リクエストがAdobe Pass Authentication サービスによって処理される際に予期しないエラー（ネットワークの問題、MVPD認証エンドポイントが使用できない場合など）が発生した場合、Preauthorize APIの応答結果の一部として、影響を受けるリソースに1つまたは複数の個別のエラー情報が含まれます。

### public preauthorize （request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any>）: void {#preauth-method}

**説明：**&#x200B;このメソッドは、アプリケーションのUIをデコレーションする目的（ロックとロック解除のアイコンでアクセス状態を示すなど）で、特定の保護されたリソースを表示するために、Adobe Pass認証サービスから認証されたユーザーの事前認証（情報）の決定を取得するためにアプリケーションで使用されます。

**可用性：** v4.4.0以降

**パラメーター：**

* `PreauthorizeRequest`: リクエストの定義に使用されるビルダーオブジェクト
* `AccessEnablerCallback`: API応答を返すために使用されるコールバック
* `PreauthorizeResponse`: API応答コンテンツを返すために使用されるオブジェクト

### class PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources （resources: string[]）: PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* 事前認証の決定を取得するリソースのリストを設定します。
* preauthorize APIの使用に設定する必要があります。
* リスト内の各要素は、リソース ID値またはMVPDと合意する必要があるMedia RSS フラグメントを表す文字列である必要があります。
* このメソッドは、このメソッド呼び出しの受信者である現在の`PreauthorizeRequestBuilder` オブジェクト インスタンスのコンテキストでのみ情報を設定します。

* 実際の`PreauthorizeRequest`を作成するには、`PreauthorizeRequestBuilder`のメソッドを参照してください。

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` リソース。 事前認証の決定を取得するリソースのリスト。
* `@returns {PreauthorizeRequestBuilder}` メソッド呼び出しの受信者である同じ`PreauthorizeRequestBuilder` オブジェクト インスタンスへの参照。
* これは、メソッドのチェーンの作成を許可するためです。

#### disableFeatures （。..features:string[]）: PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* 事前認証の決定を取得する際に無効にする機能を設定します。
* この関数は、この関数呼び出しの受信者である現在の`PreauthorizeRequestBuilder` オブジェクト インスタンスのコンテキストでのみ情報を設定します。
* 実際の`PreauthorizeRequest`を作成するには、`PreauthorizeRequestBuilder`の関数を確認します。

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}`機能。 無効にする機能セット。
* `@returns`関数呼び出しの受信者である同じ`PreauthorizeRequestBuilder` オブジェクトインスタンスへの参照。
* これは、関数チェーンの作成を許可するために実行します。

#### build （）: PreauthorizeRequest {#preauth-req}

* 新しい`PreauthorizeRequest` オブジェクト インスタンスの参照を作成して取得します。
* このメソッドは、呼び出されるたびに新しい`PreauthorizeRequest` オブジェクトをインスタンス化します。
* このメソッドは、このメソッド呼び出しの受信者である現在の`PreauthorizeRequestBuilder` オブジェクトインスタンスのコンテキストで事前に設定された値を使用します。
* この方法は副作用を引き起こさないことを覚えておいてください，
* したがって、このメソッド呼び出しの受信者であるSDKのステートや`PreauthorizeRequestBuilder` オブジェクトインスタンスのステートは変更されません。
* つまり、同じ受信者に対するこのメソッドの連続した呼び出しは、異なる新しい`PreauthorizeRequest` オブジェクトインスタンスを作成しますが、呼び出し間で変更されない`PreauthorizeRequestBuilder`に設定された値の場合、同じ情報を持ちます。
* 提供された情報（リソースとキャッシュ）のいずれかを更新する必要がない場合は、Preauthorize APIを複数回使用するためにPreauthorizeRequest インスタンスを再利用できます。
* `@returns {PreauthorizeRequest}`

### インターフェイス AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse （result: T）; {#on-response-result}

* 事前認証API リクエストが満たされたときにSDKによって呼び出された応答コールバック。
* 結果は、成功またはステータスを含むエラーの結果です。
* `@param {T} result`

#### onFailure （result: T）; {#on-failure-result}

* 事前認証API リクエストを処理できなかった場合に、SDKによって呼び出される失敗コールバック。
* 結果は、ステータスを含むエラー結果です。
* `@param {T} result`

### class PreauthorizeResponse {#preauth-response-class}

#### public status: Status; {#public-status}

* 返品：失敗した場合の追加ステータス（状態）情報。
* `null`値を保持する可能性があります。

#### 公開決定：決定[]; {#public-decisions}

* 戻り値：事前認証の決定のリスト。 リソースごとに決定。
* エラーが発生した場合、リストが空になる可能性があります。

### クラスステータス {#class-status}

#### 公開状態：番号； {#public-status-numbr}

* RFC 7231に記載されているHTTP応答ステータスコード。
* `Status`がAdobe Pass認証サービスではなくSDKから取得される場合は、0である可能性があります。

#### 公開コード：number; {#public-code-numbr}

* 標準のAdobe Pass Authentication Services エラーコード。
* 空の文字列または`null`値を保持している可能性があります。

#### パブリックメッセージ：文字列； {#public-msg-string}

* 場合によっては、MVPD認証エンドポイントまたはプログラマーのデグラデーション規則によって提供される詳細なメッセージ。
* 空の文字列または`null`値を保持している可能性があります。

#### 公開情報：文字列； {#public-details-strng}

* MVPD認証エンドポイントまたはプログラマのデグラデーション規則によって提供される場合がある詳細なメッセージを保持します。
* 空の文字列または`null`値を保持している可能性があります。


#### public helpUrl：文字列； {#public-help-url-string}

* この状態/エラーが発生した理由と考えられる解決策に関する詳細情報にリンクするURL。
* 空の文字列または`null`値を保持している可能性があります。

#### public trace：文字列； {#public-trace-string}

* この応答の一意の識別子。より複雑なシナリオで特定の問題を特定するためにサポートに連絡する際に使用できます。
* 空の文字列または`null`値を保持している可能性があります。

#### パブリックアクション：文字列； {#public-action-string}

* 状況を修復するための推奨アクション。
  * **none**：残念ながら、この問題を解決するための事前定義済みのアクションがありません。 これは、パブリック APIの不適切な呼び出しを示している可能性があります
  * **設定**：設定の変更は、TVE ダッシュボードを通じて、またはサポートに連絡して必要です。
  * **application-registration**: アプリケーションは再度登録する必要があります。
  * **認証**: ユーザーは認証または再認証を行う必要があります。
  * **認証**: ユーザーは特定のリソースの認証を取得する必要があります。
  * **劣化**：何らかの劣化を適用する必要があります。
  * **再試行**：リクエストを再試行すると、問題が解決する可能性があります
  * **retry-after**：指定された時間経過後にリクエストを再試行すると、問題が解決する可能性があります。
* 空の文字列または`null`値を保持している可能性があります。

### クラス決定 {#class-decision}

#### public id：文字列； {#public-id-string}

* 決定を取得したリソース ID。

#### public authorized: ブール値； {#public-auth-boolean}

* 決定が成功したかどうかを示すフラグの値。

#### 公開エラー：ステータス； {#public-error-status}

* 何らかのエラーが発生した場合の追加ステータス（状態）情報。 `null`値を保持する可能性があります。

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
    <th>強化されたエラーコードフラグ</th>
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


### シナリオ 2：一部のリクエストされたリソースが許可されました。 {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
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
        "authorized": false
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

<tr>
    <td>有効</td>
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
        "authorized": false,
        "error": {
            "status": 403,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "none"
        }
        },
        {
        "id": "RES03",
        "authorized": true
        },
    ]
    }
    
```

</td>
  </tr>
</tbody>


### シナリオ 3：要求されたリソースのいずれも許可されていません。 {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
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
        "authorized": false
        },
        {
        "id": "RES02",
        "authorized": false
        },
        {
        "id": "RES03",
        "authorized": false
        }
    ]
    }
       
```

</td>
  </tr>

<tr>
    <td>有効</td>
    <td>

```JavaScript
    {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "none"
            }
        },
        {
            "id": "RES02",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "preauthorization_denied_by_mvpd",
                "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
                "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
                "action": "none"
            }
        },
        {
        "id": "RES03",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "maximum_execution_time_exceeded",
            "message": "The request did not complete in the maximum allowed time. Retrying the request might solve the issue.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "retry"
                }
            }
        ]
    }
    
```

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

```JavaScript
    {
    "status": {
    "status": 400,
    "code": "internal_error",
    "message": "The request failed due to an internal error.",
    "details": "Required String[] parameter 'resource' is not present",
    "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
    "action": "none"
    },
    "decisions": []
    }
```

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

```JavaScript
    {
    "status": {
    "status": 412,
    "code": "missing_resource",
    "message": "The resource parameter is missing",
    "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
    "action": "none"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>

### シナリオ 6: ネットワーク エラー。 {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>強化されたエラーコードフラグ</th>
    <th>応答</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>有効</td>
    <td>

```JavaScript
    {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "network_received_error",
            "message": "There was a read error while retrieving the response from the associated partner service. Retrying the request might solve the issue.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "retry"
            }
        },
        {
            "id": "RES02",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "network_received_error",
                "message": "There was a read error while retrieving the response from the associated partner service. Retrying the request might solve the issue.",
                "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
                "action": "retry"
                }   
        }
    ]
    }
```

</td>
  </tr>
</tbody>
</table>

### シナリオ 7：有効なAuthN セッションなしで事前承認フローが呼び出されました。

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

```JavaScript
    {
    "status": {
    "status": 0,
    "code": "authentication_session_missing",
    "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
    "action": "authentication"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>



### シナリオ 8: setRequestor呼び出しが完了する前に事前承認フローが呼び出されました

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

```JavaScript
    {
    "status": {
    "status": 0,
    "code": "requestor_not_configured",
    "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
    "action": "retry"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>
