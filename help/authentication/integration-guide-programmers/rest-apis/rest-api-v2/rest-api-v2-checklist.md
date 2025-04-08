---
title: REST API V2 チェックリスト
description: REST API V2 チェックリスト
exl-id: 9095d1dd-a90c-4431-9c58-9a900bfba1cf
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2545'
ht-degree: 0%

---

# REST API V2 チェックリスト {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass認証 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) を使用するクライアントアプリケーションを実装するプログラマー向けの必須要件と推奨事項を 1 か所にまとめています。

このドキュメントに従う必要があるのは、REST API V2 を実装する際に、受け入れ基準の一部と見なす必要があります。また、統合を成功させるために必要なすべての手順を実行するためのチェックリストとして使用する必要があります。

## 必須の要件 {#mandatory-requirements}

### 1.登録段階 {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>登録済みアプリケーションスコーピング</i></td>
      <td>REST API v2 スコープで登録済みアプリケーションを使用します。</td>
      <td>HTTP 401 「未認証」エラー応答がトリガーされ、システムリソースが過負荷になり、待ち時間が長くなるリスクがあります。</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>クライアント資格情報のキャッシュ</i></td>
      <td>クライアント資格情報を永続ストレージに保存し、アクセストークン要求ごとに再利用します。</td>
      <td>クライアント資格情報を再生成する際に、認証が失われるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>アクセストークンのキャッシュ</i></td>
      <td>アクセストークンを永続的なストレージに保存し、期限が切れるまで再利用します。<br/><br/>REST API v2 呼び出しごとに新しいトークンをリクエストしないでください。アクセストークンは期限切れになった場合にのみ更新します。</td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
</table>

### 2.設定フェーズ {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定の取得</i></td>
      <td>認証段階の前にMVPD（TV プロバイダー）を選択するように促す必要がある場合にのみ、設定応答を取得します。<br/><br/> 次の場合、設定応答を取得する必要はありません。<ul><li>ユーザーは既に認証済みです。</li><li>ユーザーには一時的なアクセスが提供されます。</li><li>ユーザー認証の有効期限が切れているが、以前に選択したMVPDの購読者であることを確認するプロンプトをユーザーに表示できます。</li></ul></td>
      <td>システムリソースを過負荷にして、待ち時間が長くなるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>TV プロバイダー選択のキャッシュ</i></td>
      <td>ユーザーの有料テレビプロバイダー（MVPD）の選択内容を永続ストレージに保存して、以降のすべてのフェーズで使用できるようにします。<ul><li>設定応答ストアから、ユーザーが選択したMVPD「id」を保存します。</li><li>configuration response store から、ユーザーが選択したMVPD「displayName」を保存します。</li><li>Configuration response store から、ユーザーが選択したMVPD「logoUrl」を保存します。</li></ul></td>
      <td>システムリソースを過負荷にして、待ち時間が長くなるリスクがあります。</td>
   </tr>
</table>

### 3.認証フェーズ {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ポーリングメカニズムの開始</i></td>
      <td>次の条件でポーリングメカニズムを起動します。<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md"> プライマリ（画面）アプリケーション内で認証を実行 </a></b><ul><li>プライマリ（ストリーミング）アプリケーションでは、ブラウザーコンポーネントがセッションエンドポイントリクエストの「redirectUrl」パラメーターに指定された URL を読み込んだ後、ユーザーが最終的な宛先ページに到達すると、ポーリングを開始する必要があります。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md"> セカンダリ（画面）アプリケーション内で実行される認証 </a></b><ul><li>プライマリ（ストリーミング）アプリケーションでは、ユーザーが認証プロセスを開始するとすぐに、つまり、セッションエンドポイントの応答を受信して認証コードをユーザーに表示した直後に、ポーリングを開始する必要があります。</li></ul></td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ポーリングメカニズムの停止</i></td>
      <td>次の条件でポーリングメカニズムを停止します。<br/><br/><b> 認証に成功 </b><ul><li>ユーザーのプロファイル情報は正常に取得され、ユーザーの認証ステータスが確認されるので、ポーリングは不要になりました。</li></ul><br/><b> 認証セッションとコードの有効期限 </b><ul><li>認証セッションとコードの有効期限が切れると、ユーザーは認証プロセスを再開する必要があり、以前の認証コードを使用したポーリングは直ちに停止する必要があります。</li></ul><br/><b> 新しい認証コードが生成されました </b><ul><li>ユーザーが新しい認証コードを要求すると、既存のセッションが無効になり、前の認証コードを使用したポーリングがすぐに停止されます。</li></ul></td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ポーリングメカニズムの設定</i></td>
      <td>次の条件でポーリングメカニズムの頻度を設定します。<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md"> プライマリ（画面）アプリケーション内で認証が実行され </a></b> す。<ul><li>プライマリ（ストリーミング）アプリケーションは、3 ～ 5 秒以上ごとにポーリングする必要があります。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md"> セカンダリ（画面）アプリケーション内で実行される認証 </a></b><ul><li>プライマリ（ストリーミング）アプリケーションは、3～5 秒ごとにポーリングする必要があります。</li></ul></td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>プロファイルのキャッシュ</i></td>
      <td>パフォーマンスを向上させ、不要な REST API v2 呼び出しを最小限に抑えるために、ユーザーのプロファイル情報の一部を永続ストレージに保存します。<br/><br/> キャッシュは、次のプロファイル応答フィールドに焦点を当てる必要があります：<br/><br/><b>mvpd</b><ul><li>クライアントアプリケーションは、これを使用してユーザーが選択したテレビプロバイダーを追跡し、事前認証または認証フェーズ中にさらに使用できます。</li><li>現在のユーザープロファイルの有効期限が切れると、クライアントアプリケーションは記憶されているMVPDの選択内容を使用し、ユーザーに確認を求めることができます。</li></ul><br/><b> 属性 </b><ul><li>様々なユーザーメタデータキー（zip、maxRating など）に基づいてユーザーエクスペリエンスをパーソナライズするために使用されます。</li><li>ユーザーメタデータは認証フローが完了すると使用できるようになります。したがって、ユーザーメタデータ情報は既にプロファイル情報に含まれているため、クライアントアプリケーションはユーザーメタデータ情報を取得するために別のエンドポイントをクエリする必要はありません。</li><li>MVPD（Charter など）や特定のメタデータ属性（householdID など）に応じて、特定のメタデータ属性は認証フェーズ中に更新される場合があります。 その結果、最新のユーザーメタデータを取得するために、クライアントアプリケーションが認証後にプロファイル API を再度クエリする必要が生じる場合があります。</li></ul></td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
</table>

### 4. （オプション）事前認証フェーズ {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前承認決定の取得</i></td>
      <td>コンテンツのフィルタリングには事前認証の決定を使用し、再生の決定には使用しません。</td>
      <td>プログラマー、MVPD およびAdobe間の契約上の合意に違反するリスク。<br/><br/> 監視および警告システムをバイパスするリスク。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前承認決定の取得の再試行</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> を適切に処理し、アクションフィールドを利用して、必要な是正手順を決定します。<br/><br/> 再試行が必要な拡張エラーコードは限られていますが、ほとんどの場合、アクションフィールドで指定された代替解決策が必要です。<br/><br/> 事前認証決定を取得するために実装された再試行メカニズムが無限ループを引き起こさず、再試行が適切な数（2-3 など）に制限されていることを確認します。</td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前承認決定のキャッシュ</i></td>
      <td>アプリケーション実行中のサブスクリプション更新の頻度が低いため、成功した許可決定をメモリにキャッシュして、パフォーマンスを向上させ、不要な REST API v2 呼び出しを最小限に抑えます。</td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
</table>

### 5.認証フェーズ {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>承認決定の取得</i></td>
      <td>再生前に認証決定を取得 – 事前認証の決定が存在するかどうかに関係なく。<br/><br/> メディアトークンが再生中に期限切れになり、ユーザーが次の再生リクエストを行うときに（新規）メディアトークンを含む新しい認証決定をリクエストした場合でも、同じリソースまたは異なるリソースに対するものであっても、ストリームが中断されることなく続行することを許可します。<br/><br/> 長時間実行されるライブストリームでは、コンテンツの一時停止、コマーシャルブレークの開始、MRSS の変更に伴うアセットレベルの設定の変更など、ビデオ操作に続いて、新しい承認決定をリクエストする場合があります。</td>
      <td>プログラマー、MVPD およびAdobe間の契約上の合意に違反するリスク。<br/><br/> 監視および警告システムをバイパスするリスク。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>承認決定の取得の再試行</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> を適切に処理し、アクションフィールドを利用して、必要な是正手順を決定します。<br/><br/> 再試行が必要な拡張エラーコードは限られていますが、ほとんどの場合、アクションフィールドで指定された代替解決策が必要です。<br/><br/> 認証決定を取得するために実装された再試行メカニズムが無限ループを引き起こさず、再試行が適切な数（2-3 など）に制限されていることを確認します。</td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。</td>
   </tr>
</table>

### 6. ログアウトフェーズ {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ログアウトのサポート</i></td>
      <td>ログアウト API を実装して、ユーザーが手動でログアウトし、認証済みプロファイルを終了して、削除されたプロファイルごとに指定された REST API v2 アクション名に従えるようにします。<ul><li>ログアウトエンドポイントをサポートする MVPD の場合、クライアントアプリケーションは、ユーザーエージェントで指定された「url」に移動する必要があります。</li><li>「appleSSO」タイプのプロファイルの場合、クライアントアプリケーションは、パートナーレベル（Appleのシステム設定）からもログアウトするようにユーザーをガイドする必要があります。</li></ul></td>
      <td>クライアントアプリケーション側のサポートが不足しているため、クライアントアプリケーションが誤動作するリスクがあります。</td>
   </tr>
</table>

### 7. パラメーターとヘッダー {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証ヘッダーの送信</i></td>
      <td>すべての REST API v2 リクエストに対して <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> ヘッダーを送信します。</td>
      <td>HTTP 401 「未認証」エラー応答がトリガーされ、システムリソースが過負荷になり、待ち時間が長くなるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>AP-Device-Identifier ヘッダーの送信</i></td>
      <td>すべての REST API v2 リクエストに対して <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ヘッダーを送信します。<br/><br/> デバイスの代わりにサーバーからリクエストが送信された場合でも、AP-Device-Identifier ヘッダー値には、実際のストリーミングデバイスの識別子が反映されている必要があります。</td>
      <td>HTTP 400 「無効なリクエスト」エラー応答がトリガーされ、システムリソースが過負荷になり、待ち時間が長くなるリスク。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>X-Device-Info ヘッダーを送信</i></td>
      <td>すべての REST API v2 リクエストに対して <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> ヘッダーを送信します。<br/><br/> デバイスの代わりにサーバーからリクエストが送信された場合でも、X-Device-Info ヘッダー値には、実際のストリーミングデバイス情報が反映されている必要があります。</td>
      <td>リスクが、不明なプラットフォームから発生し、安全でないと見なされ、認証 TTL の短縮など、より制限が厳しいルールの対象となる。<br/><br/> さらに、ストリーミングデバイス connectionIp や connectionPort などの一部のフィールドは、Spectrum のホームベース認証などの機能に必須です。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>安定したデバイス識別子</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ヘッダーの更新やリブートを経ても変化しない、安定したデバイス識別子を計算して保存します。<br/><br/> ハードウェア ID のないプラットフォームの場合、アプリケーション属性から一意の ID を生成して保持します。</td>
      <td>デバイス識別子が変更されると、認証が失われるリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>API リファレンスに従う</i></td>
      <td>REST API v2 で想定されるパラメーターとヘッダーのみを送信していることを確認します。</td>
      <td>HTTP 400 「無効なリクエスト」エラー応答がトリガーされ、システムリソースが過負荷になり、待ち時間が長くなるリスク。</td>
   </tr>
</table>

### 8. エラー処理 {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>エラーコード処理のサポートの強化</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> を適切に処理し、アクションフィールドを利用して、必要な是正手順を決定します。<br/><br/> 再試行が必要な拡張エラーコードは限られていますが、ほとんどの場合、アクションフィールドで指定された代替解決策が必要です。<br/><br/><a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2"> 拡張エラーコード - REST API V2</a> ドキュメントにリストされているほとんどの拡張エラーコードは、アプリケーションを起動する前の開発段階で正しく処理すれば、完全に防止できます。</td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。<br/><br/> 拡張エラーコードの処理が欠落しているために、クライアントアプリケーションが誤動作するリスクがあります。その結果、不明なエラーメッセージが表示されたり、誤ったユーザーガイダンスが表示されたり、誤ったフォールバック動作が発生したりします。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>HTTP エラー処理のサポート</i></td>
      <td>上記のように、HTTP エラー応答（400、401、403、404、405、500 など）の処理と、拡張エラーコードペイロードを含む成功応答（200、201 など）の処理を区別します。<br/><br/> 再試行を保証するのは HTTP エラーコードの数が限られていますが、ほとんどの場合、別の解決策が必要です。<br/><br/> ほとんどの HTTP エラー応答は、アプリケーションを起動する前の開発フェーズで正しく処理すれば、完全に防ぐことができます。</td>
      <td>システムリソースを過負荷にし、待ち時間が増加し、HTTP 429 「Too Many Requests」エラー応答がトリガーされる可能性があるリスクがあります。<br/><br/> 拡張エラーコードの処理が欠落しているために、クライアントアプリケーションが誤動作するリスクがあります。その結果、不明なエラーメッセージが表示されたり、誤ったユーザーガイダンスが表示されたり、誤ったフォールバック動作が発生したりします。</td>
   </tr>
</table>

### 9. テスト {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要件</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ライフサイクルテスト</i></td>
      <td>公式のAdobe Pass認証非実稼動環境を使用して、アプリケーションを開発およびテストします。<ul><li>前生産</li><li>リリース – ステージング</li></ul><br/>リリース版の実稼働を開始する前に、これらの環境で徹底的な品質保証（QA）を実行します。<br/><br/> クライアントアプリケーションは、実稼動以外の環境で最初にエンドツーエンドの検証を完了しない限り、リリース実稼動に進んではなりません。</td>
      <td>重大な欠陥や重大な欠陥が発生するリスク。<br/><br/> 短く効率的なデバッグパスがないと、Adobe サポートおよびエンジニアリングがすぐに介入できなくなる可能性があります。</td>
   </tr>
</table>

## 推奨プラクティス {#recommended-practices}

### 1.登録段階 {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>アクセストークンの検証</i></td>
      <td>アクセストークンの有効期限が切れたときに、プロアクティブに有効性を確認して更新します。<br/><br/>HTTP 401 「未認証」エラーを処理する再試行メカニズムでは、最初にアクセストークンを更新してから元のリクエストを再試行するようにしてください。</td>
      <td>HTTP 401 「未認証」エラー応答がトリガーされ、システムリソースが過負荷になり、待ち時間が長くなるリスクがあります。</td>
   </tr>
</table>

### 2.設定フェーズ {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定のキャッシュ</i></td>
      <td>パフォーマンスを向上させ、不要な REST API v2 呼び出しを最小限に抑えるために、設定応答をメモリまたは永続的なストレージに短期間（例：3～5 分）保存します。</td>
      <td>システムリソースを過負荷にして、待ち時間が長くなるリスクがあります。</td>
   </tr>
</table>

### 3.認証フェーズ {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証コードの検証（2 画面目の認証）</i></td>
      <td>セカンダリ（2 番目）アプリケーション（画面）でユーザー入力を通じて送信された認証コードを検証してから、次の条件で/api/v2/authenticate API を呼び出します。<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd"> セカンダリ（画面）アプリケーション内で実行される認証で、事前に選択された mvpd を使用 </a></b> ます。<ul><li><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md"> 認証セッションの再開 </a> を利用する – POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd"> 事前に選択された mvpd を使用せずに、セカンダリ（画面）アプリケーション内で認証を実行 </a></b><ul><li><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md"> 認証セッションの取得 </a> を利用する – GET /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/>指定された認証コードが正しく入力されなかった場合、または認証セッションが期限切れになった場合、クライアントアプリケーションにエラーが発生します。</td>
      <td>認証中に様々なエラー応答やワークフローの問題が発生するリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>複数のプロファイルのサポート</i></td>
      <td>ユーザーにプロファイルを選択するように促すか、有効期間が最も長いプロファイルを自動的に選択するなどのカスタムロジックを適用して、クライアントアプリケーションが複数のプロファイルを処理できるようにします。</td>
      <td>クライアントアプリケーション側のサポートが不足しているため、クライアントアプリケーションが誤動作するリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>（任意）非基本フローのサポート</i></td>
      <td>クライアントアプリケーションビジネスで必要とされる次のような場合に、非基本フローをサポートします。<ul><li>アクセス・フローの低下（プレミアム機能）</li><li>一時的なアクセスフロー（プレミアム機能）</li><li>シングルサインオンアクセスフロー（標準機能）</li></ul></td>
      <td>理想的でないユーザーエクスペリエンスを作成するリスクがあります。</td>
   </tr>
</table>

### 4. （オプション）事前認証フェーズ {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ユーザーエクスペリエンス</i></td>
      <td>MVPD またはAdobeが拡張エラーコードを介して提供するメッセージを使用して、事前認証の決定が拒否された場合に、明確なユーザーフィードバックを表示します。</td>
      <td>理想的でないユーザーエクスペリエンスを作成するリスクがあります。</td>
   </tr>
</table>

### 5.認証フェーズ {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>メディアトークンの検証</i></td>
      <td><a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier"> メディアトークン検証 </a> ライブラリを使用してメディアトークンを検証します。</td>
      <td>ストリームリッピングなどの不正スキームをリスクにさらします。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ユーザーエクスペリエンス</i></td>
      <td>MVPD またはAdobeが拡張エラーコードを介して提供するメッセージを使用して、認証の決定が拒否された場合に明確なユーザーフィードバックを表示します。</td>
      <td>理想的でないユーザーエクスペリエンスを作成するリスクがあります。</td>
   </tr>
</table>

### 6. ログアウトフェーズ {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ユーザーエクスペリエンス</i></td>
      <td>ログアウト API は直接ユーザーリクエストに応答してのみ呼び出す必要があるので、事前認証や承認が拒否されるなどのシナリオでは、（プログラム的に）ログアウト API の呼び出しを自動的に避けます。</td>
      <td>認証が失敗すると、ユーザーが混乱するリスクがあります。</td>
   </tr>
</table>

### 7. パラメーターとヘッダー {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>コードの再利用</i></td>
      <td>軽微な調整を加えたデバイス識別子とデバイス情報の計算には REST API v1 のコードを再利用しますが、送信するのは REST API v2 で想定されるパラメーターとヘッダーのみにしてください。<br/><br/>DCR API を呼び出してアクセストークンを取得するために、REST API v1 のコードを再利用します</td>
      <td>-</td>
   </tr>
</table>

### 8. テスト {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">プラクティス</th>
      <th style="background-color: #EFF2F7;">危険</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>テスト範囲</i></td>
      <td>デバイスとプラットフォームをまたいで、次の基本フローをテストします。<br/><br/><b> 認証フロー </b><ul><li>プライマリアプリケーション（画面）認証のシナリオ</li><li>セカンダリアプリケーション（画面）認証のシナリオ</li></ul><br/><b> （任意）事前認証フロー </b><ul><li>許可の決定シナリオのテスト</li><li>拒否の決定シナリオのテスト</li></ul><br/><b> 認証フロー </b><ul><li>許可の決定シナリオのテスト</li><li>拒否の決定シナリオのテスト</li></ul><br/><b> ログアウトフロー </b><br/><br/> さらに、該当する場合は、他のアクセスフローをテストします。<br/><br/><ul><li>アクセス・フローの低下（プレミアム機能）</li><li>一時的なアクセスフロー（プレミアム機能）</li><li>シングルサインオンアクセスフロー（標準機能）</li></ul><br/>上位のMVPD統合（最も広く使用されているプロバイダーをカバー）について説明します。</td>
      <td>特にテスト頻度の低いプラットフォームや、ネガティブシナリオのような稀なフローでの実稼動環境で、予期しないエラーが発生するリスクがあります。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>テストツール</i></td>
      <td><a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a> の Web サイトを使用します。</td>
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
      <td>クライアント資格情報のキャッシュ <br/><br/> アクセストークンのキャッシュ</td>
      <td>アクセストークンの検証と更新</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>設定</i></td>
      <td>設定応答の取得を最小限に抑える</td>
      <td>キャッシュ設定応答</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証</i></td>
      <td>ポーリングメカニズムの微調整 <br/><br/> プロファイルの一部をキャッシュする</td>
      <td>複数のプロファイルのサポート <br/><br/> ビジネス要件の場合はサポートの最適化機能 <br/><br/> ビジネス要件の場合はサポートの TempPass 機能 <br/><br/> ビジネス要件の場合はシングルサインオン機能のサポート</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>事前認証</i></td>
      <td>キャッシュ許可の事前認証決定 <br/><br/> 再試行メカニズムの微調整</td>
      <td>拒否された事前承認の決定に対するエラーコードを使用してユーザーエクスペリエンスを向上</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>認証</i></td>
      <td>ユーザーが再生 <br/><br/> 再試行メカニズムの微調整をリクエストした場合に、認証決定を取得します</td>
      <td>拒否された認証決定のエラーコードを使用してユーザーエクスペリエンスを向上 <br/><br/> メディアトークンの検証</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>ログアウト</i></td>
      <td>ユーザーが手動でログアウトできるように、ログアウト API を実装します。</td>
      <td>ログアウト API を自動的に呼び出さない</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">必須</th>
      <th style="background-color: #EFF2F7;">（強く）推奨</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>パラメーターとヘッダー</i></td>
      <td>必須ヘッダーの仕様に従う</td>
      <td>REST API v1 からのコードの再利用</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>エラー処理</i></td>
      <td>拡張されたエラー処理を実装 <br/><br/>HTTP エラー処理を実装</td>
      <td>-</td>
   </tr>
</table>
