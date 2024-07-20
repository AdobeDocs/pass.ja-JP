---
title: Amazon FireOS 統合クックブック
description: Amazon FireOS 統合クックブック
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1424'
ht-degree: 0%

---

# Amazon FireOS 統合クックブック {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>


## 概要 {#intro}

このドキュメントでは、Amazon FireOS `AccessEnabler` ライブラリで公開されている API を使用して、プログラマーの上位レベルのアプリケーションが実装できる使用権限ワークフローについて説明します。

Amazon FireOS 用のAdobe Pass認証使用権ソリューションは、最終的に次の 2 つのドメインに分かれます。

- UI ドメイン - UI を実装する上位レベルのアプリケーションレイヤーで、`AccessEnabler` ライブラリが提供するサービスを使用して、制限されたコンテンツにアクセスします。
- `AccessEnabler` ドメイン – ここで、次の形式で使用権限ワークフローが実装されます。
   - Adobeのバックエンドサーバーに対して行われたネットワーク呼び出し
   - 認証および承認ワークフローに関連するビジネスロジックルール
   - 様々なリソースの管理とワークフロー状態の処理（トークンキャッシュなど）

`AccessEnabler` ドメインの目的は、エンタイトルメントワークフローの複雑さをすべて非表示にし、上位層アプリケーションに（`AccessEnabler` ライブラリを通じて）シンプルなエンタイトルメントプリミティブのセットを提供することです。 このプロセスでは、次の権利付与ワークフローを実装できます。

1. 要求者 ID を設定します。
1. 特定の ID プロバイダーに対する認証を確認し、取得します。
1. 特定のリソースの認証を確認および取得します。
1. ログアウト。

`AccessEnabler` のネットワークアクティビティは別のスレッドで行われるので、UI スレッドはブロックされません。 その結果、2 つのアプリケーションドメイン間の双方向通信チャネルは、次のような完全に非同期のパターンに従う必要があります。

- UI アプリケーションレイヤーは、`AccessEnabler` ライブラリによって公開される API 呼び出しを介して、`AccessEnabler` ドメインにメッセージを送信します。
- `AccessEnabler` は、UI レイヤーが `AccessEnabler` ライブラリに登録する `AccessEnabler` プロトコルに含まれるコールバックメソッドを介して UI レイヤーに応答します。

## 使用権限フロー {#entitlement}

1. [前提条件](#prereqs)
1. [起動フロー](#startup_flow)
1. [認証フロー](#authn_flow)
1. [認証フロー](#authz_flow)
1. [メディアフローを表示](#media_flow)
1. [ログアウトフロー](#logout_flow)

### A.前提条件 {#prereqs}

1. コールバック関数を作成します。
   - [`setRequestorComplete （）`](#$setRequestorComplete)

      - `setRequestor()` によってトリガーされ、成功または失敗を返します。     成功とは、資格コールを続行できることを示します。

   - [displayProviderDialog （mvpds）](#$displayProviderDialog)

      - ユーザーがプロバイダー（MVPD）を選択しておらず、まだ認証されていない場合にのみ、`getAuthentication()` によってトリガーされます。 `mvpds` パラメーターは、ユーザーが使用できるプロバイダーの配列です。

   - [&#39;setAuthenticationStatus （status, reason）&#39;](#$setAuthNStatus)

      - 毎回 `checkAuthentication()` によってトリガーされます。 ユーザーが既に認証済みで、プロバイダーを選択している場合にのみ、`getAuthentication()` によってトリガーされます。

      - 返されるステータスが認証済みまたは未認証です。この理由は、認証エラーまたはログアウトアクションを示します。

   - [navigateToUrl （url）](#$navigateToUrl)

      - AmazonFireOS SDK では無視されます。このメソッドは、ユーザーが MVPD を選択した後、が `getAuthentication()` によってトリガーされるAndroid プラットフォームで使用されます。  `url` パラメーターは、MVPD のログインページの場所を指定します。

   - [&#39;sendTrackingData （event, data）&#39;](#$sendTrackingData)

      - `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()` によってトリガーされます。
`event` パラメーターは、発生した使用権限イベントを示します。`data` パラメーターは、イベントに関連する値のリストです。

   - [&#39;setToken （token, resource）&#39;](#$setToken)

      - リソースを表示する認証が成功した後、`checkAuthorization()` および `getAuthorization()` によってトリガーされます。
      - `token` パラメーターは短時間のみ有効なメディアトークンです。`resource` パラメーターは、ユーザーが表示を許可されているコンテンツです。

   - [&#39;tokenRequestFailed （resource, code, description）&#39;](#$tokenRequestFailed)

      - 認証に失敗した後、`checkAuthorization()` および `getAuthorization()` によってトリガーされます。
      - `resource` パラメーターは、ユーザーが表示しようとしたコンテンツです。`code` パラメーターは、エラーのタイプを示すエラーコードです。`description` パラメーターは、エラーコードに関連付けられたエラーの説明です。

   - [&#39;selectedProvider （mvpd）&#39;](#$selectedProvider)

      - `getSelectedProvider()` によってトリガーされます。
      - `mvpd` パラメーターは、ユーザーが選択したプロバイダーに関する情報を提供します。

   - [&#39;setMetadataStatus （metadata, key, arguments）&#39;](#$setMetadataStatus)

      - `getMetadata().` によってトリガー
      - `metadata` パラメーターは、要求された特定のデータを提供します。`key` パラメーターは、`getMetadata()` 要求で使用されるキーで、`arguments` パラメーターは、`getMetadata()` に渡された辞書と同じです。

   - [&#39;preauthorizedResources （resources）&#39;](#$preauthResources)

      - `checkPreauthorizedResources()` によってトリガーされます。
      - `authorizedResources` パラメーターは、ユーザーが表示を許可されているリソースを表します。


![](assets/android-entitlement-flows.png)


### B.起動フロー {#startup_flow}

1. 上位レベルのアプリケーションを起動します。
1. Adobe Pass認証を開始します。

   1. [`getInstance`](#$getInstance) を呼び出して、Adobe Pass Authentication `AccessEnabler` のインスタンスを 1 つ作成します。

      - **依存関係：** Adobe Pass認証ネイティブ Amazon FireOS ライブラリ （`AccessEnabler`）

   1. ` setRequestor()` を呼び出して、プログラマーの ID を確立します。プログラマーの `requestorID` を渡し、（オプションで）Adobe Pass Authentication エンドポイントの配列を渡します。

      - **依存関係：** 有効なAdobe Pass認証要求者 ID （Adobe Pass認証アカウントマネージャーと協力して手配してください）

      - **トリガー:** setRequestorComplete （） コールバック

   要求者 ID が完全に確立されるまでは、使用権限の要求を完了できません。 つまり、setRequestor （）の実行中は、それ以降のエンタイトルメントリリクエスト（`checkAuthentication()` など）がすべてブロックされます。

   2 つの実装オプションがあります。要求者の識別情報がバックエンドサーバーに送信されると、UI アプリケーションレイヤーは次の 2 つの方法のいずれかを選択できます。</p>

   1. `setRequestorComplete()` コールバックのトリガー（`AccessEnabler` デリゲートの一部）を待ちます。  このオプションを使用すると、最も確実性の高い処理 `setRequestor()` 完了するので、ほとんどの実装に対してこのオプションを使用することをお勧めします。
   1. `setRequestorComplete()` コールバックのトリガーを待たずに続行し、使用権限リクエストの発行を開始します。 これらの呼び出し（checkAuthentication、checkAuthorization、getAuthentication、getAuthorization、checkPreauthorizedResource、getMetadata、logout）は `AccessEnabler` ライブラリによってキューに入れられ、`setRequestor()` の後に実際のネットワーク呼び出しを行います。 ネットワーク接続が不安定な場合など、このオプションが中断される場合があります。

1. [checkAuthentication （） ](#$checkAuthN) を呼び出すと、完全な認証フローを開始せずに既存の認証を確認できます。  この呼び出しが成功した場合は、認証フローに直接進むことができます。  そうでない場合は、認証フローに進みます。

- **依存関係：** `setRequestor()` の呼び出しが成功した（この依存関係は、後続のすべての呼び出しにも適用されます）。

- **トリガー:** setAuthenticationStatus （） コールバック

### C.認証フロー {#authn_flow}

1. [`getAuthentication()`](#$getAuthN) を呼び出して認証フローを開始するか、ユーザーが既に認証されていることを確認します。

   **トリガー:**

   - ユーザーが既に認証されている場合は、setAuthenticationStatus （） コールバック。  この場合は、[ 認証フロー ](#authz_flow) に直接進みます。
   - ユーザーがまだ認証されていない場合の displayProviderDialog （） コールバック。

1. `displayProviderDialog()` に送信されたプロバイダーのリストをユーザーに提示します。
1. ユーザーがプロバイダーを選択すると、WebView はユーザーがログインするためのプロバイダーページを開きます

   >[!NOTE]
   >
   >この時点で、ユーザーは認証フローをキャンセルできます。 この場合、`AccessEnabler` は内部状態をクリーンアップし、認証フローをリセットします。

1. ユーザーが正常にログインすると、WebView が閉じます。
1. バックエンドサーバーから認証トークンを取得するように `AccessEnabler` に指示する `getAuthenticationToken(),` を呼び出します。
1. [ オプション ] [`checkPreauthorizedResources(resources)`](#$checkPreauth) を呼び出して、ユーザーが表示を許可されているリソースを確認します。 `resources` パラメーターは、ユーザーの認証トークンに関連付けられた、保護されたリソースの配列です。

   **トリガー:** `preAuthorizedResources()` コールバック\
   **実行ポイント：** 完了した認証フロー後

1. 認証に成功した場合は、認証フローに進みます。


### D.認証フロー {#authz_flow}

1. [`getAuthorization()`](#$getAuthZ) を呼び出して、認証フローを開始します。

   依存関係：有効なリソース ID が MVPD と合意されました。

   **メモ：** ResourceID は、他のデバイスやプラットフォームで使用されるものと同じである必要があり、MVPD 間でも同じになります。

1. 認証と承認を検証します。

   - `getAuthorization()` 呼び出しが成功した場合：有効な AuthN および AuthZ トークンがある（ユーザーは認証され、リクエストされたメディアを監視する権限を持ちます）。
   - `getAuthorization()` が失敗した場合：スローされた例外を調べて、そのタイプ（AuthN、AuthZ など）を特定します。
      - 認証（AuthN）エラーの場合は、認証フローを再開します。
      - 認証（AuthZ）エラーの場合、ユーザーは要求されたメディアを監視する権限がなく、何らかのエラーメッセージがユーザーに表示されます。
      - その他のタイプのエラー（接続エラー、ネットワークエラーなど）が発生した場合 次に、適切なエラーメッセージをユーザーに表示します。

1. ショートメディアトークンを検証します。

   上記の `getAuthorization()` 呼び出しから返された短時間のみ有効なメディアトークンを検証するには、Adobe Pass認証メディアトークンベリファイライブラリを使用します。

   - 検証に成功した場合：ユーザーに要求されたメディアを再生します。
   - 検証に失敗した場合：AuthZ トークンが無効でした。メディアリクエストが拒否され、エラーメッセージがユーザーに表示されます。

1. 通常のアプリケーションフローに戻ります。

### E. メディアフローを表示 {#media_flow}

1. 表示するメディアを選択します。
1. メディアは保護されていますか？  選択したメディアが保護されているかどうかを確認します。
   - 選択したメディアが保護されている場合、アプリケーションは上記の [ 認証フロー ](#authz_flow) を開始します。
   - 選択したメディアが保護されていない場合は、そのユーザーのメディアを再生します。

### F. ログアウトフロー {#logout_flow}

1. [`logout()`](#$logout) を呼び出してユーザーをログアウトさせます。 `AccessEnabler` は、シングルサインオン経由でログインを共有するすべてのリクエスターで、現在の MVPD に対してユーザーが取得した、キャッシュされたすべての値とトークンをクリアします。 キャッシュをクリアした後、`AccessEnabler` はサーバーコールを実行して、サーバーサイドのセッションをクリーンアップします。  サーバーコールは IdP への SAML リダイレクトを引き起こす可能性があるので（これにより、IdP 側でのセッションクリーンアップが可能になります）、このコールはすべてのリダイレクトに従う必要があります。 このため、この呼び出しは WebView コントロール内で処理され、ユーザーには表示されません。

   **注意：** ログアウトフローは、ユーザーが WebView とやり取りする必要がないという点で、認証フローとは異なります。 そのため、ログアウトプロセス中に WebView コントロールを非表示（つまり、非表示）にすることが（推奨されます）可能です。
