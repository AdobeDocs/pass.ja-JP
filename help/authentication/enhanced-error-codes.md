---
title: 拡張エラーコード
description: 拡張エラーコード
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 87639ad93d8749ae7b1751cd13a099ccfc2636ac
workflow-type: tm+mt
source-wordcount: '2207'
ht-degree: 2%

---

# 拡張エラーコード {#enhanced-error-codes}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

このドキュメントでは、API エラーコードのリストと、アプリケーションに返される追加のエラー情報について説明します。

プログラマーアプリケーションで拡張エラーコードを使用するには、設定変更を有効にするために、サポートチームに対してリクエストを行う必要があります。

## 応答エラー処理 {#response-error-handling}

ほとんどの場合、Adobe Pass Authentication API には、特定のエラーが発生した理由や、問題を自動修正するための解決策に関する **意味のあるコンテキスト** を提供するために、応答本文にエラー情報が含まれています。  *ただし、認証またはログアウトフローが関連する特定のケースでは、Adobe Pass Authentication サービスがHTMLレスポンスまたは空の本文を返す場合があります。詳しくは、API のドキュメントを参照してください。*

特定のタイプのエラーは自動的に処理できますが（ネットワークタイムアウトの場合に認証リクエストを再試行したり、セッションの有効期限が切れた場合にユーザーの再認証を必要とするなど）、他のタイプでは、設定の変更やカスタマーケアチームとのインタラクションが必要になる場合があります。 このような場合、プログラマーは完全なエラー情報を収集して提供することが重要です。

Adobe Pass Authentication API は、エラーまたはエラーを示す HTTP ステータスコードを 400 ～ 500 の範囲で返します。 各 HTTP ステータスコードには、特定の影響があります。

- 4xx エラーコードは、エラーがクライアントによって生成され、エラーを修正するためにクライアントが追加の作業を行う必要があることを意味します（例えば、保護された API を呼び出す前にアクセストークンを取得したり、必要なパラメーターを指定するなど）

- 5xx エラーコードは、エラーがサーバーによって生成され、エラーを修正するためにサーバーが追加の作業を行う必要があることを意味します。

追加のエラー情報は、応答本文内の「エラー」フィールドに含まれます。

<table>
<thead>
  <tr>
    <th>名前</th>
    <th>タイプ</th>
    <th>例</th>
    <th>説明</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>エラー</td>
    <td><i>オブジェクト</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>リクエストの完了中に収集されたコレクションまたはエラーオブジェクトを参照します。</td>
  </tr>
</tbody>
</table>

</br>

複数の項目を処理するAdobe Pass API （事前認証 API など）では、特定の項目の処理が失敗し、項目レベルのエラー情報を使用して他の項目の処理が成功したかどうかを示す場合があります。 この場合、***&quot;error&quot;*** オブジェクトは項目レベルにあり、応答本文には複数の ***&quot;errors&quot;*** オブジェクトが含まれる可能性があります。API ドキュメントを参照してください。

</br>

**部分的な成功と項目レベルのエラーを含む例**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

各エラーオブジェクトには、次のパラメーターがあります。

| 名前 | タイプ | 例 | 制限付き | 説明 |
|---|---|----|:---:|---|
| *ステータス* | *整数* | *403* | &amp;check; | RFC 7231 に記載されている応答 HTTP ステータスコード（<https://tools.ietf.org/html/rfc7231#section-6>） <ul><li>400 無効なリクエスト</li><li>401 未認証</li><li>403 Forbidden</li><li>404 が見つかりません</li><li>405 メソッドは許可されていません</li><li>409 の競合</li><li>410 削除</li><li>412 事前条件が失敗しました</li><li>429 リクエストが多すぎます</li><li>500 間隔サーバーエラー</li><li>503 サービスを利用できません</li></ul> |
| *code* | *文字列* | *network_connection_failure* | &amp;check; | 標準のAdobe Pass認証エラーコード。 エラーコードの完全なリストは、次のとおりです。 |
| *message* | *文字列* | *テレビ プロバイダ サービスに接続できません* | | エンドユーザーに表示できる、人間が判読できるメッセージ。 |
| *詳細* | *文字列* | *サブスクリプションパッケージに「ライブ」チャネルが含まれていません* | | 場合によっては、詳細なメッセージが MVPD 認証エンドポイントによって、またはプログラマによって劣化ルールを介して提供されます。 <p> パートナーサービスからカスタムメッセージを受信しなかった場合、このフィールドはエラーフィールドに存在しない可能性があることに注意してください。 |
| *helpUrl* | *url* | &quot;`http://`&quot; | | このエラーが発生した理由と考えられる解決策に関する詳細情報へのリンクを示す URL。 <p>URI は絶対 URL を表すので、エラーコードから推論しないでください。 エラーコンテキストに応じて、別の URL を指定できます。 例えば、同じ bad_request エラーコードは、認証サービスと承認サービスで異なる url を生成します。 |
| *トレース* | *文字列* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | より複雑なシナリオで特定の問題を特定するためにサポートに連絡する際に使用できる、この応答の一意の ID。 |
| *アクション* | *文字列* | *再試行* | &amp;check; | 状況を修正するための推奨アクション： <ul><li> *なし* – 残念ながら、この問題を修正するための事前定義されたアクションはありません。 これは、パブリック API の呼び出しが正しくない可能性があります</li><li>*設定* - TVE ダッシュボードを使用するか、サポートに連絡して、設定を変更する必要があります。 </li><li>*application-registration* - アプリケーションは、自分自身を再度登録する必要があります。 </li><li>*認証* - ユーザーは認証または再認証する必要があります。 </li><li>*authorization* - ユーザーは、特定のリソースに対して認証を取得する必要があります。 </li><li>*degradation* – 何らかの形の最適化を適用する必要があります。 </li><li>*retry* - リクエストを再試行すると、問題が解決する場合があります</li><li>*retry-after* – 指定された期間の経過後にリクエストを再試行すると、問題が解決する場合があります。</li></ul> |

</br>

**注：**

- ***制限付き*** 列 *それぞれのフィールド値が有限セットを表しているかどうかを示します* （「*status*」フィールドの既存の HTTP ステータスコードなど）。 この仕様を今後更新した場合、制限リストに値が追加されることがありますが、既存の値は削除または変更されません。 制限のないフィールドには通常、任意のデータを含めることができますが、適切なサイズを確保するための制限が存在する場合があります。

- 各AdobeAdobeには、HTTP サービス全体を通してクライアントリクエストを特定する「Response-Request-Id」が含まれます。 「**trace**」フィールドはそれを補完し、それらを一緒に報告する必要があります。

## HTTP ステータスコードとエラーコード {#http-status-codes-and-error-codes}

様々なエラーコードと関連する HTTP ステータスコードの不一致は、古い SDK およびアプリケーションとの後方互換性に関する要件（
例 *unknown\_application* は 400 Bad Request を返しますが *unknown\_software\_statement* は 401 Unauthorized）を返します。 これらの不整合の解決は、今後のイテレーションでターゲットに設定されます。

## アクションとエラーコード {#actions-and-error-codes}

ほとんどのエラーコードでは、複数のアクションが手元の問題を修正するためのパスとして適格であったり、自動的に修正するために複数のアクションが必要であったりする場合があります。 エラーを修正する可能性が最も高いものを指定することを選択しました。 **アクション** は、次の 3 つのカテゴリに分けることができます。

1. リクエストコンテキストを修正しようとするもの（再試行、再試行後）
1. アプリケーション内でユーザーコンテキストの修正を試みるもの
（出願・登録・認証・認可）
1. アプリケーション間の統合コンテキストの修正を試みるもの
および ID プロバイダー（設定、低下）

第 1 カテゴリ（retry と retry-after）の場合は、同じリクエストを再試行するだけで問題が解決する可能性があります。 複数の項目を処理する API の場合、アプリケーションはリクエストを繰り返し、「retry」または「retry-after」アクションを持つ項目のみを含める必要があります。 「*retry-after*」アクションの場合、「<u>Retry-After</u>」ヘッダーは、アプリケーションがリクエストを繰り返すまでに待機する秒数を示します。

2 番目と 3 番目のカテゴリの場合、実際のアクションの実装は、アプリケーションの機能に大きく依存します。 例えば、「*degradation*」は、「15 分間の一時パスに切り替えてユーザーがコンテンツを再生できるようにする」または「automatic tool to apply AUTHN-ALL or AUTHZ-ALL degradation for its integration with the specified MVPD」として実装できます。 同様に、「*認証*」アクションでは、タブレットではパッシブ認証（バックチャネル認証）、接続された TV では完全な 2 画面目の認証フローをトリガーにすることができます。 だからこそ、スキーマとすべてのパラメーターを含む本格的な URL の提供を選択しました。

## エラーコード {#error-codes}

以下のエラーの表に、考えられるエラーコード、関連するメッセージ、考えられるアクションを示します。

| アクション | エラーコード | HTTP ステータスコード | 説明 |
|---|---|---|---|
| **なし** | *authorization_denied_by_mvpd* | 403 | MVPD は、指定されたリソースの認証を要求したときに「拒否」の決定を返しました。 |
|  | *authorization_denied_by_parental_controls* | 403 | MVPD は、指定されたリソースのペアレンタルコントロール設定が原因で「拒否」決定を返しました。 |
|  | *authorization_denied_by_programmer* | 403 | プログラマによって適用される最適化規則は、現在のユーザーに対して「拒否」の決定を強制します。 |
|  | *bad_request* | 400 | API リクエストが無効か、正しい形式ではありません。 API ドキュメントを確認して、リクエスト要件を決定します。 |
|  | *individualization_service_unavailable* | 503 | 個人化サービスを利用できないので、リクエストは失敗しました。 |
|  | *internal_error* | 500 | 内部サーバーエラーが発生したため、リクエストは失敗しました。 |
|  | *invalid_client_time* | 400 | クライアントマシンの日付/時刻/タイムゾーンが正しく設定されていません。 これにより、認証/承認エラーが発生する可能性があります。 |
|  | *invalid_custom_scheme* | 400 | アプリケーション登録で使用されている指定されたカスタム スキームが認識されません。 TVE ダッシュボード設定で、適切なカスタムスキーム値を確認してください。 |
|  | *invalid_domain* | 400 | リクエスターが無効なドメインを使用しています。 特定の要求者 ID で使用されるすべてのドメインは、Adobeで許可リストに登録する必要があります。 |
|  | *invalid_header* | 400 | 無効なヘッダーが含まれているため、要求は失敗しました。 API ドキュメントを参照して、リクエストに有効なヘッダーとその値に制限があるかどうかを判断してください。 |
|  | *invalid_http_method* | 405 | リクエストに関連付けられている HTTP メソッドはサポートされていません。 API ドキュメントを参照して、リクエストでサポートされる HTTP メソッドを判断してください。 |
|  | *invalid_parameter_value* | 400 | 無効なパラメーターまたはパラメーター値が含まれているため、要求は失敗しました。 API ドキュメントを参照して、リクエストに有効なパラメーターとその値に制限があるかどうかを判断してください。 |
|  | *invalid_resource_value* | 400 | 無効なリソースまたは形式が正しくないリソースが使用されたため、要求は失敗しました。 API ドキュメントを参照して、リクエストに対して複雑なリソースをエンコードする必要があるか、また、その値やサイズに制限があるかどうかを判断してください。 |
|  | *invalid_registration_code* | 404 | 指定された登録コードは無効であるか、有効期限が切れています。 |
|  | *invalid_service_configuration* | 500 | サービス設定が正しくないため、リクエストは失敗しました。 |
|  | *missing_authentication_header* | 400 | 特定の API に必要な認証ヘッダーが含まれていないので、リクエストは失敗しました。 |
|  | *missing_resource_mapping* | 400 | 指定されたリソースに対応するマッピングがありません。 必要なマッピングを修正するには、サポートにお問い合わせください。 |
|  | *preauthorization_denied_by_mvpd* | 403 | MVPD は、指定されたリソースの事前認証を要求したときに「拒否」決定を返しました。 |
|  | *preauthorization_denied_by_programmer* | 403 | プログラマーによって適用される最適化規則は、現在のユーザーに対して「拒否」の決定を強制します。 |
|  | *registration_code_service_unavailable* | 503 | 登録コード サービスが利用できないため、要求に失敗しました。 |
|  | *service_unavailable* | 503 | 認証サービスまたは承認サービスが利用できないので、要求は失敗しました。 |
|  | *access_token_unavailable* | 400 | アクセストークンの取得中に予期しないエラーが発生したため、リクエストに失敗しました。 使用可能なソフトウェア ステートメントおよび登録済みのカスタム スキームについては、TVE ダッシュボードの構成を確認してください。 |
|  | *unsupported_client_version* | 400 | このバージョンのAdobe Pass Authentication SDK は古すぎるので、サポートされなくなりました。 最新バージョンにアップグレードするために必要な手順については、API ドキュメントを確認してください。 |
| **設定** | *network_required_ssl* | 403 | ターゲット パートナーサービスの SSL 接続に問題があります。 サポートチームにお問い合わせください。 |
|  | *too_many_resources* | 403 | クエリされたリソースが多すぎるため、承認または事前承認の要求に失敗しました。 認証および事前認証制限を適切に設定するには、サポートチームにお問い合わせください。 |
|  | *unknown_programmer* | 400 | プログラマまたはサービス プロバイダが認識されていません。 TVE ダッシュボードを使用して、指定されたプログラマを登録します。 |
|  | *unknown_application* | 400 | アプリケーションが認識されません。 TVE ダッシュボードを使用して、指定されたアプリケーションを登録します。 |
|  | *unknown_integration* | 400 | 指定されたプログラマーと ID プロバイダーの統合が存在しません。 TVE ダッシュボードを使用して、必要な統合を作成します。 |
|  | *unknown_software_statement* | 401 | アクセス トークンに関連付けられたソフトウェア ステートメントが認識されません。 ソフトウェアのステータスを明確にするには、サポートチームにお問い合わせください。 |
| **application-registration** | *access_token_expired* | 401 | アクセストークンの有効期限が切れています。 API ドキュメントに示されているように、アプリケーションはアクセストークンを更新する必要があります。 |
|  | *invalid_access_token_signature* | 401 | アクセストークン署名が無効になりました。 API ドキュメントに示されているように、アプリケーションはアクセストークンを更新する必要があります。 |
|  | *invalid_client_id* | 401 | 関連付けられたクライアント識別子が認識されません。 アプリケーションは、API ドキュメントに記載されているアプリケーション登録プロセスに従う必要があります。 |
| **認証** | *authentication_session_expired* | 410 | 現在の認証セッションの有効期限が切れています。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *authentication_session_missing* | 401 | この要求に関連付けられている認証セッションを取得できませんでした。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *authentication_session_invalidated* | 401 | 認証セッションは ID プロバイダーによって無効化されました。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *authentication_session_issuer_mismatch* | 400 | 認証フローに対して示された MVPD が認証セッションを発行した MVPD と異なるため、認証リクエストは失敗しました。 続行するには、ユーザーは目的の MVPD で再認証する必要があります。 |
|  | *authorization_denied_by_hba_policies* | 403 | MVPD が、ホームベースの認証ポリシーが原因で「拒否」決定を返しました。 現在の認証はホームベースの認証フロー（HBA）を使用して取得されましたが、指定されたリソースの認証を要求する際にデバイスは自宅に存在しなくなりました。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *identity_not_recognized_by_mvpd* | 403 | ユーザー ID が MVPD によって認識されなかったため、承認要求は失敗しました。 |
| **認証** | *authorization_expired* | 410 | 指定されたリソースの以前の認証の有効期限が切れています。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
|  | *authorization_not_found* | 404 | 指定されたリソースの承認が見つかりませんでした。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
|  | *device_identifier_mismatch* | 403 | 指定されたデバイス識別子が承認デバイス識別子と一致しません。 続行するには、ユーザーが新しい認証を取得する必要があります。 |
| **再試行** | *network_connection_failure* | 403 | 関連付けられたパートナーサービスとの接続に失敗しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|  | *network_connection_timeout* | 403 | 関連付けられたパートナーサービスで接続タイムアウトが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|  | *network_received_error* | 403 | 関連付けられたパートナーサービスから応答を取得する際に読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|  | *maximum_execution_time_exceeded* | 403 | 最大許容時間内にリクエストが完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。 |
| **retry-after** | *too_many_requests* | 429 | 指定された間隔内に送信されたリクエストが多すぎます。 アプリケーションは、提案された期間の経過後にリクエストを再試行できます。 |
|  | *user_rate_limit_exceeded* | 429 | 指定された期間内に特定のユーザーによって発行されたリクエストが多すぎます。 アプリケーションは、提案された期間の経過後にリクエストを再試行できます。 |
