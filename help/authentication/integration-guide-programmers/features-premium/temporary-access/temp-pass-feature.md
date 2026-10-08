---
title: TempPass機能
description: TempPass機能
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '2245'
ht-degree: 0%
---
# TempPass機能 {#temp-pass-feature}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

TempPassは、プログラマーが有効なMVPDアカウント資格情報を持たないユーザーに、保護されたコンテンツへの一時的なアクセスを提供できるようにする汎用性の高い機能です。 基本的なアクセスシナリオでも、ターゲットを絞ったプロモーションキャンペーンでも、視聴者を惹きつけるための効果的なツールとして機能します。

TempPassは、プログラマーが次のことを行うための強力なソリューションです。

* **視聴者のエンゲージメント：**&#x200B;新しい加入者を引き付けるために、プレミアムコンテンツの味を提供します。
* **プロモーションを促進：** コンテンツの露出を高め、ブランドロイヤルティを構築するために、ターゲットを絞ったキャンペーンを実施します。
* **制御の維持：** ビジネス目標に合わせて、必要に応じてアクセス期間を管理し、制限を適用し、アクセスをリセットします。

TempPass機能は、Adobe Pass Authentication Server Configuration内に疑似MVPD（さらに「Temp Pass」という名前）を導入し、関連するプログラマーとの統合として提供されます。 TempPass機能は、次の2つの構成で利用できます。

* 時間ベースのアクセス用の[Basic TempPass](#basic-temp-pass)。
* [ キャンペーン駆動型の柔軟なアクセスを実現するプロモーション TempPass](#promotional-temp-pass)。

>[!IMPORTANT]
>
> TempPass機能はプレミアム機能であり、Adobeの現在のライセンスが必要です。

次の表に、基本TempPass機能とプロモーション TempPass機能の簡単な比較を示します。

| **機能** | **基本TempPass** | **プロモーション TempPass** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **コンテンツへのアクセス** | <ul><li>時間ベース</li></ul> | <ul><li>時間ベース</li><li>リソースの最大数に制限されています</li></ul> |
| **に基づくアクセス セキュリティ :** | <ul><li>デバイス ID</li></ul> | <ul><li>デバイス ID</li><li>提供されたユーザーID情報（電子メールなど）のハッシュ</li></ul> |
| **強化されたエラーコード** | 利用可能 | 利用可能 |
| **TempPass リセット機能** | 利用可能 | 利用可能 |

>[!IMPORTANT]
> 
> Adobe Pass認証には、割り当てられた時間（X分）が経過すると、進行中のストリームを自動的に停止する組み込みメカニズムは含まれていません。 進行中のストリーム中にTempPassが期限切れになると、アクセス制限を適用するのはプログラマーの責任です。

TempPassでは、コンテンツライブラリを存分に活用したい場合にも、マーキーイベントを宣伝したい場合にも、アクセス制御を維持しながらオーディエンスを拡大するためのツールを提供しています。

## 基本TempPass {#basic-temp-pass}

基本的なTempPass機能により、プログラマーは、さまざまなシナリオに対応したコンテンツへの時間制限付きアクセスを提供できます。

* **ショートプレビュー：**&#x200B;潜在的な購読者を惹きつけるために、毎日10分間のアクセス期間などの簡単なプレビューを提供します。
* **イベントベースのアクセス：** 4時間セッションなどの主要なイベントに対する長いアクセスを有効にします。
* **組み合わせアクセス：**&#x200B;最初の延長視聴期間に続いて、数日にわたって毎日のプレビューを短くするなど、ミックスとマッチの期間を設定します。

特定のイベントでは、最初の無料利用期間の延長（4時間など）、その後の毎日の無料利用期間の短縮（毎日10分など）など、コンテンツへの段階的な無料アクセスが必要になる場合があります。 このシナリオを実装するには、プログラマーはAdobeの担当者と連携して、ニーズに合わせて2つのTempPass MVPDを設定する必要があります。

例えば、最初の4時間の無料セッションの後に毎日10分の無料セッションを提供するために、Adobeはプログラマーに対して次のように設定できます。

* **TempPass1**：最初の無料アクセス期間をカバーするために、有効期間（TTL）が4時間で構成されています。
* **TempPass2**：その後の毎日の無料アクセス間隔に対して、有効期間（TTL）が10分になるように設定されています。

毎日アクセスするための適切な機能を確保するために、TempPass2は、毎日00:00時間にすべてのデバイスでリセットする必要があります。

### 機能について {#basic-temp-pass-feature-details}

**設定パラメーター：**

* **TTL （Time-To-Live）:** プログラマーはアクセス時間を指定できます。 この時計ベースのTTLは、実際の視聴時間に関係なく有効期限が切れます。

**ユーザーID:**

基本的なTempPass機能では、デバイス識別子をユーザー識別パラメーターとして使用します。

次の表は、ユーザー識別パラメーターがユーザートライアル体験にどのような影響を与えるかを理解するのに役立ちます。

| デバイス識別子 | 結果 |
|-------------------|----------------|
| 新規 | 新しい体験版 |
| 既存 | 既存の体験版 |

**表示時間の計算：**

TTLは、コンテンツを実際に表示した時間に関係なく、最初の認証リクエスト時間から有効期限までの時間を表します。 今後の各リクエストは、現在のサーバー時間を、保存された有効期限と比較して確認し、アクセスを承認します。

**認証：**

Basic TempPassでは、認証は必要ありません。認証ステップに直接進むことができます。

**認証：**

実際のMVPDとのインタラクションがないので、TempPassが有効な場合、基本の「Temp Pass」MVPDはリソースを認証します。 承認が成功した場合、メディアトークン検証ライブラリは、コンテンツ再生を開始する前に、メディアトークンを検証し、リソース検証を確保するために引き続き適用されます。

認証の決定は、ユーザー識別パラメーターと設定されたTTLに基づいて行われます。 リソースの認証を成功させるには、次の条件を有効なリクエストで満たす必要があります。

* **未使用期間：**&#x200B;有効期限は、最初の承認要求時間（データベースに保存）を設定されたTTLに追加することで計算されます。 現在のサーバー時間がこの有効期限と比較され、TempPassがまだ有効かどうかを判断します。

ユーザーが設定されたTTLを超えた場合、TempPassがリセットされない限り、同じデバイスでコンテンツを表示できなくなります。

**事前認証：**

基本的な「Temp Pass」MVPDに対して事前認証リクエストを行うと、リクエストからリクエストのリスト全体が正常に事前承認された状態で返されます。 この動作は、認証条件が特定のリソースではなく時間制限に基づいていることを考えると、認証ロジックを反映します。 時間制約が有効である限り、リクエストされたリソースは許可されます。

**ログアウト：**

Basic TempPassでは、ログアウトは必要ありません。これにより、実際のユーザーMVPDを使用して認証ステップに直接切り替えることができます。

**データと分析の追跡：**

基本的なTempPass フローでは、トラッキングデータはデバイス IDのハッシュ化されたバージョンを使用し、MVPD IDは「Temp Pass」に設定されます。 プログラマーは、アナリティクス実装において、TempPass指標をMVPDの標準的な指標と区別する必要があります。

## プロモーション TempPass {#promotional-temp-pass}

プロモーション TempPass機能は、プロモーションキャンペーンの実行に特化して設計された基本的なTempPassの機能を拡張します。 この機能を使用すると、電子メールアドレスなどの有効なユーザーIDを収集した後、指定された期間にわたって定義済みの数のVOD タイトルにアクセスできるため、ユーザーをエンゲージできます。

プロモーション TempPassには、基本的なTempPassのすべての機能が含まれており、次のような柔軟性が追加されています。

* プロモーション期間中にアクセスできるVOD タイトルの最大数を定義します。
* プロモーションアクセスが有効な期間を設定します。

ユーザーが事前定義されたアクセス制限（VODのタイトル数または期間）を超えると、TempPassがリセットされない限り、同じデバイスまたは同じユーザーIDを使用してコンテンツを表示できなくなります。

### 機能について {#promotional-temp-pass-feature-details}

**設定パラメーター：**

* **ユーザー情報キー：**&#x200B;電子メールアドレス（キーは電子メール）など、ユーザーが指定した識別子を通信するために使用するキー。
* **リソース数：** ユーザーがアクセスできるVOD タイトルの数を定義します。
* **TTL （Time-To-Live）:** ユーザーが許可されたリソースを使用できる期間。

**ユーザーID:**

プロモーション TempPass機能では、デバイス識別子の上にユーザーが指定した識別子のハッシュをユーザー識別パラメーターとして使用します。

>[!IMPORTANT]
>
> ユーザーが指定したIDの検証とハッシュ化は、Adobeではなくプログラマによって管理されます。 Adobeは、個人情報（PII）を一切保存しません。 そのため、プログラマーは、Adobe Pass Authentication APIを操作する際に、一意のユーザーが提供したIDのハッシュを生成して送信する責任があります。

Adobeでは、データがAdobeに送信される前に、**SHA-2** ファミリーまたはその特定の&#x200B;**SHA-256**、**SHA-512**&#x200B;関数を使用することをお勧めします。 例えば、**&quot;user@domain.com&quot;**&#x200B;の&#x200B;**SHA-256**&#x200B;は&#x200B;**&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**&#x200B;です。

次の表は、ユーザー識別パラメーターがユーザートライアル体験にどのような影響を与えるかを理解するのに役立ちます。

| ユーザーが指定した識別子ハッシュ | デバイス識別子 | 結果 |
|-------------------------------|-------------------|---------------------------------------------------------|
| 新規 | 新規 | 新しい体験版 |
| 既存 | 新規 | 既存の体験版（ユーザーが提供した識別子ハッシュに基づく） |
| 新規 | 既存 | 既存の体験版（デバイス識別子に基づく） |
| 既存 | 既存 | 既存の体験版 |

**表示時間の計算：**

TTLは、コンテンツを実際に表示した時間に関係なく、最初の認証リクエスト時間から有効期限までの時間を表します。 今後の各リクエストは、現在のサーバー時間を、保存された有効期限と比較して確認し、アクセスを承認します。

**認証：**

プロモーション TempPassでは、認証は必要ありません。認証ステップに直接進むことができます。

プログラマーアプリケーションの実装をサポートするために、プロモーションテンプパスは、対応するキーを介してアクセス可能な次のユーザーメタデータ情報を公開します。

* **`remaining_resources`**: ユーザーが引き続き使用できるリソースの数を示します。
* **`used_assets`**: ユーザーが既に使用しているリソースのリストを提供します。
* **`expiration_date`**: ユーザーのプロモーション一時パスの有効期限を表示します。

**認証：**

実際のMVPDとのインタラクションがないので、TempPassが有効な場合、プロモーション「Temp Pass」MVPDは任意のリソースを認証します。 承認が成功した場合、メディアトークン検証ライブラリは、コンテンツ再生を開始する前に、メディアトークンを検証し、リソース検証を確保するために引き続き適用されます。

認証の決定は、ユーザー識別パラメーター、設定されたリソース数およびTTLに基づいて行われます。 リソースの認証を成功させるには、次の条件を有効なリクエストで満たす必要があります。

* **未使用期間：**&#x200B;有効期限は、最初の承認要求時間（データベースに保存）を設定されたTTLに追加することで計算されます。 現在のサーバー時間がこの有効期限と比較され、TempPassがまだ有効かどうかを判断します。
* **未使用のリソース：**&#x200B;使用されたリソースの数が追跡されます（データベースに保存されます）。 消費されたリソースの数が、設定されたリソース数と比較され、TempPassが引き続き有効かどうかを判断します。

設定されたTTLまたはリソース数を超えるユーザーの場合、TempPassがリセットされない限り、同じデバイスまたは同じユーザーが提供した識別子でコンテンツを表示できなくなります。

**事前認証：**

プロモーション「Temp Pass」MVPDに対して事前承認リクエストが行われると、リクエストからリクエストのリスト全体が正常に事前承認された状態で返されます。 この動作は認証ロジックを反映します。認証条件は、特定のリソースではなく、時間制限とアクセスされるリソースの合計数に基づいています。 時間制約が有効で、リソース制限を超えていない限り、リクエストされたリソースは承認されます。

**ログアウト：**

プロモーションテンプパスでは、ログアウトは必要ありません。これにより、実際のユーザーMVPDを使用して認証ステップに直接切り替えることができます。

**データと分析の追跡：**

プロモーション用TempPass フローでは、トラッキングデータはデバイス IDのハッシュ化されたバージョンを使用し、MVPD IDは「Temp Pass」に設定されます。 プログラマーは、アナリティクス実装において、TempPass指標をMVPDの標準的な指標と区別する必要があります。

## TempPass API アクセスのリセット {#reset-tempass-api-access}

Reset TempPass APIにアクセスする前に、Dynamic Client Registration （DCR）プロセスで必要な手順を完了する必要があります。 この必須プロセスにより、Reset TempPass APIを操作するために必要なアクセストークンを取得できます。

包括的な手順については、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)のドキュメントを参照してください。

## TempPass APIのリセット - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

デバイスまたはすべてのデバイスに対して特定のTempPassをリセットするには、Adobe Pass Authenticationで、BasicとPromotional TempPassの両方に対応するAPIがプログラマーに提供されます。

### リクエスト {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ホスト</td>
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
      <th style="background-color: #EFF2F7;">クエリのパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部一意のID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>オンボーディングプロセス中にTempPassに関連付けられた内部一意のID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            このリセット操作が有効なデバイス ID。
            <br/><br/>
            値が指定されていない場合、リセット操作はすべてのデバイスに適用されます。
      </td>
      <td>オプション</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンのペイロードの生成については、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md"> アクセストークンの取得</a> ドキュメントを参照してください。</td>
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
        リセットが成功しました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>不正なリクエスト</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して、もう一度試す必要があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未承認</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得し、再試行する必要があります。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動的クライアント登録の概要</a>のドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>禁止</td>
      <td>
        アクセストークンが無効です。クライアントは新しいクライアント資格情報と新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動的クライアント登録の概要</a>のドキュメントを参照してください。
      </td>
   </tr>
</table>

### サンプル {#reset-tempass-v3-reset-samples}

#### 特定のデバイスのTempPassをリセット {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### すべてのデバイスのTempPassをリセット {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## TempPass APIのリセット - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

汎用キー（ユーザーが提供する識別子ハッシュ）またはすべてのキーに対して特定のTempPassをリセットするために、Adobe Pass Authenticationはプログラマーにプロモーション TempPassで動作するAPIを提供します。

### リクエスト {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ホスト</td>
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
      <th style="background-color: #EFF2F7;">クエリのパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部一意のID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>オンボーディングプロセス中にTempPassに関連付けられた内部一意のID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">キー</td>
      <td>
            このリセット操作が有効なユーザーが指定した識別子ハッシュ。
            <br/><br/>
            値が指定されていない場合、リセット操作はすべてのユーザーに適用されます。
      </td>
      <td>オプション</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンのペイロードの生成については、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md"> アクセストークンの取得</a> ドキュメントを参照してください。</td>
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
        リセットが成功しました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>不正なリクエスト</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して、もう一度試す必要があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未承認</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得し、再試行する必要があります。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動的クライアント登録の概要</a>のドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>禁止</td>
      <td>
        アクセストークンが無効です。クライアントは新しいクライアント資格情報と新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">動的クライアント登録の概要</a>のドキュメントを参照してください。
      </td>
   </tr>
</table>

### サンプル {#reset-tempass-v3-reset-generic-samples}

#### 特定のキーのTempPassのリセット {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### すべてのキーのTempPassをリセット {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

TempPass機能を利用するには、コードの更新を実装して、TV Everywhere （TVE） アプリケーションがAdobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)とどのように連携するかを変更する必要があります。

これらの更新と関連ワークフローに関する包括的なガイドについては、[一時アクセスフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)のドキュメントを参照してください。
