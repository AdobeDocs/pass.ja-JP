---
title: 拡張エラーコード
description: 拡張エラーコード
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 7ac04991289c95ebb803d1fd804e9b497f821cda
workflow-type: tm+mt
source-wordcount: '2696'
ht-degree: 3%

---

# 拡張エラーコード {#enhanced-error-codes}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

拡張エラーコードは、Adobe Passの認証機能を表し、と統合されたクライアントアプリケーションに追加のエラー情報を提供します。

* Adobe Pass認証 REST API:
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [（レガシー） REST API v1](../../legacy/rest-api-v1/rest-api-overview.md)
* Adobe Pass認証 SDK による API の事前認証：
   * [（従来の）JavaScript SDK（事前認証 API）](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [（従来の）iOS/tvOS SDK（事前認証 API）](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [（従来の）Android SDK（事前認証 API）](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _（*）事前認証 API は、拡張エラーコードをサポートする唯一のAdobe Pass認証SDK API です。_

>[!IMPORTANT]
>
> Adobe Pass認証 REST API v2 を統合するアプリケーションでは、追加の設定を行わなくてもデフォルトで拡張エラーコードのメリットが得られます。
>
> <br/>
>
> Adobe Pass認証 REST API v1 または SDK 事前認証 API を統合するアプリケーションでは、この機能が明示的に有効になっている場合にのみ、拡張エラーコードのメリットを受けることができます。
>
> <br/>
>
> この機能を明示的に有効にするには、アドビの [Zendesk](https://adobeprimetime.zendesk.com) からチケットを作成し、テクニカルアカウントマネージャー（TAM）にお問い合わせください。

## 表示域 {#enhanced-error-codes-representation}

拡張エラーコードは、統合Adobe Pass認証 API と使用されている「Accept」ヘッダー値（`JSON` または `XML`）に応じて、`application/json` 形式または `application/xml` 形式で表すことができます。

| Adobe Pass認証 API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v2 | &amp;check; |         |
| REST API v1 | &amp;check; | &amp;check; |
| SDK 事前認証 API | &amp;check; |         |

>[!IMPORTANT]
>
> Adobe Pass認証は、次の 2 つの形式で、拡張エラーコードをクライアントアプリケーションに通信できます。
>
> <br/>
>
> * **最上位レベルのエラー情報**：この場合、***&quot;error&quot;*** オブジェクトは最上位レベルにあるので、応答本文には ***&quot;error&quot;*** オブジェクトのみを含めることができます。
> * **項目レベルのエラー情報**：この場合、***&quot;error&quot;*** オブジェクトは項目レベルにあるので、応答本文には、サービス中にエラーが発生したすべての項目の ***&quot;error&quot;*** オブジェクトを含めることができます。
>
> <br/>
>
> 統合された各Adobe Pass認証 API の公開ドキュメントを確認して、拡張エラーコード表現の詳細を判断します。

**REST API v2**

REST API v2 に適用できる例として示されている拡張エラーコードを含む次の HTTP 応答 `JSON` 参照してください。

>[!BEGINTABS]

>[!TAB REST API v2 – 項目レベルのエラー情報（JSON） ]

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
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 – 最上位のエラー情報（JSON） ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**REST API v1**

REST API v1 に適用できる `JSON` または `XML` として表される、拡張エラーコードを含む次の HTTP 応答の例を参照してください。

>[!BEGINTABS]

>[!TAB REST API v1 – 項目レベルのエラー情報（JSON） ]

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
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v1 – 最上位のエラー情報（JSON） ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 – 最上位レベルのエラー情報（XML） ]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### 構造 {#enhanced-error-codes-representation-structure}

拡張エラーコードには、例を使用した次の `JSON` フィールドまたは `XML` 属性が含まれます。

| 名前 | タイプ | 例 | 制限付き | 説明 |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *アクション* | *文字列* | *なし* | &amp;check; | Adobe Pass認証は、このドキュメントで定義されている状況を修正する可能性のある、推奨されるアクションを行います。 <br/><br/> 詳しくは、[ アクション ](#enhanced-error-codes-action) の節を参照してください。 |
| *ステータス* | *整数* | *403* | &amp;check; | [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6) ドキュメントで定義されている HTTP 応答ステータスコード。 <br/><br/> 詳しくは、[ ステータス ](#enhanced-error-codes-status) の節を参照してください。 |
| *code* | *文字列* | *authorization_denied_by_mvpd* | &amp;check; | このドキュメントで定義されているように、エラーに関連付けられたAdobe Pass認証の一意の ID コード。 <br/><br/> 詳しくは、[ コード ](#enhanced-error-codes-code) の節を参照してください。 |
| *message* | *文字列* | *指定されたリソースの認証をリクエストしたときに、MVPDから「拒否」の決定が返されました* |            | 場合によってはエンドユーザーに表示される可能性がある、人間が判読できるメッセージ。 <br/><br/> 詳しくは、[ 応答の処理 ](#enhanced-error-codes-response-handling) の節を参照してください。 |
| *詳細* | *文字列* | *サブスクリプションパッケージに「ライブ」チャネルが含まれていません* |            | サービスパートナーが提供できる場合がある詳細なメッセージ <br/><br/> サービスパートナーがカスタムメッセージを提供しない場合、このフィールドは存在しない可能性があります。 |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | このエラーが発生した理由と考えられる解決策の詳細に関するリンクのAdobe Pass認証の公開ドキュメント URL。 <br/><br/> このフィールドには絶対 URL が格納されます。エラーコードから推論しないでください。エラーコンテキストに応じて、別の URL を指定できます。 |
| *トレース* | *文字列* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | 特定の問題のトラブルシューティングのためにAdobe Pass Authentication サポートに連絡する際に使用できる、レスポンスの一意の ID。 |

>[!IMPORTANT]
>
> **Restricted** 列は、各フィールドに有限セットの値が保持されているかどうかを示し、無制限フィールドには任意のデータを含めることができます。
>
> <br/>
>
> このドキュメントを今後更新すると、有限セットに値が追加される可能性がありますが、既存の値は削除または変更されません。

### アクション {#enhanced-error-codes-representation-action}

拡張エラーコードには、状況を修正する可能性のある推奨アクションを提供する「アクション」フィールドが含まれています。

「アクション」フィールドには、次の値を使用できます。

| アクション | 説明 | カテゴリ |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| なし | この問題を修正するための事前定義されたアクションはありませんが、場合によっては、API の不適切な呼び出しを示す可能性があります。 | リクエストコンテキストを修正します。 |
| 設定 | クライアントアプリケーションでは、設定を変更する必要があります。ほとんどの場合、設定変更はAdobe Pass TVE ダッシュボードで行われます。 | 統合設定コンテキストを修正します。 |
| application-registration | クライアントアプリケーションは、自分自身を再度登録する必要があります。 | クライアントアプリケーションコンテキストを修正します。 |
| 認証 | クライアントアプリケーションでは、ユーザーの認証または再認証が必要です。 | クライアントアプリケーションコンテキストを修正します。 |
| 認証 | クライアントアプリケーションは、指定されたリソースの認証を取得する必要があります。 | クライアントアプリケーションコンテキストを修正します。 |
| 再試行 | クライアントアプリケーションはリクエストを再試行する必要があります。 | リクエストコンテキストを修正します。 |

_（*）エラーによっては、複数のアクションが解決策になる場合がありますが、「アクション」フィールドは、エラーを修正する可能性が最も高いアクションを示します。_

### ステータス {#enhanced-error-codes-representation-status}

拡張エラーコードには、エラーに関連付けられた HTTP ステータスコードを示す「ステータス」フィールドが含まれます。

「ステータス」フィールドに指定可能な値は次のとおりです。

| コード | 理由フレーズ |
|------|-----------------------|
| 400 | リクエストが正しくありません |
| 401 | 未認証 |
| 403 | 禁止 |
| 404 | 見つかりません |
| 405 | 許可されていないメソッド |
| 410 | 廃止 |
| 412 | 事前条件が失敗しました |
| 500 | 内部サーバーエラー |

「ステータス」が 4xx の拡張エラーコードは、通常、エラーがクライアントによって生成された場合に表示され、ほとんどの場合、クライアントがそれを修正するために追加の作業を必要とすることを意味します。

通常、5xx の「ステータス」を含む拡張エラーコードは、サーバーによってエラーが生成されたときに表示され、ほとんどの場合、サーバーがそれを修正するために追加の作業を必要とすることを意味します。

>[!IMPORTANT]
>
> HTTP レスポンスのステータスコードが、拡張エラーコードの「ステータス」フィールドと異なる場合があります。特に、拡張エラーコードを項目レベルのエラー情報として通信するAdobe Pass認証 API とやり取りする場合に発生します。

### コード {#enhanced-error-codes-representation-code}

拡張エラーコードには、エラーに関連付けられたAdobe Pass認証の一意の ID を提供する「コード」フィールドが含まれています。

「code」フィールドに指定可能な値は、統合Adobe Pass認証 API に基づいて [ 以下 ](#enhanced-error-codes-list)2 つのリストに集約されます。

## リスト {#enhanced-error-codes-lists}

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

次の表に、Adobe Pass Authentication REST API v2 と統合した場合にクライアントアプリケーションで発生する可能性のある拡張エラーコードを示します。

| アクション | コード | ステータス | メッセージ |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **なし** | *invalid_parameter_service_provider* | 400 | サービスプロバイダーパラメーター値がないか、無効です。 |
|                              | *invalid_parameter_mvpd* | 400 | mvpd パラメーター値がないか、無効です。 |
|                              | *invalid_parameter_code* | 400 | コードパラメーター値がないか、無効です。 |
|                              | *invalid_parameter_resources* | 400 | リソースパラメーター値がないか、無効です。 |
|                              | *invalid_parameter_redirect_url* | 400 | リダイレクト URL パラメーター値がないか、無効です。 |
|                              | *invalid_parameter_partner* | 400 | パートナーパラメーター値がないか、無効です。 |
|                              | *invalid_parameter_saml_response* | 400 | SAML 応答パラメーター値がないか、無効です。 |
|                              | *invalid_header_device_info* | 400 | デバイス情報ヘッダー値がないか、無効です。 |
|                              | *invalid_header_device_identifier* | 400 | デバイス識別子ヘッダー値がないか、無効です。 |
|                              | *invalid_header_identity_for_temporary_access* | 400 | 一時アクセスヘッダー値の ID がないか、無効です。 |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | パートナーフレームワーク状態ヘッダーからのアクセス許可状態の値がありません。 |
|                              | *invalid_header_pfs_permission_access_not_determined* | 400 | パートナーフレームワークステータスヘッダーの権限アクセスステータス値が不明です。 |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | パートナーフレームワークステータスヘッダーの権限アクセスステータス値が付与されません。 |
|                              | *invalid_header_pfs_provider_id_not_determined* | 400 | パートナーフレームワーク状態ヘッダーのプロバイダー ID 値は、既知の mvpd に関連付けられていません。 |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | パートナーフレームワーク状態ヘッダーのプロバイダー ID 値が、パラメーターとして送信された mvpd と一致しません。 |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | パートナーフレームワーク状態ヘッダーのプロバイダー情報の有効期限が切れました。 |
|                              | *invalid_integration* | 400 | 指定されたサービス プロバイダーと mvpd の統合が存在しないか、無効です。 |
|                              | *invalid_authentication_session* | 400 | この要求に関連付けられている認証セッションが見つからないか無効です。 |
|                              | *preauthorization_denied_by_mvpd* | 403 | MVPDが、指定されたリソースの事前認証をリクエストする際に「拒否」の決定を返しました。 |
|                              | *authorization_denied_by_mvpd* | 403 | MVPDが、指定されたリソースの認証をリクエストする際に「拒否」の決定を返しました。 |
|                              | *authorization_denied_by_parental_controls* | 403 | MVPDが、指定されたリソースに対するペアレンタルコントロール設定が原因の「拒否」の決定を返しました。 |
|                              | *authorization_denied_by_degradation_rule* | 403 | 指定されたサービス プロバイダと mvpd の統合には、要求されたリソースの承認を拒否する最適化規則が適用されています。 |
|                              | *internal_server_error* | 500 | 内部サーバーエラーが発生したため、リクエストは失敗しました。 |
| **設定** | *too_many_resources* | 403 | クエリされたリソースが多すぎるため、承認または事前承認の要求に失敗しました。 認証および事前認証制限を適切に設定するには、サポートチームにお問い合わせください。 |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | ユーザーメタデータ証明書の構成が見つからないか無効です。 |
|                              | *invalid_configuration_temporary_access* | 500 | 一時アクセス設定が無効です。 |
|                              | *invalid_configuration_platform* | 500 | プラットフォーム設定がないか、統合に対して無効です。 |
|                              | *invalid_configuration_platform_id* | 500 | プラットフォーム ID 設定がないか、無効です。 |
|                              | *invalid_configuration_platform_trait* | 500 | プラットフォーム特性設定がないか、無効です。 |
|                              | *invalid_configuration_platform_category_trait* | 500 | プラットフォームカテゴリ特性の設定がないか、無効です。 |
|                              | *invalid_configuration_platform_services* | 500 | Platform サービス設定がないか、統合に対して無効です。 |
|                              | *invalid_configuration_mvpd_platform* | 500 | mvpd プラットフォーム設定が見つからないか、mvpd およびプラットフォームで無効です。 |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | mvpd プラットフォームのボーディングステータス設定が、mvpd とプラットフォームでは見つからないか無効です。 |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | mvpd プラットフォームプロファイル交換の設定が、mvpd およびプラットフォームに対して見つからないか無効です。 |
| **application-registration** | *invalid_access_token_service_provider* | 401 | サービスプロバイダーが無効なため、アクセストークンが無効です。 |
|                              | *invalid_access_token_client_application* | 401 | クライアントアプリケーションが無効なため、アクセストークンが無効です。 |
| **認証** | *authenticated_profile_missing* | 403 | このリクエストに関連付けられている認証済みプロファイルがありません。 |
|                              | *authenticated_profile_expired* | 403 | このリクエストに関連付けられている認証済みプロファイルの有効期限が切れています。 |
|                              | *authenticated_profile_invalidated* | 403 | このリクエストに関連付けられている認証済みプロファイルが無効化されました。 |
|                              | *temporary_access_duration_limit_exceeded* | 403 | 一時アクセス期間の制限を超えています。 |
|                              | *temporary_access_resources_limit_exceeded* | 403 | 一時アクセスリソースの制限を超えています。 |
|                              | *authorization_denied_by_hba_policies* | 403 | MVPDが、ホームベースの認証ポリシーが原因で、「拒否」の決定を返しました。 現在の認証はホームベースの認証フローを通じて取得されたものですが、指定されたリソースの認証を要求したときにデバイスはホームに存在しなくなりました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                              | *authorization_denied_by_session_invalidated* | 403 | 認証セッションは ID プロバイダーによって無効化されました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                              | *identity_not_recognized_by_mvpd* | 403 | MVPDでユーザー ID が認識されなかったため、認証リクエストが失敗しました。 |
| **再試行** | *network_received_error* | 403 | 関連付けられたパートナーサービスから応答を取得する際に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                              | *network_connection_timeout* | 403 | 関連付けられたパートナーサービスで接続タイムアウトが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                              | *maximum_execution_time_exceeded* | 403 | 最大許容時間内にリクエストが完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。 |

### （レガシー） REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

次の表に、Adobe Pass認証 REST API v1 と統合した場合にクライアントアプリケーションで発生する可能性のある拡張エラーコードを示します。

| アクション | コード | ステータス | メッセージ |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **なし** | *invalid_requestor* | 400 | 要求者パラメーターがないか、無効です。 |
|                    | *invalid_device_info* | 400 | デバイス情報がないか、無効です。 |
|                    | *invalid_device_id* | 400 | デバイス識別子がないか、無効です。 |
|                    | *missing_resource* | 400、412 | リソースパラメーターがありません。 |
|                    | *malformed_authz_request* | 400、412 | 認証リクエストが null または無効です。 |
|                    | *preauthorization_denied_by_mvpd* | 403 | MVPDが、指定されたリソースの事前認証をリクエストする際に「拒否」の決定を返しました。 |
|                    | *authorization_denied_by_mvpd* | 403 | MVPDが、指定されたリソースの認証をリクエストする際に「拒否」の決定を返しました。 |
|                    | *authorization_denied_by_parental_controls* | 403 | MVPDが、指定されたリソースに対するペアレンタルコントロール設定が原因の「拒否」の決定を返しました。 |
|                    | *internal_error* | 400、405、500 | 内部サーバーエラーが発生したため、リクエストは失敗しました。 |
| **設定** | *unknown_integration* | 400、412 | 指定されたプログラマーと ID プロバイダーの統合が存在しません。 TVE ダッシュボードを使用して、必要な統合を作成します。 |
|                    | *too_many_resources* | 403 | クエリされたリソースが多すぎるため、承認または事前承認の要求に失敗しました。 認証および事前認証制限を適切に設定するには、サポートチームにお問い合わせください。 |
| **認証** | *authentication_session_issuer_mismatch* | 400 | 認証フローに示されたMVPDが認証セッションを発行したユーザーと異なるので、認証リクエストは失敗しました。 続行するには、目的のMVPDでユーザーの再認証が必要です。 |
|                    | *authorization_denied_by_hba_policies* | 403 | MVPDが、ホームベースの認証ポリシーが原因で、「拒否」の決定を返しました。 現在の認証はホームベースの認証フロー（HBA）を使用して取得されましたが、指定されたリソースの認証を要求する際にデバイスは自宅に存在しなくなりました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *authorization_denied_by_session_invalidated* | 403 | 認証セッションは ID プロバイダーによって無効化されました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *identity_not_recognized_by_mvpd* | 403 | MVPDでユーザー ID が認識されなかったため、認証リクエストが失敗しました。 |
|                    | *authentication_session_invalidated* | 403 | 認証セッションは ID プロバイダーによって無効化されました。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *authentication_session_missing* | 403、412 | この要求に関連付けられている認証セッションを取得できませんでした。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *authentication_session_expired* | 403、412 | 現在の認証セッションの有効期限が切れています。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *preauthorization_authentication_session_missing* | 412 | この要求に関連付けられている認証セッションを取得できませんでした。 続行するには、サポートされているMVPDで再認証する必要があります。 |
|                    | *preauthorization_authentication_session_expired* | 412 | 現在の認証セッションの有効期限が切れています。 続行するには、サポートされているMVPDで再認証する必要があります。 |
| **認証** | *authorization_not_found* | 403、404 | 指定されたリソースの承認が見つかりませんでした。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
|                    | *authorization_expired* | 410 | 指定されたリソースの以前の認証の有効期限が切れています。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
| **再試行** | *network_received_error* | 403 | 関連付けられたパートナーサービスから応答を取得する際に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                    | *network_connection_timeout* | 403 | 関連付けられたパートナーサービスで接続タイムアウトが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|                    | *maximum_execution_time_exceeded* | 403 | 最大許容時間内にリクエストが完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。 |

### （レガシー） SDK の事前認証 API {#enhanced-error-codes-lists-sdks-preauthorize-api}

Adobe Pass Authentication SDK の事前認証 API と統合した場合にクライアントアプリケーションで発生する可能性のある拡張エラーコードについては、前の [ 節 ](#enhanced-error-codes-list-rest-api-v1) を参照してください。

## 応答処理 {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> ネットワークタイムアウトが発生した場合の認証リクエストの再試行や、セッションの有効期限が切れたときにユーザーに再認証を求めるなど、クライアントアプリケーションコードで自動的に処理できる拡張エラーコードがありますが、それ以外のタイプでは、設定の変更やAdobe Pass認証カスタマーケアチームとのインタラクションが必要になる場合があります。
>
> <br/>
>
> したがって、アドビの [Zendesk](https://adobeprimetime.zendesk.com) を使用してチケットを作成する際に、完全なエラー情報を収集して提供し、新しいアプリケーションや新機能を起動する前に、必要な変更がおこなわれるようにすることが重要です。

要約すると、拡張エラーコードを含む応答を処理する場合は、次の点を考慮する必要があります。

1. **エラーを返す API に依存しません**：生成される API に関係なく、拡張エラーコードの完全なカタログをサポートする一元化されたエラー処理ロジックを実装します。 いくつかの拡張エラーコードは API 間で共有され、一貫して処理する必要があります。

1. **トップレベルと項目レベルのエラー情報に依存しない**：トップレベルおよび項目レベルのエラー情報を通信方法に依存せずに処理し、拡張エラーコードの両方の送信形式を確実に処理します。

1. **両方のステータス値を確認**：常に、HTTP 応答のステータスコードと拡張エラーコードの「ステータス」フィールドの両方を確認します。 これらは異なる場合があり、どちらも貴重な情報を提供します。

1. **再試行ロジック**：再試行が必要なエラーの場合は、再試行が制限されていること（例：2～3）またはサーバーが圧倒されるのを避けるために指数バックオフで行われていることを確認します。 また、複数の項目を一度に処理するAdobe Pass Authentication API の場合（例：事前認証 API）、繰り返しリクエストには、リスト全体ではなく、「再試行」とマークされた項目のみを含める必要があります。

1. **設定の変更**：設定の変更が必要なエラーの場合は、新しいアプリケーションまたは新機能を起動する前に、必要な変更が行われていることを確認します。

1. **認証と承認**：認証と承認に関連するエラーの場合、必要に応じて、再認証を行うか新しい承認を取得するようにユーザーに促す必要があります。

1. **ユーザーのフィードバック**：任意。人間が判読できる「メッセージ」フィールドと（潜在的な）「詳細」フィールドを使用して、問題についてユーザーに通知します。 「詳細」テキストメッセージは、MVPDの事前認証や認証エンドポイントから、または劣化規則を適用する際のプログラマーから渡される場合があります。
