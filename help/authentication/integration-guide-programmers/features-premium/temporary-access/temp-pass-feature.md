---
title: TempPass フィーチャ
description: TempPass フィーチャ
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# TempPass フィーチャ {#temp-pass-feature}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

TempPass は、有効なMVPD アカウント資格情報がなくても、プログラマーが保護されたコンテンツへの一時的なアクセスを提供できるようにする多目的な機能です。 基本的なアクセスシナリオやターゲットを絞ったプロモーションキャンペーンを通じて、視聴者を惹きつける効果的なツールとして機能します。

TempPass は、プログラマーが次のことを行うための強力なソリューションです。

* **エンゲージメント閲覧者：** 新規購読者を引き付けるプレミアムコンテンツの嗜好を提供します。
* **プロモーションの促進：** ターゲットキャンペーンを実行して、コンテンツのエクスポージャーを増やし、ブランドロイヤルティを構築します。
* **制御を保持：** アクセス期間の管理、制限の適用、アクセスのリセットを必要に応じて行い、ビジネス目標に合わせます。

TempPass 機能は、Adobe Pass Authentication Server configuration 内に疑似MVPD（さらに「Temp Pass」と呼ばれる）を導入し、参加するプログラマーとの連携によって提供されます。 TempPass 機能は、次の 2 つの構成で使用できます。

* 時間ベースのアクセス用 [&#x200B; 基本 TempPass](#basic-temp-pass)。
* [&#x200B; プロモーション TempPass](#promotional-temp-pass) で、キャンペーン駆動型の柔軟なアクセスが可能になります。

>[!IMPORTANT]
>
> TempPass 機能はプレミアム機能で、Adobeの現在のライセンスが必要です。

次の表に、基本およびプロモーションの TempPass 機能を簡単に比較します。

| **機能** | **基本 TempPass** | **プロモーション TempPass** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **コンテンツへのアクセス** | <ul><li>時間ベース</li></ul> | <ul><li>時間ベース</li><li>リソースの最大数に制限</li></ul> |
| **アクセスのセキュリティ基準** | <ul><li>デバイス ID</li></ul> | <ul><li>デバイス ID</li><li>指定されたユーザー識別情報のハッシュ （例：メール）</li></ul> |
| **拡張エラーコード** | 利用可能 | 利用可能 |
| **TempPass リセット機能** | 利用可能 | 利用可能 |

>[!IMPORTANT]
> 
> Adobe Pass認証には、割り当てられた時間（X 分）が経過すると進行中のストリームを自動的に停止する組み込みのメカニズムは含まれていません。 進行中のストリーム中に TempPass が期限切れになったら、アクセス制限を実施するのは、プログラマーの責任です。

コンテンツライブラリを見る機会を与える場合でも、マーキーイベントを宣伝する場合でも、TempPass はアクセス制御を維持しながらオーディエンスを拡大するツールを提供します。

## 基本 TempPass {#basic-temp-pass}

基本的な TempPass 機能を使用すると、プログラマーは、様々なシナリオに対応して、コンテンツへの時間制限アクセスを提供できます。

* **短いプレビュー：** 潜在的な購読者を引き付けるために、1 日 10 分のアクセス期間などの簡単なプレビューを提供します。
* **イベントベースのアクセス：** 4 時間のセッションなど、主要なイベントに対して長時間のアクセスを有効にします。
* **組み合わせアクセス：** 最初の拡張表示期間と、数日にわたる毎日の短いプレビューなど、期間の組み合わせと一致。

特定のイベントでは、コンテンツへの段階的な無料アクセスが必要となる場合があります。例えば、最初の拡張無料アクセス期間（4 時間）の後、1 日あたりの無料アクセス間隔（1 日あたり 10 分など）を短くすることが挙げられます。 このシナリオを実装するには、プログラマーはAdobe担当者と調整して、ニーズに合わせて 2 つの TempPass MVPD を設定する必要があります。

例えば、最初の 4 時間のフリーセッションと 1 日の 10 分間のフリーセッションを提供するには、Adobeでプログラマーのを設定します。

* **TempPass1**：最初の空きアクセス期間に対応するために、有効期間（TTL）を 4 時間に設定します。
* **TempPass2**：後続の 1 日の空きアクセス間隔の有効期間（TTL）が 10 分に設定されます。

毎日のアクセスに対して適切な機能を確保するには、すべてのデバイスの TempPass2 を毎日 00:00 にリセットする必要があります。

### 機能の詳細 {#basic-temp-pass-feature-details}

**設定パラメーター：**

* **TTL （Time-To-Live）:** プログラマーは、アクセス時間を指定できます。 このクロックベース TTL は、実際の表示時間に関係なく期限切れとなります。

**ユーザー ID:**

基本的な TempPass 機能では、デバイス識別子をユーザー識別パラメーターとして使用します。

次の表を参照すると、ユーザー識別パラメーターがユーザーの体験版エクスペリエンスにどのように影響するかを理解するのに役立ちます。

| デバイス識別子 | 結果 |
|-------------------|----------------|
| 新規 | 新規体験版 |
| 既存 | 既存の体験版 |

**表示時間の計算：**

TTL は、コンテンツの表示に費やされた実際の時間とは無関係に、最初の承認リクエストの時間から有効期限までの期間を表します。 今後の各リクエストでは、現在のサーバー時間と保存されている有効期限を確認して、アクセスを許可します。

**認証：**

ベーシック TempPass では認証が必要ないため、認証手順に直接進むことができます。

**認証：**

実際のMVPDとのやり取りは行われないので、基本「一時パス」MVPDは、その一時パスが有効であると指定されたリソースを認証します。 認証が成功した場合、メディアトークン検証用ライブラリは、コンテンツ再生を開始する前のメディアトークンの検証とリソースの検証に引き続き適用されます。

認証決定は、ユーザー識別パラメーターおよび設定された TTL に基づいて行われます。 リソースに対して正常な認証を取得するには、有効なリクエストが次の条件を満たす必要があります。

* **未使用の期間：** 有効期限は、（データベースに保存された）最初の認証リクエスト時間を設定済み TTL に追加することによって計算されます。 現在のサーバー時間をこの有効期限と比較して、TempPass がまだ有効かどうかを判断します。

ユーザーが設定された TTL を超えると、TempPass がリセットされない限り、同じデバイス上のコンテンツを表示できなくなります。

**事前認証：**

基本的な「一時パス」MVPDに対して事前認証リクエストが行われると、リクエストからリソースのリスト全体が正常に事前認証として返されます。 認証条件が特定のリソースではなく、時間制限に基づいていることを考慮すると、この動作は認証ロジックを反映しています。 時間制約が有効である限り、リクエストされたリソースは許可されます。

**ログアウト：**

ベーシック TempPass ではログアウトする必要がないため、実際のユーザーのMVPDを使用して、認証手順に直接切り替えることができます。

**トラッキングデータと分析：**

基本的な TempPass フローでは、トラッキングデータは、MVPD ID が「Temp Pass」に設定された、ハッシュ化されたバージョンのデバイス ID を使用します。 プログラマーは、Analytics 実装において、TempPass 指標を標準のMVPD指標と区別する必要があります。

## プロモーション TempPass {#promotional-temp-pass}

プロモーション TempPass 機能は、プロモーションキャンペーンを実行するために特別に設計された基本的な TempPass の機能を拡張します。 この機能を使用すると、メールアドレスなどの有効なユーザー ID を収集した後、指定した期間、事前定義済みの数のVOD タイトルにアクセスできるようにすることで、ユーザーを引き付けることができます。

プロモーション TempPass には、基本的な TempPass のすべての機能が含まれており、次の柔軟性が追加されています。

* プロモーション期間中にアクセスできるVOD タイトルの最大数を定義します。
* プロモーション アクセスが有効な期間を設定します。

ユーザーが定義済みのアクセス制限（VOD タイトルの数または期間）を超えると、TempPass がリセットされない限り、同じデバイス上または同じユーザー ID を持つコンテンツを表示できなくなります。

### 機能の詳細 {#promotional-temp-pass-feature-details}

**設定パラメーター：**

* **ユーザー情報キー：** メールアドレスなどの、ユーザーが指定した識別子を通信するために使用するキー（キーはメール）。
* **リソース数：** ユーザーがアクセスできるVOD タイトルの数を定義します。
* **TTL （Time-To-Live）:** ユーザーが許可されたリソースを使用できる期間です。

**ユーザー ID:**

プロモーション TempPass 機能では、デバイス識別子の上に表示されるユーザー指定の識別子のハッシュをユーザー識別パラメーターとして使用します。

>[!IMPORTANT]
>
> ID の検証とハッシュは、Adobeではなく、プログラマーによって管理されます。 Adobeには、個人を特定できる情報（PII）は格納されません。 そのため、プログラマーは、Adobe Pass Authentication API とのやり取りの際に、一意のユーザーが指定した ID のハッシュを生成して送信する責任があります。

Adobeでは、データをAdobeに送信する前に、**SHA-2** ファミリまたはその固有の **SHA-256** 関数 **SHA-512** を使用することをお勧めします。 例えば、**&quot;user@domain.com&quot;** を超える **SHA-256** は、**&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;** です。

次の表を参照すると、ユーザー識別パラメーターがユーザーの体験版エクスペリエンスにどのように影響するかを理解するのに役立ちます。

| ユーザー指定の識別子ハッシュ | デバイス識別子 | 結果 |
|-------------------------------|-------------------|---------------------------------------------------------|
| 新規 | 新規 | 新規体験版 |
| 既存 | 新規 | 既存の体験版（ユーザーが指定した識別子ハッシュに基づく） |
| 新規 | 既存 | 既存の体験版（デバイス ID に基づく） |
| 既存 | 既存 | 既存の体験版 |

**表示時間の計算：**

TTL は、コンテンツの表示に費やされた実際の時間とは無関係に、最初の承認リクエストの時間から有効期限までの期間を表します。 今後の各リクエストでは、現在のサーバー時間と保存されている有効期限を確認して、アクセスを許可します。

**認証：**

プロモーションの TempPass では認証が必要ないため、認証手順に直接進むことができます。

プログラマーのアプリケーションの実装をサポートするために、プロモーションの TempPass は、対応するキーを介してアクセスできる以下のユーザーメタデータ情報を公開します。

* **`remaining_resources`**: ユーザーがまだ使用できるリソースの数を示します。
* **`used_assets`**：ユーザーが既に消費したリソースのリストを提供します。
* **`expiration_date`**: ユーザーのプロモーション一時パスの有効期限を表示します。

**認証：**

実際のMVPDとのやり取りがないため、プロモーションの「Temp Pass」MVPDは、TempPass が有効であると指定されたリソースを認証します。 認証が成功した場合、メディアトークン検証用ライブラリは、コンテンツ再生を開始する前のメディアトークンの検証とリソースの検証に引き続き適用されます。

認証決定は、ユーザー識別パラメーター、設定されたリソース数および TTL に基づいて行われます。 リソースに対して正常な認証を取得するには、有効なリクエストが次の条件を満たす必要があります。

* **未使用の期間：** 有効期限は、（データベースに保存された）最初の認証リクエスト時間を設定済み TTL に追加することによって計算されます。 現在のサーバー時間をこの有効期限と比較して、TempPass がまだ有効かどうかを判断します。
* **未使用のリソース：** 消費されたリソースの数が追跡されます（データベースに保存されます）。 消費されたリソースの数と設定されたリソースの数を比較して、TempPass がまだ有効かどうかを判断します。

設定された TTL またはリソース数を超えるユーザーが発生した場合、TempPass がリセットされない限り、同じデバイス上または同じユーザー提供の ID を持つコンテンツを表示できなくなります。

**事前認証：**

プロモーションの「Temp Pass」MVPDに対して事前認証リクエストが行われると、リクエストからリソースのリスト全体が正常に事前認証として返されます。 承認条件が特定のリソースではなく、時間制限とアクセスしたリソースの合計数に基づいていることを考慮すると、この動作は承認ロジックを反映します。 時間制限が有効で、リソースの制限を超えていない限り、リクエストされたリソースは許可されます。

**ログアウト：**

プロモーションの TempPass にログアウトする必要はなく、実際のユーザーのMVPDを使用して認証ステップに直接切り替えることができます。

**トラッキングデータと分析：**

プロモーションの TempPass フロー中、トラッキングデータは、MVPD ID が「Temp Pass」に設定された、ハッシュ化されたバージョンのデバイス ID を使用します。 プログラマーは、Analytics 実装において、TempPass 指標を標準のMVPD指標と区別する必要があります。

## TempPass API アクセスをリセット {#reset-tempass-api-access}

Reset TempPass API にアクセスする前に、動的クライアント登録（DCR）プロセスで必要な手順を完了する必要があります。 この必須のプロセスにより、Reset TempPass API とやり取りするのに必要なアクセストークンが確保されます。

包括的な手順については、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## TempPass API のリセット - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

デバイスまたはすべてのデバイスの特定の TempPass をリセットするために、Adobe Pass認証では、基本とプロモーションの両方の TempPass で機能する API をプログラマーに提供します。

### リクエスト {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">クエリパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>オンボーディングプロセス中に TempPass に関連付けられた内部一意 ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            このリセット操作が有効なデバイス識別子。
            <br/><br/>
            値を指定しない場合、リセット操作はすべてのデバイスに適用されます。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md"> アクセストークンの取得 </a> ドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
</table>

### 応答 {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>204</td>
      <td>コンテンツなし</td>
      <td>
        リセットに成功しました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>禁止</td>
      <td>
        アクセストークンが無効です。クライアントは新しいクライアント資格情報と新しいアクセストークンを取得して、もう一度試してください。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
</table>

### サンプル {#reset-tempass-v3-reset-samples}

#### 特定のデバイスの TempPass をリセット {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### すべてのデバイスの TempPass をリセット {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## TempPass API のリセット - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

汎用キー（ユーザーが指定した ID ハッシュ）またはすべてのキーの特定の TempPass をリセットするために、Adobe Pass認証は、プロモーション TempPass で機能する API をプログラマーに提供します。

### リクエスト {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">クエリパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>オンボーディングプロセス中に TempPass に関連付けられた内部一意 ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">キー</td>
      <td>
            このリセット操作が有効なユーザー指定の識別子ハッシュ。
            <br/><br/>
            値を指定しない場合、リセット操作はすべてのユーザーに適用されます。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md"> アクセストークンの取得 </a> ドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
</table>

### 応答 {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>204</td>
      <td>コンテンツなし</td>
      <td>
        リセットに成功しました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>禁止</td>
      <td>
        アクセストークンが無効です。クライアントは新しいクライアント資格情報と新しいアクセストークンを取得して、もう一度試してください。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
</table>

### サンプル {#reset-tempass-v3-reset-generic-samples}

#### 特定のキーの TempPass をリセット {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### すべてのキーの TempPass をリセット {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

TempPass 機能を活用するには、TV Everywhere （TVE）アプリケーションとAdobe Pass認証 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) のやり取りを変更するためのコード更新を実装する必要があります。

これらのアップデートと関連ワークフローの包括的なガイドについては、[&#x200B; 一時的なアクセスフロー &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) ドキュメントを参照してください。
