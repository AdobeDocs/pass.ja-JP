---
title: プロモーション一時パス
description: プロモーション一時パス
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# プロモーション一時パス {#promotional-temp-pass}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 機能の概要 {#feature-summary}

プロモーション一時パスを使用すると、プログラマーは、MVPD のアカウント資格情報を持たないユーザーに対して、保護されたコンテンツへの一時的なアクセスを提供できます。

プロモーション一時パスは、ユーザーが有効な識別情報（電子メールアドレスなど）をプログラマーに提供した後に、 **事前に定義された期間の異なる VOD タイトルの定義済みの数**.

>[!IMPORTANT]
>
>Adobeには、個人を特定できる情報 (PII) は格納されません。 したがって、プログラマーは、Adobe Pass Authentication API で提供された一意のユーザー情報に対してハッシュを設定する必要があります。

プロモーション用の一時パスは、 [一時パス](/help/authentication/temp-pass.md) 機能には、Temp Pass のすべての機能が含まれます。

VOD タイトルの最大定義済み数または事前定義時間を超えると、Adobe Pass Authentication Server から認証トークンが消去されるまで、同じデバイス上のコンテンツや、同じユーザー識別子情報（電子メールアドレスなど）を使用してコンテンツにアクセスできなくなります。

>[!NOTE]
>
>Temp Pass は Premium ワークフローパッケージの一部です。 この機能の使用に関心がある場合は、Adobe Passの営業担当にお問い合わせください。

## 一時パスとプロモーション一時パスの比較 {#tp-ptp-comparison}

| 一時パス | プロモーション一時パス |
|----------------------------------|----------------------------------------------------------------------------------------|
| コンテンツへのアクセス <ul><li>時間ベース</li></ul> | コンテンツへのアクセス <ul><li>時間ベース</li><li>リソース数に基づく</li></ul> |
| 次に基づくアクセスセキュリティ <ul><li>デバイス ID</li></ul> | セキュリティの基準 <ul><li>デバイス ID</li><li>提供されたユーザー識別子情報（電子メールなど）のハッシュ</li></ul> |
| クライアントエラー API が使用可能 | クライアントエラー API が使用可能 |
| リセット/パージ可能 | リセット/パージ可能 |

## 機能の詳細 {#feature-details}

この機能を使用すると、ユーザーは、プログラマーのアプリケーションで電子メールアドレスなどの一意の情報を指定した後に、特定のデバイス（電話とタブレット）からプロモーションコンテンツにアクセスできます。

プログラマーは、認証および認証 API でユーザーの PII に対してハッシュを提供します。 このハッシュは、ユーザーとデバイスを識別する一意のキーを生成する際に、デバイス ID と共に使用されます。

Adobe Pass Authentication は、ユーザーが提供したデバイス ID と情報に基づいて、そのユーザーが新しい体験版か既存の体験版かを判断します。

* ユーザーが指定した情報（電子メールなど）に対する新しいハッシュ、新しいデバイス ID => 新しい体験版
* ユーザー指定の情報（電子メールなど）に対する既存のハッシュ、新しいデバイス ID => 既存の試用版 ( ユーザー指定の情報（電子メールなど）に対する既存のハッシュを使用 )
* ユーザーが指定した情報（例：電子メール）に対する新しいハッシュ、既存のデバイス ID => 既存の体験版（既存のデバイス ID を含む）
* ユーザーが指定した情報（電子メールなど）に対する既存のハッシュ、既存のデバイス ID => 既存の体験版

>[!NOTE]
>ユーザーが提供した情報の検証とハッシュは、Adobeではなく、プログラマーが処理します。

**プロモーション一時パス機能は、次のプロパティに基づいて設定できます。**

* ユーザーが指定した情報キー（例：E メール）
* ユーザーが使用する権利を持つリソースの数
* TTL — ユーザーが設定された数のリソースを消費する権利を持つ時間枠

### ユーザーメタデータ {#user-metadata}

プログラマーのアプリケーションの実装を容易にするには、次の手順を実行します。 **ユーザーメタデータ情報が公開されます** 対応するキー ( キーを有効にするには、tve-support@adobe.comにお問い合わせください ) と共に、プロモーション一時パスで次の手順を実行します。

* **remaining_resources**：現在のユーザーが使用できる残りのリソースの数
* **used_assets**：現在のユーザーが既に使用しているリソースのリスト
* **expiration_date**：現在のユーザーの有効期限

### 表示時間はどのように計算されますか？ {#compute-viewing-time}

Temp Pass が有効なままの時間は、ユーザーがプログラマーのアプリケーションでコンテンツを表示していた時間とは関係ありません。 プロモーション一時パスを介した初期ユーザーの認証要求時に、初期現在の要求時間をプログラマーが指定した TTL（期間）に追加することで、有効期限が計算されます。

### 認証と承認 {#authn-authz}

プロモーション一時パスフローの場合、認証と承認は実際の MVPD と通信しません。 **したがって、すべての認証リクエストが成功します** これらの条件がすべて満たされている限り：

* 認証トークンは、指定されたリソースに対して有効です
* 数 **used_assets** は、プログラマが設定した制限よりも低い
* **expiration_date** の値が現在の日付より後になっています。

### プリフライト動作 {#preflight-beh}

プロモーション一時パス MVPD に対してプリフライトまたは事前認証リクエストがおこなわれると、返されるプリフライト応答には、プリフライトリクエストのリソースのリスト全体が正常に含まれます。

この背後にあるロジックは、プロモーション一時パスの認証条件は、特定のリソースではなく、時間とリソース数の制限に基づいています。 具体的には、時間制限を満たし、リソースの制限を超えない限り、呼び出されたリソースが許可されます。

### SSO {#sso}

SSO は、「要求元ごとの認証」が有効に設定された、プロモーション一時パスのインスタンスに対しては有効になっていません。 つまり、ユーザーが同じプロモーション Temp Pass と統合されている別のアプリケーション B にアプリケーション A から切り替えても、ユーザーは自動的にサインインしなくなります。

### ログアウト {#logout}

デバイス上のすべてのトークンはログアウト時に削除されます。 したがって、プロモーション一時パスから通常のユーザーが選択した MVPD に切り替える場合は、この実装に依存しないでください。 この場合、 `setSelectedProvider(null)` 関数を使用して、アプリケーションの状態をクリアし、認証フローを再起動することで、ユーザーエクスペリエンスが向上します。

### プロモーション一時パスのフロー図 {#promo-tempass-flowdia}

![プロモーション一時パスのフロー図](assets/promo-temp-pass-flow.png)

*図：プロモーション一時パスフロー*

## プロモーション一時パスの実装 {#impl-promo-tempass}

プロモーション用の一時パスには、次のクライアント側機能が必要です。

* **ユーザー識別子情報（例：電子メールアドレスの伝播）** （認証および承認フローでのユーザーの電子メールアドレスの送信）。 この電子メールは、Adobe Pass Authentication で、認証および認証トークン ( `device_ID`（すべての呼び出しで必要）。
* **認証を強制**  — ユーザーが既に認証されている場合、プログラマーは認証フローを強制できます。 この機能は、ユーザーメタデータの更新（ユーザーメタデータキー）を強制するために必要です。 **used_assets** には、アプリが起動されるたびに使用できるリソースの数が含まれます。 ユーザーは複数のデバイスでログインできるので、アプリの起動時にデバイスに存在するユーザーメタデータは信頼性が低く、その特定のユーザーの現在の状態（電子メールアドレスで識別）を反映するために更新する必要があります。


>[!IMPORTANT]
>強制認証は、iOSと Android でのみ可能です。
>Adobe Pass認証には、X 分経過後にフリーストリーミングを停止する機能が組み込まれていません。 Adobe Pass認証による発行停止 **認証** および **ショートメディア** Y 個の空きリソースを消費した後のトークン。 プロモーション一時パスの有効期限が切れたら、アクセスを制限するのはプログラマー次第です。

## セキュリティ {#security}

>[!IMPORTANT]
>Adobeには、個人を特定できる情報 (PII) は格納されません。 したがって、プログラマーは、Adobe Pass Authentication API で提供された一意のユーザー情報に対してハッシュを設定する必要があります。

**ユーザー識別子情報のハッシュ化**

Adobeは、 **SHA-2** 家族またはその特定の **SHA-256**, **SHA-512** 関数は、Adobeに送信される前のデータに対して実行されます。

例： **SHA-256** over **&quot;user@domain.com&quot;** 次に該当 **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

## プロモーション一時パスをリセットまたはパージ {#reset-promo-tempass}

特定のビジネス・ルールでは、プロモーション一時パスを定期的にパージする必要があります。 そのために、Adobe Pass Authentication は、プログラマーに *公開* web API（以下で説明）。

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>プロトコル： **https**</li><li>ホスト：<ul><li>リリース： **mgmt.auth.adobe.com**</li><li>前提条件： **mgmt-prequal.auth.adobe.com**</li></ul></li><li>パス： **/reset-tempass/v2/reset**</li><li>クエリパラメーター： **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>headers: ApiKey: **1232293681726481**</li> <li>応答：<ul><li>成功： **HTTP 204**</li><li>失敗： **HTTP 400** 不正なリクエストに対して **HTTP 401** ApiKey が指定されていない場合、 **HTTP 403** apiKey が無効な場合</li></ul></li></ul> |

Temp Pass をパージするための要件に加えて、プロモーション Temp Pass は、 **generic_data** 認証およびパージの承認時に使用されます。

JSON 全体ではなく、ハッシュが送信されます。

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### サポートされるクライアント {#supported-clients}

| Adobe Pass Authentication Clients | プロモーション一時パス | ツールをリセット | 専用の応答コード/クライアントエラーをサポート |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS Access Enabler | はい | はい | はい（v 3.0.0 以降） |
| ネイティブクライアントiOS | はい | はい | はい（v 1.10 以降） |
| ネイティブクライアント Android | はい | はい | はい |
| クライアントレス API | はい | はい | いいえ |


## 制限事項 {#limitations}

この項では、プロモーション一時パスの現在の実装に適用される制限について説明します。

### クライアントなし {#lim-clientless}

**一意のデバイス ID を持たないスマートデバイス**

一意のデバイス ID を提供できないスマートデバイスアプリもあります。 1 つがない場合、Adobe Pass Authentication は、Adobe登録コードサービスで生成された UUID を一意のデバイス ID として使用できます。 つまり、ユーザーがサインアウトすると、認証および認証トークンが削除されます。 ユーザーが再度認証を試みると、今回は別のユーザー情報（E メールなど）を使用して、ユーザーが再認証できるようになります。 Adobeは、システムを「だまして」使用できない UI フローを追加し、試用版をリクエストする新しいユーザーか既存の試用版かを判断するロジックを追加することをお勧めします。

**一時パスのリセット/パージ**

Xbox 360 と Xbox One の場合、クライアントレス用の一時パスをリセットは利用できません。これらのプラットフォームでは、追加のデバイス ID 解析が必要です。一時パスのリセットツールでは使用できません。

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
