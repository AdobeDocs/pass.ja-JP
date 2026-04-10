---
title: 強化されたエラーコード
description: 強化されたエラーコード
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '2747'
ht-degree: 3%

---

# 強化されたエラーコード {#enhanced-error-codes}

>[!IMPORTANT]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

拡張エラーコードは、以下に統合されたクライアントアプリケーションに追加のエラー情報を提供するAdobe Pass認証機能を表します。

* Adobe Pass Authentication REST API:
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [（レガシー） REST API v1](../../legacy/rest-api-v1/rest-api-overview.md)
* Adobe Pass Authentication SDK Preauthorize API:
   * [（レガシー） JavaScript SDK （Preauthorize API）](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [（レガシー） iOS/tvOS SDK （Preauthorize API）](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [（レガシー） Android SDK （Preauthorize API）](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _（*）事前認証APIは、拡張エラーコードのサポートを提供する唯一のAdobe Pass Authentication SDK APIです。_

>[!IMPORTANT]
>
> Adobe Pass Authentication REST API v2を統合するアプリケーションは、デフォルトで、追加の設定を必要とせずに拡張エラーコードのメリットを享受できます。
>
> <br/>
>
> Adobe Pass Authentication REST API v1またはSDKを統合するアプリケーション Preauthorize APIは、機能が明示的に有効になっている場合にのみ、拡張エラーコードのメリットを受けることができます。
>
> <br/>
>
> この機能を明示的に有効にするには、[Zendesk](https://adobeprimetime.zendesk.com)でチケットを作成し、テクニカルアカウントマネージャー（TAM）にお問い合わせください。

## 表示域 {#enhanced-error-codes-representation}

拡張エラーコードは、統合されたAdobe Pass Authentication APIと使用される「Accept」ヘッダー値（つまり、`application/json`または`application/xml`）に応じて、`JSON`または`XML`形式で表すことができます。

| Adobe Pass Authentication API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v2 | &check; |         |
| REST API v1 | &check; | &check; |
| SDK Preauthorize API | &check; |         |

>[!IMPORTANT]
>
> Adobe Pass Authenticationは、次の2つの形式で拡張エラーコードをクライアントアプリケーションに伝えることができます。
>
> <br/>
>
> * **トップレベルのエラー情報**：この場合、***&quot;error&quot;*** オブジェクトはトップレベルに配置されているため、応答本文には&#x200B;***&quot;error&quot;*** オブジェクトのみを含めることができます。
> * **アイテムレベルのエラー情報**：この場合、***&quot;error&quot;*** オブジェクトはアイテムレベルにあるため、応答本文には、サービス中にエラーが発生したすべてのアイテムに対する&#x200B;***&quot;error&quot;*** オブジェクトを含めることができます。
>
> <br/>
>
> 統合されたAdobe Pass Authentication APIの公開ドキュメントを確認して、拡張エラーコードの表現詳細を確認します。

**REST API v2**

REST API v2に適用できる`JSON`として表される拡張エラーコードの例を含む次のHTTP応答を参照してください。

>[!BEGINTABS]

>[!TAB REST API v2 - アイテムレベルのエラー情報（JSON） ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 - トップレベルのエラー情報（JSON） ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**REST API v1**

REST API v1に適用できる`JSON`または`XML`として表される拡張エラーコードの例を含む次のHTTP応答を参照してください。

>[!BEGINTABS]

>[!TAB REST API v1 - アイテムレベルのエラー情報（JSON） ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v1 - トップレベルのエラー情報（JSON） ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 - トップレベルのエラー情報（XML） ]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### 構造 {#enhanced-error-codes-representation-structure}

拡張エラーコードには、次の`JSON` フィールドまたは`XML`属性が含まれます。例：

| 名前 | タイプ | 例 | 制限付き | 説明 |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *アクション* | *文字列* | *none* | &check; | Adobe Pass認証では、このドキュメントで定義されているように、状況を修正する可能性のあるアクションを推奨しました。<br/><br/> 詳しくは、「[&#x200B; アクション &#x200B;](#enhanced-error-codes-action)」の節を参照してください。 |
| *ステータス* | *整数* | *403* | &check; | [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6)文書で定義されているHTTP応答ステータスコード。<br/><br/> 詳細については、「[&#x200B; ステータス &#x200B;](#enhanced-error-codes-status)」の節を参照してください。 |
| *コード* | *文字列* | *authorization_denied_by_mvpd* | &check; | このドキュメントで定義されているように、エラーに関連付けられたAdobe Pass認証の一意のID コード。<br/><br/> 詳しくは、「[Code](#enhanced-error-codes-code)」の節を参照してください。 |
| *メッセージ* | *文字列* | *指定されたリソースの認証をリクエストする際に、MVPDから「拒否」の判断が返されました* |            | エンドユーザーに表示できる、人間が判読可能なメッセージです。<br/><br/> 詳しくは、[応答処理](#enhanced-error-codes-response-handling) セクションを参照してください。 |
| *詳細* | *文字列* | *サブスクリプションパッケージに「ライブ」チャネルが含まれていません* |            | 場合によっては、サービス パートナーから提供される可能性のある詳細なメッセージ、<br/><br/> サービス パートナーがカスタム メッセージを提供しない場合、このフィールドは存在しない可能性があります。 |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja* |            | このエラーが発生した理由と考えられる解決策の詳細にリンクするAdobe Pass認証の公開ドキュメント URL。<br/><br/> このフィールドには絶対URLが含まれており、エラーコードから推測されるべきではありません。エラーコンテキストに応じて、異なるURLを指定できます。 |
| *トレース* | *文字列* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | Adobe Pass認証サポートに連絡して特定の問題のトラブルシューティングを行う際に使用できる応答の一意のID。 |

>[!IMPORTANT]
>
> **制限付き**&#x200B;列は、各フィールドが有限セットの値を保持しているかどうかを示し、無制限フィールドには任意のデータを含めることができます。
>
> <br/>
>
> このドキュメントの今後の更新では、限定的なセットに値を追加できますが、既存のセットを削除したり変更したりすることはありません。

### アクション {#enhanced-error-codes-representation-action}

強化されたエラーコードには、状況を修復する可能性のある推奨アクションを提供する「アクション」フィールドが含まれています。

「アクション」フィールドに指定できる値は次のとおりです。

| アクション | 説明 | カテゴリ |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| なし | この問題を解決するアクションは事前に定義されていませんが、場合によっては、APIの呼び出しが正しくないことを示している可能性があります。 | リクエストコンテキストを修正します。 |
| 設定 | クライアントアプリケーションでは、ほとんどの時間はAdobe Pass TVE ダッシュボードを通じて行われます。 | 統合設定コンテキストを修正します。 |
| application-registration | クライアントアプリケーションは、再度登録する必要があります。 | クライアントアプリケーションのコンテキストを修正します。 |
| 認証 | クライアントアプリケーションは、ユーザーを認証または再認証する必要があります。 | クライアントアプリケーションのコンテキストを修正します。 |
| 認証 | クライアントアプリケーションは、指定されたリソースの認証を取得する必要があります。 | クライアントアプリケーションのコンテキストを修正します。 |
| 再試行 | クライアントアプリケーションは、リクエストを再試行する必要があります。 | リクエストコンテキストを修正します。 |

_（*）一部のエラーでは、複数のアクションが解決策になる可能性がありますが、「アクション」フィールドは、エラーを修正する可能性が最も高いアクションを示します。_

### ステータス {#enhanced-error-codes-representation-status}

拡張エラーコードには、エラーに関連付けられたHTTP ステータスコードを示す「ステータス」フィールドが含まれます。

「ステータス」フィールドに指定できる値は次のとおりです。

| コード | Reason-Phrase |
|------|-----------------------|
| 400 | 不正なリクエスト |
| 401 | 未承認 |
| 403 | 禁止 |
| 404 | 見つかりません |
| 405 | メソッドは許可されていません |
| 410 | ゴーン |
| 412 | 前提条件が失敗しました |
| 500 | 内部サーバーエラー |

通常、4xxの「ステータス」を持つ拡張エラーコードは、エラーがクライアントによって生成され、ほとんどの場合、クライアントがそれを修正するために追加の作業を必要とすることを意味する場合に表示されます。

通常、5xxの「ステータス」を持つ拡張エラーコードは、エラーがサーバーによって生成され、ほとんどの場合、サーバーがそれを修正するために追加の作業を必要とすることを意味する場合に表示されます。

>[!IMPORTANT]
>
> HTTP レスポンスのステータスコードが拡張エラーコードの「ステータス」フィールドと異なる場合があります。特に、拡張エラーコードをアイテムレベルのエラー情報として伝えるAdobe Pass認証APIと対話する場合に問題が発生します。

### コード {#enhanced-error-codes-representation-code}

拡張エラーコードには、エラーに関連付けられたAdobe Pass認証の一意のIDを提供する「コード」フィールドが含まれています。

「コード」フィールドの可能な値は、統合されたAdobe Pass Authentication APIに基づいて、2つのリストの[以下](#enhanced-error-codes-list)に集約されます。

## Lists {#enhanced-error-codes-lists}

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

次の表に、クライアントアプリケーションがAdobe Pass Authentication REST API v2と統合されたときに発生する可能性のある拡張エラーコードを示します。

| アクション | コード | ステータス | メッセージ |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **none** | *invalid_parameter_service_provider* | 400 | サービス プロバイダーのパラメーター値が見つからないか、無効です。 |
|                              | *invalid_parameter_mvpd* | 400 | mvpd パラメーター値が見つからないか、無効です。 |
|                              | *invalid_parameter_code* | 400 | コードパラメーター値が見つからないか、無効です。 |
|                              | *invalid_parameter_resources* | 400 | リソース パラメーター値が見つからないか、無効です。 |
|                              | *invalid_parameter_redirect_url* | 400 | リダイレクト URL パラメーターの値が見つからないか、無効です。 |
|                              | *invalid_parameter_partner* | 400 | パートナーパラメーター値が見つからないか、無効です。 |
|                              | *invalid_parameter_saml_response* | 400 | SAML応答パラメーター値が見つからないか、無効です。 |
|                              | *invalid_header_device_info* | 400 | デバイス情報ヘッダー値が見つからないか、無効です。 |
|                              | *invalid_header_device_identifier* | 400 | デバイス ID ヘッダー値が見つからないか、無効です。 |
|                              | *invalid_header_identity_for_temporary_access* | 400 | 一時アクセス ヘッダー値のIDが見つからないか、無効です。 |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | パートナーフレームワークのステータスヘッダーの権限アクセスのステータス値が存在しません。 |
|                              | *invalid_header_pfs_permission_access_not_determined* | 400 | パートナーフレームワークのステータスヘッダーからの権限アクセスのステータス値は未定です。 |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | パートナーフレームワークのステータスヘッダーからの権限アクセスのステータス値は付与されません。 |
|                              | *invalid_header_pfs_provider_id_not_determined* | 400 | パートナーフレームワークのステータスヘッダーのプロバイダーID値が、既知のmvpdに関連付けられていません。 |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | パートナーフレームワークのステータスヘッダーのプロバイダーID値が、パラメーターとして送信されたmvpdと一致しません。 |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | パートナーフレームワークのステータスヘッダーのプロバイダー情報が期限切れになっています。 |
|                              | *invalid_integration* | 400 | 指定されたサービスプロバイダーとmvpdの統合が存在しないか、無効になっています。 |
|                              | *invalid_authentication_session* | 400 | この要求に関連付けられている認証セッションが見つからないか、無効です。 |
|                              | *preauthorization_denied_by_mvpd* | 403 | 指定されたリソースの事前認証をリクエストする際に、MVPDが「拒否」の判断を返しました。 |
|                              | *authorization_denied_by_mvpd* | 403 | 指定されたリソースの認証をリクエストする際に、MVPDが「拒否」の判断を返しました。 |
|                              | *authorization_denied_by_parental_controls* | 403 | 指定されたリソースのペアレンタルコントロール設定が原因で、MVPDが「拒否」の決定を返しました。 |
|                              | *authorization_denied_by_degradation_rule* | 403 | 指定されたサービスプロバイダーとmvpdの統合には、要求されたリソースの認証を拒否する劣化規則が適用されています。 |
|                              | *internal_server_error* | 500 | 内部サーバーエラーが原因でリクエストが失敗しました。 |
| **設定** | *too_many_resources* | 403 | クエリされたリソースが多すぎるため、認証または事前認証の要求が失敗しました。 承認と事前承認の制限を適切に設定するには、サポートチームにお問い合わせください。 |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | ユーザーメタデータ証明書設定が見つからないか、無効です。 |
|                              | *invalid_configuration_temporary_access* | 500 | 一時アクセス設定が無効です。 |
|                              | *invalid_configuration_platform* | 500 | プラットフォーム設定が見つからないか、統合に無効です。 |
|                              | *invalid_configuration_platform_id* | 500 | プラットフォーム ID設定が見つからないか、無効です。 |
|                              | *invalid_configuration_platform_trait* | 500 | プラットフォーム特性の設定が見つからないか、無効です。 |
|                              | *invalid_configuration_platform_category_trait* | 500 | プラットフォームカテゴリ特性設定が見つからないか、無効です。 |
|                              | *invalid_configuration_platform_services* | 500 | Platform サービス設定が見つからないか、統合に無効です。 |
|                              | *invalid_configuration_mvpd_platform* | 500 | mvpd プラットフォーム設定が見つからないか、mvpdおよびプラットフォームに対して無効です。 |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | mvpd プラットフォームのボーディングステータス設定が見つからないか、mvpdおよびプラットフォームでは無効です。 |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | mvpd プラットフォームプロファイル交換設定が見つからないか、mvpdおよびプラットフォームに対して無効です。 |
| **application-registration** | *invalid_access_token_service_provider* | 401 | サービスプロバイダーが無効なため、アクセストークンが無効です。 |
|                              | *invalid_access_token_client_application* | 401 | 無効なクライアントアプリケーションが原因で、アクセストークンが無効です。 |
| **認証** | *authenticated_profile_missing* | 403 | このリクエストに関連付けられた認証済みプロファイルがありません。 |
|                              | *authenticated_profile_expired* | 403 | このリクエストに関連付けられた認証済みプロファイルの有効期限が切れています。 |
|                              | *authenticated_profile_invalidated* | 403 | このリクエストに関連付けられた認証済みプロファイルは無効化されました。 |
|                              | *temporary_access_duration_limit_exceeded* | 403 | 一時的なアクセス時間の制限を超えました。 |
|                              | *temporary_access_resources_limit_exceeded* | 403 | 一時的なアクセス リソースの制限を超えました。 |
|                              | *authorization_denied_by_hba_policies* | 403 | MVPDは、ホームベースの認証ポリシーにより、「拒否」の判断を返しました。 現在の認証はホームベースの認証フローを通じて取得されましたが、指定されたリソースの認証をリクエストする際に、デバイスがホームインではなくなりました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                              | *authorization_denied_by_session_invalidated* | 403 | 認証セッションはID プロバイダーによって無効化されました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                              | *identity_not_recognized_by_mvpd* | 403 | MVPDでユーザーIDが認識されなかったため、認証リクエストが失敗しました。 |
| **再試行** | *network_received_error* | 403 | 関連付けられたパートナーサービスから応答を取得中に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                              | *network_connection_timeout* | 403 | 関連付けられたパートナーサービスとの接続タイムアウトがありました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                              | *maximum_execution_time_exceeded* | 403 | リクエストは許可された最大時間に完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。 |

### （レガシー） REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

次の表に、クライアントアプリケーションがAdobe Pass Authentication REST API v1と統合されたときに発生する可能性のある拡張エラーコードを示します。

| アクション | コード | ステータス | メッセージ |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **none** | *invalid_requestor* | 400 | リクエストパラメーターがないか、無効です。 |
|                    | *invalid_device_info* | 400 | デバイス情報が見つからないか、無効です。 |
|                    | *invalid_device_id* | 400 | デバイス IDが見つからないか、無効です。 |
|                    | *missing_resource* | 400, 412 | リソースパラメーターがありません。 |
|                    | *malformed_authz_request* | 400, 412 | 承認要求がnullまたは無効です。 |
|                    | *preauthorization_denied_by_mvpd* | 403 | 指定されたリソースの事前認証をリクエストする際に、MVPDが「拒否」の判断を返しました。 |
|                    | *authorization_denied_by_mvpd* | 403 | 指定されたリソースの認証をリクエストする際に、MVPDが「拒否」の判断を返しました。 |
|                    | *authorization_denied_by_parental_controls* | 403 | 指定されたリソースのペアレンタルコントロール設定が原因で、MVPDが「拒否」の決定を返しました。 |
|                    | *internal_error* | 400, 405, 500 | 内部サーバーエラーが原因でリクエストが失敗しました。 |
| **設定** | *unknown_integration* | 400, 412 | 指定されたプログラマーとID プロバイダーの統合が存在しません。 TVE ダッシュボードを使用して、必要な統合を作成します。 |
|                    | *too_many_resources* | 403 | クエリされたリソースが多すぎるため、認証または事前認証の要求が失敗しました。 承認と事前承認の制限を適切に設定するには、サポートチームにお問い合わせください。 |
| **認証** | *authentication_session_issuer_mismatch* | 400 | 認証フローの示されたMVPDが認証セッションを発行したユーザーと異なるため、認証リクエストは失敗しました。 続行するには、目的のMVPDで再認証する必要があります。 |
|                    | *authorization_denied_by_hba_policies* | 403 | MVPDは、ホームベースの認証ポリシーにより、「拒否」の判断を返しました。 現在の認証はホームベースの認証フロー（HBA）を使用して取得されましたが、指定されたリソースの認証をリクエストする際に、デバイスが自宅にいることはなくなりました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *authorization_denied_by_session_invalidated* | 403 | 認証セッションはID プロバイダーによって無効化されました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *identity_not_recognized_by_mvpd* | 403 | MVPDでユーザーIDが認識されなかったため、認証リクエストが失敗しました。 |
|                    | *authentication_session_invalidated* | 403 | 認証セッションはID プロバイダーによって無効化されました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *authentication_session_missing* | 403, 412 | この要求に関連付けられている認証セッションを取得できませんでした。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *authentication_session_expired* | 403, 412 | 現在の認証セッションの有効期限が切れています。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *preauthorization_authentication_session_missing* | 412 | この要求に関連付けられている認証セッションを取得できませんでした。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *preauthorization_authentication_session_expired* | 412 | 現在の認証セッションの有効期限が切れています。 続行するには、サポートされているMVPDで再認証する必要があります。 |
| **認証** | *authorization_not_found* | 403, 404 | 指定されたリソースの認証が見つかりませんでした。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
|                    | *authorization_expired* | 410 | 指定されたリソースの以前の認証は期限切れです。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
| **再試行** | *network_received_error* | 403 | 関連付けられたパートナーサービスから応答を取得中に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                    | *network_connection_timeout* | 403 | 関連付けられたパートナーサービスとの接続タイムアウトがありました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                    | *maximum_execution_time_exceeded* | 403 | リクエストは許可された最大時間に完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。 |

### （レガシー） SDK Preauthorize API {#enhanced-error-codes-lists-sdks-preauthorize-api}

クライアントアプリケーションがAdobe Pass Authentication SDK Preauthorize APIと統合されたときに発生する可能性のある拡張エラーコードについては、前の[&#x200B; セクション &#x200B;](#enhanced-error-codes-list-rest-api-v1)を参照してください。

## 応答処理 {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> ネットワークタイムアウトの場合に認証リクエストを再試行したり、セッションの有効期限が切れた場合にユーザーに再認証を求めるなど、クライアントアプリケーションコードで自動的に処理できる拡張エラーコードがありますが、他のタイプでは設定の変更やAdobe Pass Authentication カスタマーケアチームの操作が必要になる場合があります。
>
> <br/>
>
> したがって、新しいアプリケーションまたは新機能を起動する前に必要な変更を行うために、チケットを作成する際に[Zendesk](https://adobeprimetime.zendesk.com)を通じて完全なエラー情報を収集して提供することが重要です。

要約すると、拡張エラーコードを含む応答を処理する場合は、次の点を考慮する必要があります。

1. エラーを返す&#x200B;**APIに依存しない**：拡張エラーコードの完全なカタログをサポートする一元化されたエラー処理ロジックを、APIが生成するかどうかに関係なく実装します。 いくつかの拡張エラーコードはAPI間で共有され、一貫して処理する必要があります。

1. **トップレベルとアイテムレベルのエラー情報に依存しない**：トップレベルとアイテムレベルのエラー情報を通信方法に依存しない方法で処理し、両方の形式の拡張エラーコードを送信できることを確認します。

1. **両方のステータス値を確認する**: HTTP応答のステータスコードと拡張エラーコードの「ステータス」フィールドの両方を常に確認します。 両者には違いがあり、どちらも貴重な情報です。

1. **再試行ロジック**：再試行が必要なエラーの場合、再試行が制限されているか（2-3など）、サーバーに負荷がかかるのを避けるために指数関数的なバックオフで行われていることを確認します。 また、複数の項目を一度に処理するAdobe Pass認証API （Preauthorize APIなど）の場合は、繰り返しリクエストに「再試行」とマークされた項目のみを含め、リスト全体を含める必要はありません。

1. **構成の変更**：構成の変更が必要なエラーについては、新しいアプリケーションまたは新機能を起動する前に、必要な変更を行ってください。

1. **認証と認証**：認証と認証に関連するエラーの場合、必要に応じてユーザーに再認証を促すか、新しい認証を取得する必要があります。

1. **ユーザーからのフィードバック**：オプションで、人間が読み取れる「メッセージ」フィールドと（潜在的な）「詳細」フィールドを使用して、問題についてユーザーに通知します。 「詳細」テキストメッセージは、MVPDの事前認証エンドポイントまたは認証エンドポイントから、または劣化ルールを適用する際にプログラマーから渡される可能性があります。
