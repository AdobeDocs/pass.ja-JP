---
title: エラーコードの強化
description: エラーコードの強化
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 87639ad93d8749ae7b1751cd13a099ccfc2636ac
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# エラーコードの強化 {#enhanced-error-codes}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

このドキュメントでは、API エラーコードの一覧と、アプリケーションに返される追加のエラー情報を説明します。

プログラマーアプリケーションで拡張エラーコードを使用するには、構成の変更を加えて有効にするために、サポートチームにリクエストする必要があります。

## 応答エラー処理 {#response-error-handling}

ほとんどのシナリオで、Adobe Pass Authentication API は、 **意味のあるコンテキスト** ：特定のエラーが発生した理由や、問題を自動的に修正するための解決策。  *ただし、認証フローやログアウトフローが関与する特定のケースでは、Adobe Pass Authentication Services がHTML応答や空の本文を返す場合があります。詳しくは、 API ドキュメントを参照してください。*

特定のタイプのエラーは自動的に処理されます（ネットワークタイムアウトの場合の認証リクエストの再試行や、セッションの期限が切れた場合のユーザーの再認証の要求など）が、他のタイプでは設定の変更やカスタマーケアチームの操作が必要になります。 このような場合、プログラマーは完全なエラー情報を収集して提供することが重要です。

Adobe Pass Authentication API は、400～500 の範囲の HTTP ステータスコードを返し、エラーまたはエラーを示します。 各 HTTP ステータスコードには、次のような影響があります。

- 4xx エラーコードは、エラーがクライアントによって生成され、それを修正するために追加の作業（保護された API を呼び出す前にアクセストークンを取得する、または必要なパラメーターを指定するなど）がクライアントによって必要であることを意味します

- 5xx エラーコードは、エラーがサーバーによって生成され、サーバーがエラーを修正するために追加の作業をおこなう必要があることを意味します。

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
    <td><i>object</i></td>
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
    <td>リクエストの完了中に収集されたコレクションまたはエラーオブジェクトを指します。</td>
  </tr>
</tbody>
</table>

</br>

複数の項目（事前認証 API など）を処理するAdobe Pass API は、項目レベルのエラー情報を使用して、特定の項目の処理が失敗したかどうか、および他の項目の処理が成功したかどうかを示す場合があります。 この場合、 ***&quot;error&quot;*** オブジェクトが項目レベルに配置されており、応答本文に複数の ***&quot;errors&quot;*** オブジェクト — API ドキュメントを参照してください。

</br>

**部分的な成功と項目レベルのエラーの例**

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

各エラーオブジェクトには次のパラメーターがあります。

| 名前 | タイプ | 例 | 制限 | 説明 |
|---|---|----|:---:|---|
| *ステータス* | *整数* | *403* | &amp;check; | RFC 7231 に記載されている応答 HTTP ステータスコード (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 無効なリクエスト</li><li>401 未認証</li><li>403 Forbidden</li><li>404 Not found</li><li>405 メソッドは許可されていません</li><li>409 紛争</li><li>410 消失</li><li>412 事前条件が失敗しました</li><li>429 リクエストが多すぎます</li><li>500 Interval サーバーエラー</li><li>503 サービスを利用できません</li></ul> |
| *コード* | *文字列* | *network_connection_failure* | &amp;check; | 標準のAdobe Pass認証エラーコードです。 エラーコードの完全なリストは以下のとおりです。 |
| *メッセージ* | *文字列* | *TV プロバイダーサービスに連絡できません* | | エンドユーザーに表示できる、人間が読み取れるメッセージ。 |
| *詳細* | *文字列* | *サブスクリプションパッケージに「ライブ」チャネルが含まれていません* | | 場合によっては、詳細なメッセージが MVPD 認証エンドポイントによって、またはプログラマーによって、劣化ルールによって提供されることがあります。 <p> パートナーサービスからカスタムメッセージが受信されなかった場合、このフィールドがエラーフィールドに表示されない可能性があります。 |
| *helpUrl* | *url* | &quot;`http://`&quot; | | このエラーが発生した理由と考えられる解決策の詳細にリンクする URL。 <p>URI は絶対 URL を表し、エラーコードから推論しないでください。 エラーコンテキストに応じて、異なる URL を指定できます。 例えば、同じ bad_request エラーコードを実行すると、認証サービスと認証サービスで異なる URL が生成されます。 |
| *trace* | *文字列* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | より複雑なシナリオで特定の問題を識別するためにサポートに連絡する際に使用できる、この応答の一意の識別子です。 |
| *アクション* | *文字列* | *再試行* | &amp;check; | 状況の修正に推奨されるアクション： <ul><li> *なし*  — 残念ながら、この問題を修正するための事前定義済みのアクションはありません。 これは、パブリック API の不適切な呼び出しを示している可能性があります</li><li>*設定* - TVE ダッシュボードを通じて、またはサポートに問い合わせて、設定を変更する必要があります。 </li><li>*出願登録*  — アプリケーションは再度登録する必要があります。 </li><li>*認証*  — ユーザーは認証または再認証する必要があります。 </li><li>*認証*  — ユーザーは、特定のリソースの認証を取得する必要があります。 </li><li>*劣化*  — 何らかの形式の劣化を適用する必要があります。 </li><li>*再試行*  — リクエストを再試行すると、問題が解決する場合があります</li><li>*retry-after*  — 指定された時間が経過した後にリクエストを再試行すると、問題が解決する場合があります。</li></ul> |

</br>

**メモ：**

- ***制限*** 列 *それぞれのフィールド値が有限のセットを表しているかどうかを示します* ( 例えば、「 」の既存の HTTP ステータスコード&#x200B;*ステータス*&quot;フィールド ) に書き込みます。 この仕様の今後の更新では、制限付きリストに値が追加される可能性がありますが、既存の値は削除または変更されません。 通常、無制限のフィールドには任意のデータを含めることができますが、適切なサイズを確保するために制限が設けられる場合があります。

- 各Adobe応答には、HTTP サービス全体を通じてクライアント要求を識別する「Adobe — リクエスト —ID」が含まれます。 「**trace**」フィールドは、これを補完し、一緒に報告する必要があります。

## HTTP ステータスコードとエラーコード {#http-status-codes-and-error-codes}

様々なエラーコードと関連する HTTP ステータスコードの間に不整合があるのは、古い SDK やアプリケーション ( 例えば、 *unknown\_application* は 400 Bad Request を生成しますが、 *不明な\\ソフトウェア\_statement* は、401 Unauthorized を返します )。 これらの不整合の解決は、今後の繰り返しでターゲットになります。

## アクションとエラーコード {#actions-and-error-codes}

ほとんどのエラーコードでは、現在の問題を修正するための手段として複数のアクションを使用できます。また、自動的に修正するには複数のアクションが必要になる場合もあります。 エラーを修正する確率が最も高いものを示すことを選択しました。 The **アクション** は、次の 3 つのカテゴリに分割できます。

1. リクエストコンテキストの修正を試みるもの (retry、retry-after)
1. アプリケーション内のユーザーコンテキストを修正しようとするもの（アプリケーションの登録、認証、承認）
1. アプリケーションと id プロバイダー間の統合コンテキストの修正を試みるもの（設定、劣化）

1 番目のカテゴリ（再試行および再試行後）の場合、同じリクエストを再試行するだけで問題が解決する可能性があります。 複数の項目を処理する API の場合、アプリケーションはリクエストを繰り返し、「retry」または「retry-after」アクションを持つ項目のみを含める必要があります。 &quot;の&#x200B;*retry-after*&quot;アクション、&quot;<u>Retry-After</u>「 」ヘッダーは、アプリケーションが要求を繰り返すまでに待機する秒数を示します。

2 番目と 3 番目のカテゴリの場合、実際のアクションの実装は、アプリケーションの機能に大きく依存します。 例：*劣化*&quot;は、&quot;ユーザーがコンテンツを再生できるように 15 分間の一時パスに切り替える&quot;、または&quot;指定した MVPD との統合に AUTHN-ALL または AUTHZ-ALL 分解を適用する自動ツール&quot;として実装できます。 「*認証*「 」アクションは、タブレットでパッシブトリガー（バックチャネル認証）を使用し、接続された TV ではフル 2 画面認証フローを使用できます。 そのため、完全に 1 つの URL にスキーマとすべてのパラメーターを提供することを選択しました。

## エラーコード {#error-codes}

以下のエラーの表に、考えられるエラーコード、関連するメッセージおよび実行可能なアクションを示します。

| アクション | エラーコード | HTTP ステータスコード | 説明 |
|---|---|---|---|
| **なし** | *authorization_denied_by_mvpd* | 403 | MVPD は、指定されたリソースに対する認証をリクエストする際に、「拒否」の決定を返しました。 |
|  | *authorization_denied_by_parental_controls* | 403 | MVPD は、指定されたリソースに対する親の制御設定により、「拒否」の決定を返しました。 |
|  | *authorization_denied_by_programmer* | 403 | プログラマが適用した劣化規則は、現在のユーザに対して「拒否」の決定を強制します。 |
|  | *bad_request* | 400 | API リクエストの形式が無効か、正しくありません。 API ドキュメントを確認して、リクエスト要件を判断してください。 |
|  | *individualization_service_unavailable* | 503 | 個別化サービスが利用できないため、リクエストに失敗しました。 |
|  | *internal_error* | 500 | 内部サーバーエラーが原因でリクエストに失敗しました。 |
|  | *invalid_client_time* | 400 | クライアントマシンの日付/時刻/タイムゾーンが正しく設定されていません。 認証/認証エラーが発生する可能性があります。 |
|  | *invalid_custom_scheme* | 400 | アプリケーション登録で使用される指定されたカスタムスキームは認識されません。 TVE ダッシュボードの設定で、適切なカスタムスキーム値を確認してください。 |
|  | *invalid_domain* | 400 | 要求元が無効なドメインを使用しています。 特定の要求者 ID で使用されるすべてのドメインは、Adobeでホワイトリストに登録する必要があります。 |
|  | *invalid_header* | 400 | 無効なヘッダーが含まれているので、リクエストに失敗しました。 API ドキュメントを確認して、リクエストに対して有効なヘッダーと、その値に制限があるかどうかを判断します。 |
|  | *invalid_http_method* | 405 | リクエストに関連付けられている HTTP メソッドはサポートされていません。 API ドキュメントを確認し、リクエストでサポートされている HTTP メソッドを特定します。 |
|  | *invalid_parameter_value* | 400 | 無効なパラメーターまたはパラメーター値が含まれていたので、リクエストに失敗しました。 API ドキュメントを確認して、リクエストに有効なパラメーターと、その値に制限があるかどうかを判断します。 |
|  | *invalid_resource_value* | 400 | 無効なまたは形式の正しくないリソースが使用されたので、リクエストに失敗しました。 API ドキュメントを確認して、複雑なリソースをリクエストに対してエンコードする必要がある方法と、その値やサイズに制限があるかどうかを判断します。 |
|  | *invalid_registration_code* | 404 | 指定した登録コードは有効でないか、期限が切れています。 |
|  | *invalid_service_configuration* | 500 | サービス設定が正しくないため、リクエストに失敗しました。 |
|  | *missing_authentication_header* | 400 | 特定の API に必要な認証ヘッダーが含まれていないので、リクエストに失敗しました。 |
|  | *missing_resource_mapping* | 400 | 指定したリソースに対応するマッピングがありません。 必要なマッピングを修正するには、サポートにお問い合わせください。 |
|  | *preauthorization_denied_by_mvpd* | 403 | MVPD は、指定されたリソースに対する事前認証をリクエストする際に、「拒否」の決定を返しました。 |
|  | *preauthorization_denied_by_programmer* | 403 | プログラマーが適用する劣化ルールは、現在のユーザーに対して「拒否」の決定を適用します。 |
|  | *registration_code_service_unavailable* | 503 | 登録コードサービスを利用できないので、リクエストに失敗しました。 |
|  | *service_unavailable* | 503 | 認証または承認サービスが使用できないため、リクエストに失敗しました。 |
|  | *access_token_unavailable* | 400 | アクセストークンの取得中に予期しないエラーが発生したため、リクエストに失敗しました。 TVE ダッシュボードの設定で、利用可能なソフトウェア文と登録済みのカスタムスキームを確認してください。 |
|  | *unsupported_client_version* | 400 | このバージョンのAdobe Pass Authentication SDK は古すぎるので、サポートされなくなりました。 最新バージョンにアップグレードするために必要な手順については、 API ドキュメントを参照してください。 |
| **設定** | *network_required_ssl* | 403 | ターゲットパートナーサービスの SSL 接続に問題があります。 サポートチームにお問い合わせください。 |
|  | *too_many_resources* | 403 | 照会されたリソースが多すぎるので、承認または事前認証要求に失敗しました。 認証と事前認証の制限を適切に設定するには、サポートチームにお問い合わせください。 |
|  | *unknown_programmer* | 400 | プログラマーまたはサービスプロバイダーが認識されません。 TVE ダッシュボードを使用して、指定したプログラマを登録します。 |
|  | *unknown_application* | 400 | アプリケーションが認識されません。 TVE ダッシュボードを使用して、指定したアプリケーションを登録します。 |
|  | *unknown_integration* | 400 | 指定されたプログラマーと ID プロバイダー間の統合が存在しません。 TVE ダッシュボードを使用して、必要な統合を作成します。 |
|  | *unknown_software_statement* | 401 | アクセストークンに関連付けられたソフトウェアステートメントが認識されません。 ソフトウェアのステートメントの状況を明確にするために、サポートチームに問い合わせてください。 |
| **出願登録** | *access_token_expired* | 401 | アクセストークンの有効期限が切れています。 アプリケーションは、API ドキュメントに記載されているようにアクセストークンを更新する必要があります。 |
|  | *invalid_access_token_signature* | 401 | アクセストークンの署名が無効になりました。 アプリケーションは、API ドキュメントに記載されているようにアクセストークンを更新する必要があります。 |
|  | *invalid_client_id* | 401 | 関連付けられたクライアント識別子が認識されません。 アプリケーションは、API ドキュメントに示されているアプリケーション登録プロセスに従う必要があります。 |
| **認証** | *authentication_session_expired* | 410 | 現在の認証セッションの有効期限が切れました。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *authentication_session_missing* | 401 | このリクエストに関連付けられた認証セッションを取得できませんでした。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *authentication_session_invalidated* | 401 | 認証セッションは ID プロバイダーによって無効化されました。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *authentication_session_issuer_mismatch* | 400 | 認証フローに対する指示された MVPD が認証セッションを発行した MVPD と異なるため、認証要求が失敗しました。 続行するには、ユーザーが目的の MVPD で再認証する必要があります。 |
|  | *authorization_denied_by_hba_policies* | 403 | MVPD は、ホームベースの認証ポリシーに基づいて「拒否」の決定を返しました。 現在の認証は、ホームベースの認証フロー (HBA) を使用して取得されましたが、指定したリソースの認証を要求する際に、デバイスがホームになくなりました。 続行するには、サポートされている MVPD でユーザーを再認証する必要があります。 |
|  | *identity_not_recognized_by_mvpd* | 403 | ユーザー ID が MVPD に認識されなかったため、認証リクエストが失敗しました。 |
| **認証** | *authorization_expired* | 410 | 指定したリソースの以前の認証が期限切れになりました。 続行するには、新しい認証を取得する必要があります。 |
|  | *authorization_not_found* | 404 | 指定されたリソースの認証が見つかりませんでした。 続行するには、新しい認証を取得する必要があります。 |
|  | *device_identifier_mismatch* | 403 | 指定されたデバイス識別子は、認証デバイスの識別子と一致しません。 続行するには、新しい認証を取得する必要があります。 |
| **再試行** | *network_connection_failure* | 403 | 関連するパートナーサービスとの接続に失敗しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|  | *network_connection_timeout* | 403 | 関連するパートナーサービスとの接続タイムアウトが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|  | *network_received_error* | 403 | 関連するパートナーサービスからの応答を取得する際に、読み取りエラーが発生しました。 リクエストを再試行すると、問題が解決する場合があります。 |
|  | *maximum_execution_time_exceeded* | 403 | リクエストは、許可された最大時間内に完了しませんでした。 リクエストを再試行すると、問題が解決する場合があります。 |
| **retry-after** | *too_many_requests* | 429 | 一定の間隔内に送信されたリクエストが多すぎます。 推奨期間が経過した後に、アプリケーションがリクエストを再試行できます。 |
|  | *user_rate_limit_exceeded* | 429 | 特定の期間内に特定のユーザーから発行されたリクエストが多すぎます。 推奨期間が経過した後に、アプリケーションがリクエストを再試行できます。 |
