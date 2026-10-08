---
title: REST API V2 チェックリスト
description: REST API V2 チェックリスト
exl-id: 9095d1dd-a90c-4431-9c58-9a900bfba1cf
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '2578'
ht-degree: 0%
---
# REST API V2 チェックリスト {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このドキュメントでは、Adobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)を使用するクライアントアプリケーションを実装するプログラマーの必須の要件と推奨されるプラクティスを1か所にまとめています。

REST API V2を実装する場合、このドキュメントに従うことは受け入れ基準の一部と見なされ、統合を成功させるために必要なすべての手順が実行されていることを確認するためのチェックリストとして使用する必要があります。

>[!TIP]
>
> AI支援による開発については、[AI ルール ](rest-api-v2-ai-rules.md)を参照してください。このルールは、これらの要件をAI コーディングアシスタントの構造化ルールに変換します。

## 必須要件 {#mandatory-requirements}

### &#x200B;1. 登録フェーズ {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>登録済みアプリケーションの範囲</i></td>
      <td>REST API v2 スコープで登録済みアプリケーションを使用します。</td>
      <td>HTTP 401 「不正な」エラー応答をトリガーするリスク、システムリソースの過負荷、および遅延の増加。</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>クライアント資格情報のキャッシュ</i></td>
      <td>クライアント資格情報を永続的なストレージに保存し、アクセストークン要求ごとに再利用します。</td>
      <td>クライアント資格情報が再生成されると、認証が失われるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>アクセストークンのキャッシュ</i></td>
      <td>永続的なストレージにアクセストークンを保存し、有効期限が切れるまで再利用します。<br/><br/>REST API v2呼び出しごとに新しいトークンをリクエストしないでください。アクセストークンは、有効期限が切れた場合にのみ更新してください。</td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
</table>

### &#x200B;2. 設定フェーズ {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定の取得</i></td>
      <td>認証フェーズの前にMVPD （TV プロバイダー）の選択を促す必要がある場合にのみ、コンフィギュレーション応答を取得します。<br/><br/>次の場合は、コンフィギュレーション応答を取得する必要はありません。<ul><li>ユーザーは既に認証されています。</li><li>ユーザーには一時的なアクセス権が付与されます。</li><li>ユーザー認証の有効期限が切れましたが、以前に選択したMVPDのサブスクライバーであることを確認するように求められます。</li></ul></td>
      <td>システムリソースが過負荷になり、遅延が増大するリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>TV Provider Selection Caching</i></td>
      <td>ユーザーの有料TV プロバイダー（MVPD）の選択内容を永続的なストレージに保存し、その後のすべてのフェーズで使用します。<ul><li>設定レスポンスから、ユーザーが選択したMVPDの「ID」を保存します。</li><li>コンフィギュレーションレスポンスから、ユーザーが選択したMVPDの「displayName」を保存します。</li><li>設定レスポンスから、ユーザーが選択したMVPDの「logoUrl」を保存します。</li></ul></td>
      <td>システムリソースが過負荷になり、遅延が増大するリスクがあります。</td>
   </tr>
</table>

### &#x200B;3. 認証フェーズ {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ポーリングメカニズムの開始</i></td>
      <td>次の条件でポーリングメカニズムを開始します。<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md"> プライマリ（画面）アプリケーション内で実行された認証</a></b><ul><li>プライマリ（ストリーミング）アプリケーションは、ユーザーコンポーネントがSessions エンドポイントリクエストの「redirectUrl」パラメーターに指定されたURLを読み込んだ後、最終宛先ページに到達したときにポーリングを開始する必要があります。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md"> セカンダリ （画面） アプリケーション内で実行された認証</a></b><ul><li>プライマリ（ストリーミング）アプリケーションは、ユーザーが認証プロセスを開始するとすぐにポーリングを開始する必要があります（セッションエンドポイント応答を受信し、ユーザーに認証コードを表示した直後）。</li></ul></td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ポーリング機構の停止</i></td>
      <td>次の条件でポーリングメカニズムを停止します。<br/><br/><b>認証に成功</b><ul><li>ユーザーのプロファイル情報が正常に取得され、認証ステータスが確認されるため、ポーリングは不要になります。</li></ul><br/><b>認証セッションとコードの有効期限</b><ul><li>認証セッションとコードが期限切れになると、ユーザーは認証プロセスを再起動する必要があり、以前の認証コードを使用したポーリングはすぐに停止する必要があります。</li></ul><br/><b>新しい認証コードが生成されました</b><ul><li>ユーザーが新しい認証コードを要求した場合、既存のセッションは無効になり、以前の認証コードを使用したポーリングはすぐに停止する必要があります。</li></ul></td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ポーリングメカニズムの設定</i></td>
      <td>次の条件でポーリングメカニズムの頻度を設定します。<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md"> プライマリ（画面）アプリケーション内で実行される認証</a></b><ul><li>プライマリ（ストリーミング）アプリケーションは、3～5秒以上ごとにポーリングする必要があります。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md"> セカンダリ （画面） アプリケーション内で実行された認証</a></b><ul><li>プライマリ（ストリーミング）アプリケーションは、3～5秒ごとにポーリングする必要があります。</li></ul></td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>プロファイルのキャッシュ</i></td>
      <td>ユーザーのプロファイル情報の一部を永続的なストレージに保存し、パフォーマンスを向上させ、不要なREST API v2呼び出しを最小限に抑えます。<br/><br/> キャッシュでは、次のプロファイル応答フィールドに焦点を当てる必要があります：<br/><br/><b>mvpd</b><ul><li>クライアントアプリケーションはこれを使用して、ユーザーが選択したテレビプロバイダーを追跡し、事前認証フェーズまたは承認フェーズでさらに使用し続けることができます。</li><li>現在のユーザープロファイルが期限切れになると、クライアントアプリケーションは記憶されたMVPDの選択範囲を使用し、ユーザーに確認を依頼することができます。</li></ul><br/><b>属性</b><ul><li>様々なユーザーメタデータキー（zip、maxRatingなど）に基づいてユーザーエクスペリエンスをパーソナライズするために使用されます。</li><li>認証フローが完了すると、ユーザーメタデータが使用できるようになります。そのため、クライアントアプリケーションは、プロファイル情報に既に含まれているため、ユーザーメタデータ情報を取得するために別のエンドポイントをクエリする必要はありません。</li><li>特定のメタデータ属性は、MVPD（Charterなど）および特定のメタデータ属性（householdIDなど）に応じて、承認フェーズ中に更新される場合があります。 その結果、クライアントアプリケーションは、最新のユーザーメタデータを取得するために、認証後にProfiles APIを再度クエリする必要がある場合があります。</li></ul></td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
</table>

### &#x200B;4. （オプション）事前認証フェーズ {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前承認決定の取得</i></td>
      <td>コンテンツのフィルタリングには事前承認決定を使用し、再生決定には絶対に使用しない。</td>
      <td>プログラマー、MVPD、Adobe間の契約上の合意に違反するリスク。<br/><br/>監視および警告システムを回避するリスク。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前承認決定取得の取得</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a>を適切に処理し、アクションフィールドを利用して、必要な修復手順を決定します。<br/><br/>一部の強化されたエラーコードのみが再試行を許可しますが、ほとんどの場合、アクション フィールドで指定された代替解決策が必要です。<br/><br/>事前認証の決定を取得するために実装された再試行メカニズムが無限ループにならないこと、および再試行を合理的な数（2-3）に制限することを確認します。</td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前承認決定のキャッシュ</i></td>
      <td>アプリケーションの実行中にサブスクリプションが更新される頻度が低いため、メモリに許可に関する決定をキャッシュして、パフォーマンスを向上させ、不要なREST API v2呼び出しを最小限に抑えます。</td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
</table>

### &#x200B;5. 承認フェーズ {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>承認決定の取得</i></td>
      <td>事前認証の決定が存在するかどうかに関係なく、再生前に認証の決定を取得します。<br/><br/>再生中にメディアトークンが期限切れになった場合でも、ストリームを中断せずに続行できるようにし、ユーザーが次の再生リクエストを行う際に、同じリソースまたは別のリソースに関わらず、（新鮮な）メディアトークンを含む新しい承認決定をリクエストします。<br/><br/>長時間のライブストリームを実行している場合、コンテンツの一時停止、商用利用の開始、MRSSの変更時のアセットレベル設定の変更などのビデオ操作に従って、新しい承認決定をリクエストできます。</td>
      <td>プログラマー、MVPD、Adobe間の契約上の合意に違反するリスク。<br/><br/>監視および警告システムを回避するリスク。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>承認決定取得の取得</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a>を適切に処理し、アクションフィールドを利用して、必要な修復手順を決定します。<br/><br/>一部の強化されたエラーコードのみが再試行を許可しますが、ほとんどの場合、アクション フィールドで指定された代替解決策が必要です。<br/><br/>認証の決定を取得するために実装された再試行メカニズムが無限ループにならないこと、および再試行を合理的な数（2-3）に制限することを確認します。</td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。</td>
   </tr>
</table>

### &#x200B;6. ログアウトフェーズ {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ログアウトサポート</i></td>
      <td>ログアウト APIを実装して、ユーザーが手動でログアウトし、認証済みプロファイルを終了し、削除された各プロファイルに指定されたREST API v2 アクション名に従えるようにします。<ul><li>ログアウトエンドポイントをサポートするMVPDの場合、クライアントアプリケーションはユーザーエージェントで指定された「URL」に移動する必要があります。</li><li>「appleSSO」タイプのプロファイルの場合、クライアントアプリケーションは、パートナーレベル（Appleのシステム設定）からもログアウトするようにユーザーをガイドする必要があります。</li></ul></td>
      <td>クライアントアプリケーション側でサポートが欠落しているため、クライアントアプリケーションが誤動作するリスクがあります。</td>
   </tr>
</table>

### &#x200B;7. パラメーターとヘッダー {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Send Authorization Header</i></td>
      <td>REST API v2 リクエストごとに<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> ヘッダーを送信します。</td>
      <td>HTTP 401 「不正な」エラー応答をトリガーするリスク、システムリソースの過負荷、および遅延の増加。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Send AP-Device-Identifier Header</i></td>
      <td>REST API v2 リクエストごとに<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ヘッダーを送信します。<br/><br/> リクエストがデバイスの代理でサーバーから送信された場合でも、AP-Device-Identifier ヘッダー値は実際のストリーミングデバイス識別子を反映する必要があります。</td>
      <td>HTTP 400 「Bad Request」エラー応答のトリガー、システムリソースの過負荷、遅延の増加のリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>X-Device-Info ヘッダーを送信</i></td>
      <td>REST API v2 リクエストごとに<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> ヘッダーを送信します。<br/><br/> リクエストがデバイスの代理でサーバーから送信された場合でも、X-Device-Info ヘッダー値は実際のストリーミングデバイス情報を反映する必要があります。</td>
      <td>未知のプラットフォームから送信されたものとして分類され、安全でない扱いになり、認証TTLの短縮など、より制限の厳しいルールの対象となるリスク。<br/><br/>さらに、ストリーミングデバイス connectionIpやconnectionPortなどの一部のフィールドは、Spectrumのホームベース認証などの機能に必須です。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>安定したデバイス Id</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ヘッダーの更新または再起動時に変更されない安定したデバイス IDを計算して保存します。<br/><br/> ハードウェア IDのないプラットフォームの場合は、アプリケーション属性から一意のIDを生成して保持します。</td>
      <td>デバイス IDが変更されると、認証が失われるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>API参照に従う</i></td>
      <td>REST API v2の想定されるパラメーターとヘッダーのみを送信していることを確認します。</td>
      <td>HTTP 400 「Bad Request」エラー応答のトリガー、システムリソースの過負荷、遅延の増加のリスクがあります。</td>
   </tr>
</table>

### &#x200B;8. エラー処理 {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>エラーコード処理のサポートの強化</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a>を適切に処理し、アクションフィールドを利用して、必要な修復手順を決定します。<br/><br/>一部の強化されたエラーコードのみが再試行を許可しますが、ほとんどの場合、アクション フィールドで指定された代替解決策が必要です。<br/><br/>拡張エラーコード - REST API V2</a> ドキュメントに記載されている最も強化されたエラーコードは、アプリケーションを起動する前に開発フェーズで正しく処理された場合、完全に防ぐことができます。<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2"></td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。<br/><br/>拡張エラーコードの処理が見つからないため、クライアントアプリケーションが誤動作するリスクがあります。不明確なエラーメッセージ、不適切なユーザーガイダンス、誤ったフォールバック動作が発生します。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>HTTP エラー処理のサポート</i></td>
      <td>HTTP エラー応答（例：400、401、403、404、405、500）の処理と、前述のように、拡張エラーコードペイロードを含む成功レスポンス（例：200、201）の処理を区別します。<br/><br/>再試行が必要なHTTP エラーコードは限られていますが、ほとんどの場合は代替解決策が必要です。<br/><br/> アプリケーションを起動する前に開発段階で正しく処理すれば、ほとんどのHTTP エラー応答を完全に防ぐことができます。</td>
      <td>システムリソースが過負荷になり、遅延が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があります。<br/><br/>拡張エラーコードの処理が見つからないため、クライアントアプリケーションが誤動作するリスクがあります。不明確なエラーメッセージ、不適切なユーザーガイダンス、誤ったフォールバック動作が発生します。</td>
   </tr>
</table>

### &#x200B;9. テスト {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件定義</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ライフサイクルテスト</i></td>
      <td>Adobe Pass認証の公式な非実稼働環境を使用して、アプリケーションを開発およびテストします。<ul><li>Prequal-Production</li><li>リリースステージング</li></ul><br/>これらの環境で完全な品質保証（QA）を実行してから、Release-Productionに起動してください。<br/><br/> クライアントアプリケーションは、非実稼動環境でエンドツーエンドの検証を最初に完了せずにRelease-Productionに進んではなりません。</td>
      <td>重大な欠陥や大きな欠陥によって立ち上がるリスク：<br/><br/>短く効率的なデバッグパスを欠くと、Adobe サポートとエンジニアリングが迅速に介入する妨げになる可能性があります。</td>
   </tr>
</table>

## 推奨される慣行 {#recommended-practices}

### &#x200B;1. 登録フェーズ {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>アクセストークンの検証</i></td>
      <td>アクセストークンの有効性を先見的に確認し、有効期限が切れたときに更新します。<br/><br/>HTTP 401 「未承認」エラーを処理するための再試行メカニズムでは、最初に元のリクエストを再試行する前に、アクセストークンが更新されていることを確認します。</td>
      <td>HTTP 401 「不正な」エラー応答をトリガーするリスク、システムリソースの過負荷、および遅延の増加。</td>
   </tr>
</table>

### &#x200B;2. 設定フェーズ {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定のキャッシュ</i></td>
      <td>設定レスポンスをメモリまたは永続的なストレージに短期間（3～5分など）保存することで、パフォーマンスを向上させ、不要なREST API v2呼び出しを最小限に抑えます。</td>
      <td>システムリソースが過負荷になり、遅延が増大するリスクがあります。</td>
   </tr>
</table>

### &#x200B;3. 認証フェーズ {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証コード検証（第2画面認証）</i></td>
      <td>次の条件で/api/v2/authenticate APIを呼び出す前に、セカンダリ （2nd） アプリケーション （screen）でユーザー入力を介して送信された認証コードを検証します。<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">事前選択されたmvpd</a></b>を使用して、セカンダリ （screen） アプリケーション内で実行された認証<ul><li><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">認証セッションを再開</a> - POST /api/v2/{serviceProvider}/sessions/{code}を使用します</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">事前に選択されたmvpd</a></b>を使用せずに、セカンダリ（画面）アプリケーション内で認証が実行されました<ul><li><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">認証セッションを取得</a> - GET /api/v2/{serviceProvider}/sessions/{code}を使用します</li></ul><br/>提供された認証コードが間違って入力された場合、または認証セッションが期限切れになった場合、クライアントアプリケーションはエラーを受け取ります。</td>
      <td>認証中に様々なエラー応答やワークフローの問題が発生するリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>複数プロファイルのサポート</i></td>
      <td>クライアントアプリケーションが、ユーザーにプロファイルの選択を促したり、最も長い有効期間を持つプロファイルを自動的に選択するなど、カスタムロジックを適用したりすることで、複数のプロファイルを処理できるようにします。</td>
      <td>クライアントアプリケーション側でサポートが欠落しているため、クライアントアプリケーションが誤動作するリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>（オプション）非基本フローのサポート</i></td>
      <td>クライアントアプリケーションビジネスで必要な場合に備えて、非基本フローをサポートします。<ul><li>劣化したアクセスフロー（プレミアム機能）</li><li>一時的なアクセスフロー（プレミアム機能）</li><li>シングルサインオンアクセスフロー（標準機能）</li></ul></td>
      <td>理想的でないユーザーエクスペリエンスを生み出すリスクがあります。</td>
   </tr>
</table>

### &#x200B;4. （オプション）事前認証フェーズ {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ユーザーエクスペリエンス</i></td>
      <td>MVPDまたはAdobeから提供されるメッセージを拡張エラーコードで使用し、事前認証の決定が拒否された場合に、明確なユーザーフィードバックを表示します。</td>
      <td>理想的でないユーザーエクスペリエンスを生み出すリスクがあります。</td>
   </tr>
</table>

### &#x200B;5. 承認フェーズ {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>メディアトークンの検証</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Media Token Verifier</a> ライブラリを使用してメディアトークンを検証します。</td>
      <td>ストリームリッピングなどの不正行為のリスクを抱える。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ユーザーエクスペリエンス</i></td>
      <td>MVPDまたはAdobeから提供されたメッセージを拡張エラーコードで使用し、認証の決定が拒否された場合に、明確なユーザーフィードバックを表示します。</td>
      <td>理想的でないユーザーエクスペリエンスを生み出すリスクがあります。</td>
   </tr>
</table>

### &#x200B;6. ログアウトフェーズ {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ユーザーエクスペリエンス</i></td>
      <td>ログアウト APIは直接のユーザーリクエストに応じてのみ呼び出す必要があるため、拒否された事前認証や認証などのシナリオでは、ログアウト APIを自動的に（プログラムによって）呼び出さないようにします。</td>
      <td>認証が失敗していることをユーザーを混乱させるリスクがあります。</td>
   </tr>
</table>

### &#x200B;7. パラメーターとヘッダー {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>コードを再利用</i></td>
      <td>REST API v1のコードを再利用して、デバイス識別子とデバイス情報を少し調整して計算します。ただし、REST API v2で想定されるパラメーターとヘッダーのみを送信するようにしてください。<br/><br/>REST API v1のコードを再利用して、DCR APIを呼び出し、アクセストークンを取得します。</td>
      <td>-</td>
   </tr>
</table>

### &#x200B;8. テスト {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">慣行</th>
      <th style="background-color: #EFF2F7;">リスク</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>テスト範囲</i></td>
      <td>デバイスとプラットフォーム間で、次の基本フローがテストされていることを確認します。<br/><br/><b>認証フロー</b><ul><li>プライマリアプリ（画面）の認証シナリオ</li><li>セカンダリアプリ（画面）認証のシナリオ</li></ul><br/><b> （オプション）事前認証フロー</b><ul><li>許可の決定シナリオのテスト</li><li>テスト拒否の決定シナリオ</li></ul><br/><b>承認フロー</b><ul><li>許可の決定シナリオのテスト</li><li>テスト拒否の決定シナリオ</li></ul><br/><b> ログアウトフロー</b><br/><br/>さらに、他のアクセスフローをテストします（該当する場合）:<br/><br/><ul><li>劣化したアクセスフロー（プレミアム機能）</li><li>一時的なアクセスフロー（プレミアム機能）</li><li>シングルサインオンアクセスフロー（標準機能）</li></ul><br/>Adobe MVPDとの主要な統合（最も広く使用されているプロバイダーを含む）について説明します。</td>
      <td>頻繁にテストされていないプラットフォームや、ネガティブなシナリオのようなまれなフローにおいて、本番環境で予期せぬ失敗が発生するリスクが高まります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>テストツール</i></td>
      <td><a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a> web サイトを使用します。</td>
      <td>-</td>
   </tr>
</table>

## 概要 {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">フェーズ</th>
      <th style="background-color: #EFF2F7;">必須</th>
      <th style="background-color: #EFF2F7;">（強く）推奨</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>登録</i></td>
      <td>クライアント資格情報をキャッシュ <br/><br/> アクセス トークンをキャッシュ</td>
      <td>アクセストークンの検証と更新</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定</i></td>
      <td>設定応答の取得を最小化</td>
      <td>キャッシュ設定の応答</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証</i></td>
      <td>ポーリングメカニズムの微調整<br/><br/> プロファイルの一部をキャッシュ</td>
      <td>複数のプロファイルをサポート <br/><br/> サポートのデグラデーション機能（ビジネス要件の場合） <br/><br/>TempPass機能（ビジネス要件の場合） <br/><br/> シングルサインオン機能（ビジネス要件の場合）をサポート</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前認証</i></td>
      <td>キャッシュ許可の事前承認決定<br/><br/> メカニズムの微調整を再試行する</td>
      <td>拒否された事前認証の決定にエラーコードを使用することで、ユーザーエクスペリエンスを向上させます</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証</i></td>
      <td>ユーザーが再生を要求したときに承認決定を取得する<br/><br/> メカニズムの微調整を再試行する</td>
      <td>拒否された承認決定にエラーコードを使用して、ユーザーエクスペリエンスを向上させる<br/><br/> メディアトークンの検証</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ログアウト</i></td>
      <td>ユーザーが手動でログアウトできるようにログアウト APIを実装する</td>
      <td>ログアウト APIの自動呼び出しを避ける</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">必須</th>
      <th style="background-color: #EFF2F7;">（強く）推奨</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>パラメーターとヘッダー</i></td>
      <td>必須のヘッダーの仕様に従う</td>
      <td>REST API v1からのコードの再利用</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>エラー処理</i></td>
      <td>強化されたエラー処理の実装<br/><br/>HTTP エラー処理の実装</td>
      <td>-</td>
   </tr>
</table>
