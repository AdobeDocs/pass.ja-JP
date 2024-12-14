---
title: Android SDK クックブック
description: Android SDK クックブック
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1704'
ht-degree: 0%

---

# （従来の）Android SDK クックブック {#android-sdk-cookbook}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

## 概要 {#intro}

このドキュメントでは、Android AccessEnabler ライブラリによって公開される API を使用して、プログラマの上位レベルのアプリケーションが実装できる使用権限ワークフローについて説明します。


AndroidのAdobe Pass認証使用権限ソリューションは、最終的に次の 2 つのドメインに分かれます。

- UI ドメイン - UI を実装する上位レベルのアプリケーションレイヤーで、AccessEnabler ライブラリが提供するサービスを使用して、制限されたコンテンツにアクセスします。
- AccessEnabler ドメイン：ここで、次の形式で使用権限ワークフローが実装されます。
   - Adobeのバックエンドサーバーに対して行われたネットワーク呼び出し
   - 認証および承認ワークフローに関連するビジネスロジックルール
   - 様々なリソースの管理とワークフロー状態の処理（トークンキャッシュなど）

AccessEnabler ドメインの目的は、使用権限ワークフローの複雑さを全て隠し、AccessEnabler ライブラリを通じて上位レイヤー・アプリケーションに使用権限ワークフローを実装するためのシンプルな使用権限プリミティブのセットを提供することです。

1. 要求者 ID を設定します。

1. 特定の ID プロバイダーに対する認証を確認し、取得します。

1. 特定のリソースの認証を確認および取得します。

1. ログアウト。

AccessEnabler のネットワーク アクティビティは別のスレッドで実行されるため、UI スレッドがブロックされることはありません。 その結果、2 つのアプリケーションドメイン間の双方向通信チャネルは、次のような完全に非同期のパターンに従う必要があります。

- UI アプリケーション レイヤは、AccessEnabler ライブラリによって公開された API 呼び出しを介して、AccessEnabler ドメインにメッセージを送信します。
- AccessEnabler は、UI レイヤーが AccessEnabler ライブラリに登録する AccessEnabler プロトコルに含まれるコールバック・メソッドを介して UI レイヤーに応答します。

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

     `setRequestor()` によってトリガーされ、成功または失敗を返します。\
     成功とは、資格コールを続行できることを示します。

   - [displayProviderDialog （mvpds）](#$displayProviderDialog)

     ユーザーがプロバイダー（MVPD）を選択しておらず、まだ認証されていない場合にのみ、`getAuthentication()` によってトリガーされます。\
     `mvpds` パラメーターは、ユーザーが使用できるプロバイダーの配列です。

   - [&#39;setAuthenticationStatus （status, errorcode）&#39;](#$setAuthNStatus)

     毎回 `checkAuthentication()` によってトリガーされます。\
     ユーザーが既に認証済みで、プロバイダーを選択している場合にのみ、`getAuthentication()` によってトリガーされます。

     返されるステータスは成功または失敗です。エラーコードは失敗のタイプを示します。

   - [navigateToUrl （url）](#$navigateToUrl)

     ユーザーがMVPDを選択した後、`getAuthentication()` によってトリガーされます。 `url` パラメーターは、MVPDのログインページの場所を指定します。

   - [&#39;sendTrackingData （event, data）&#39;](#$sendTrackingData)

     `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()` によってトリガーされます。\
     `event` パラメーターは、どの使用権限イベントが発生したかを示します。`data` パラメーターは、イベントに関連する値のリストです。

   - [&#39;setToken （token, resource）&#39;](#$setToken)

     リソースを表示する認証が成功した後、`checkAuthorization()` および `getAuthorization()` によってトリガーされます。\
     `token` パラメーターは短時間のみ有効なメディアトークンです。`resource` パラメーターは、ユーザーが表示を許可されているコンテンツです。

   - [&#39;tokenRequestFailed （resource, code, description）&#39;](#$tokenRequestFailed)

     認証に失敗した後、`checkAuthorization()` および `getAuthorization()` によってトリガーされます。\
     `resource` パラメーターは、ユーザーが表示しようとしたコンテンツです。`code` パラメーターは、エラーのタイプを示すエラーコードです。`description` パラメーターは、エラーコードに関連付けられたエラーの説明です。

   - [&#39;selectedProvider （mvpd）&#39;](#$selectedProvider)

     `getSelectedProvider()` によってトリガーされます。\
     `mvpd` パラメーターは、ユーザーが選択したプロバイダーに関する情報を提供します。

   - [&#39;setMetadataStatus （metadata, key, arguments）&#39;](#$setMetadataStatus)

     `getMetadata().` によってトリガー\
     `metadata` パラメーターは、要求された特定のデータを提供します。`key` パラメーターは、`getMetadata()` 要求で使用されるキーで、`arguments` パラメーターは、`getMetadata()` に渡された辞書と同じです。

   - [&#39;preauthorizedResources （resources）&#39;](#$preauthResources)

     `checkPreauthorizedResources()` によってトリガーされます。\
     `authorizedResources` パラメーターは、ユーザーが表示を許可されているリソースを表します。


![](../../../../assets/android-entitlement-flows.png)


### B.起動フロー {#startup_flow}

1. 上位レベルのアプリケーションを起動します。
1. Adobe Pass認証の開始

   a. [`getInstance`](#$getInstance) を呼び出して、Adobe Pass Authentication AccessEnabler のインスタンスを 1 つ作成します。

   - **依存関係：** Adobe Pass認証ネイティブ
Androidライブラリ（AccessEnabler）

   b. Call` setRequestor()` プログラマーの ID を確立します。プログラマーの `requestorID` と（オプションで）Adobe Pass Authentication エンドポイントの配列を渡します。

   - **依存関係：** 有効なAdobe Pass認証要求者 ID\
     （Adobe Pass認証アカウントマネージャーと協力してこれを手配します）。

   - **トリガー:** setRequestorComplete （） コールバック

   | メモ |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | 要求者 ID が完全に確立されるまでは、使用権限の要求を完了できません。 つまり、setRequestor （）の実行中は、それ以降のエンタイトルメントリリクエスト（`checkAuthentication()` など）がすべてブロックされます。<br><br>2 つの実装オプションがあります。要求者の識別情報がバックエンドサーバーに送信されると、UI アプリケーションレイヤーは次の 2 つの方法のいずれかを選択できます。<br><br>1。  `setRequestorComplete()` コールバックのトリガー（AccessEnabler デリゲートの一部）を待ちます。  このオプションを使用すると、最も確実性の高い処理 `setRequestor()` 完了するので、ほとんどの実装に対してこのオプションを使用することをお勧めします。<br>2。  `setRequestorComplete()` コールバックのトリガーを待たずに続行し、使用権限リクエストの発行を開始します。 これらの呼び出し（checkAuthentication、checkAuthorization、getAuthentication、getAuthorization、checkPreauthorizedResource、getMetadata、logout）は AccessEnabler ライブラリによってキューに入れられ、後で実際のネットワーク呼び出しが行われます `setRequestor(). ` このオプションは、ネットワーク接続が不安定な場合など、中断されることがあります。 |

1. [checkAuthentication （） ](#$checkAuthN) を呼び出すと、完全な認証フローを開始せずに既存の認証を確認できます。   この呼び出しが成功した場合は、認証フローに直接進むことができます。  そうでない場合は、認証フローに進みます。

   - **依存関係：** `setRequestor()` の呼び出しが成功した（この依存関係は、後続のすべての呼び出しにも適用されます）。

   - **トリガー:** setAuthenticationStatus （） コールバック

### C.認証フロー {#authn_flow}

1. [`getAuthentication()`](#$getAuthN) を呼び出して認証フローを開始するか、ユーザーが既に認証されていることを確認します。\
   **トリガー:**
   - ユーザーが既に認証されている場合は、setAuthenticationStatus （） コールバック。  この場合は、[ 認証フロー ](#authz_flow) に直接進みます。
   - ユーザーがまだ認証されていない場合の displayProviderDialog （） コールバック。

1. `displayProviderDialog()` に送信されたプロバイダーのリストをユーザーに提示します。

1. ユーザーがプロバイダーを選択したら、`navigateToUrl()` コールバックからユーザーのMVPDの URL を取得します。  WebView を開き、その WebView コントロールを URL に誘導します。

1. 前の手順でインスタンス化した WebView を使用して、MVPDのログインページに移動し、ログイン資格情報を入力します。 WebView 内では、いくつかのリダイレクト操作が行われます。

   **メモ：** この時点で、ユーザーは認証フローをキャンセルできます。 この場合、UI レイヤは、`null` をパラメータとして `setSelectedProvider()` を呼び出すことにより、AccessEnabler にこのイベントを通知します。 これにより、AccessEnabler は内部状態をクリーンアップし、認証フローをリセットできます。

1. ユーザーが正常にログインすると、アプリケーションレイヤーは「カスタムリダイレクト URL」の読み込みを検出します（例：`http://adobepass.android.app`）。 このカスタム URL は、実際には無効な URL であり、WebView での読み込みを目的としたものではありません。 これは、認証フローが完了し、WebView を閉じる必要があるシグナルです。

1. WebView コントロールを閉じて `getAuthenticationToken()` を呼び出します。これにより、AccessEnabler はバックエンド サーバから認証トークンを取得します。

1. [ オプション ] [`checkPreauthorizedResources(resources)`](#$checkPreauth) を呼び出して、ユーザーが表示を許可されているリソースを確認します。 `resources` パラメーターは、ユーザーの認証トークンに関連付けられた、保護されたリソースの配列です。\
   **トリガー:** `preAuthorizedResources()` コールバック\
   **実行ポイント：** 完了した認証フロー後

1. 認証に成功した場合は、認証フローに進みます。



### D.認証フロー {#authz_flow}

1. [getAuthorization （） ](#$getAuthZ) を呼び出して認証を開始します
フロー。

   依存関係：有効な ResourceID がMVPDと合意されました。

   **メモ：** ResourceID は、他のデバイスやプラットフォームで使用されるものと同じである必要があり、MVPD 間でも同じになります。

1. 認証と承認を検証します。

   - `getAuthorization()` 呼び出しが成功した場合：有効な AuthN および AuthZ トークンがある（ユーザーは認証され、リクエストされたメディアを監視する権限を持ちます）。
   - `getAuthorization()` が失敗した場合：スローされた例外を調べて、そのタイプ（AuthN、AuthZ など）を特定します。
      - 認証（AuthN）エラーの場合は、認証フローを再開します。
      - 認証（AuthZ）エラーの場合、ユーザーは要求されたメディアを監視する権限がなく、何らかのエラーメッセージがユーザーに表示されます。
      - 他のタイプのエラー（接続エラー、ネットワークエラーなど）がある場合は、適切なエラーメッセージをユーザーに表示します。

1. ショートメディアトークンを検証します。\
   Adobe Pass認証メディアトークンベリファイライブラリを使用して、上記の `getAuthorization()` 呼び出しから返された短時間のみ有効なメディアトークンを検証します。

   - 検証に成功した場合：ユーザーに要求されたメディアを再生します。
   - 検証に失敗した場合：AuthZ トークンが無効でした。メディアリクエストが拒否され、エラーメッセージがユーザーに表示されます。

1. 通常のアプリケーションフローに戻ります。

### E. メディアフローを表示 {#media_flow}

1. 表示するメディアを選択します。
2. メディアは保護されていますか？  選択したメディアが保護されているかどうかを確認します。
- 選択したメディアが保護されている場合、アプリケーションは上記の [ 認証フロー ](#authz_flow) を開始します。
- 選択したメディアが保護されていない場合は、そのユーザーのメディアを再生します。



### F. ログアウトフロー {#logout_flow}

1. [`logout()`](#$logout) を呼び出してユーザーをログアウトさせます。\
   AccessEnabler は、現在のリクエスタおよびシングル サインオンを使用するリクエスタに対して、現在のMVPDのすべてのキャッシュされた値とトークンをクリアします。 キャッシュをクリアした後、AccessEnabler はサーバ・コールを実行してサーバ・サイド・セッションをクリーンアップします。  サーバーコールは IdP への SAML リダイレクトを引き起こす可能性があるので（これにより、IdP 側でのセッションクリーンアップが可能になります）、このコールはすべてのリダイレクトに従う必要があります。 このため、この呼び出しは WebView コントロール内で処理する必要があります。

   a.認証ワークフローと同じパターンに従って、AccessEnabler ドメインが（`navigateToUrl()` コールバックを介して） UI アプリケーション レイヤにリクエストを送信し、WebView コントロールを作成して、そのコントロールにバックエンド サーバ上のログアウト エンドポイントの URL をロードするように指示します。

   b.もう一度、UI で WebView コントロールのアクティビティを監視し、コントロールが、複数のリダイレクトを経てアプリケーションのカスタム URL を読み込む瞬間を検出する必要があります（例：`http://adobepass.android.app/`）。 このイベントが発生すると、UI アプリケーションレイヤーが WebView を閉じて、ログアウトプロセスが完了します。

   **注意：** ログアウトフローは、ユーザーが WebView とやり取りする必要がないという点で、認証フローとは異なります。 UI アプリケーションレイヤーは、WebView を使用して、すべてのリダイレクトに従っていることを確認します。 したがって、ログアウトプロセス中に WebView コントロールを非表示（つまり非表示）にすることが可能（および推奨）です。



### 複数の MVPD とログアウトを使用したログインのユーザーフロー {#user_flows}

[ ここでは ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) 複数の MVPD を使用した場合の動作と、ユーザーがアプリケーションからログアウトした場合の動作を説明するドキュメントがあります。

説明されている動作は、Android SDK バージョン >= 2.0.0 を使用する場合に利用できます。
