---
title: iOS/tvOS クックブック
description: iOS/tvOS クックブック
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '2424'
ht-degree: 0%

---

# （従来の）iOS/tvOS SDK クックブック {#iostvos-sdk-cookbook}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#intro}

このドキュメントでは、iOS/tvOS AccessEnabler ライブラリによって公開される API を使用して、プログラマーの上位レベルのアプリケーションが実装できる使用権限ワークフローについて説明します。

iOS/tvOS 用のAdobe Pass認証使用権ソリューションは、最終的に次の 2 つのドメインに分かれます。

* UI ドメイン - UI を実装する上位レベルのアプリケーションレイヤーで、AccessEnabler ライブラリが提供するサービスを使用して、制限されたコンテンツにアクセスします。

* AccessEnabler ドメイン：ここで、次の形式で使用権限ワークフローが実装されます。

   * Adobeのバックエンドサーバーに対して行われたネットワーク呼び出し
   * 認証および承認ワークフローに関連するビジネスロジックルール
   * 様々なリソースの管理とワークフロー状態の処理（トークンキャッシュなど）

AccessEnabler ドメインの目的は、使用権限ワークフローの複雑さを全て隠し、AccessEnabler ライブラリを通じて上位レイヤー・アプリケーションに使用権限ワークフローを実装するためのシンプルな使用権限プリミティブのセットを提供することです。

1. 要求者 ID の設定
1. 特定の ID プロバイダーに対する認証の確認と取得
1. 特定のリソースに対する認証の確認と取得
1. ログアウト
1. Apple VSA フレームワークをプロキシすることで、Apple SSO フローを生成します。

AccessEnabler のネットワーク・アクティビティは独自のスレッドで実行されるため、UI スレッドがブロックされることはありません。 その結果、2 つのアプリケーションドメイン間の双方向通信チャネルは、次のような完全に非同期のパターンに従う必要があります。

* UI アプリケーション レイヤは、AccessEnabler ライブラリによって公開された API 呼び出しを介して、AccessEnabler ドメインにメッセージを送信します。
* AccessEnabler は、UI レイヤーが AccessEnabler ライブラリに登録する AccessEnabler プロトコルに含まれるコールバック・メソッドを介して UI レイヤーに応答します。

## Experience Cloud ID サービスの設定（訪問者 ID） {#visitorIDSetup}

[Experience Cloud ID](https://experienceleague.adobe.com/docs/id-service/using/home.html) の値の設定は、[!DNL Analytics] の観点から重要です。 `visitorID` の値が設定されると、SDKはネットワーク呼び出しごとにこの情報を送信し、[!DNL Adobe Pass] Authentication Server がこの情報を収集します。 Adobe Pass Authentication Service の Analytics を、他のアプリケーションや Web サイトからの他の分析レポートと関連付けることができます。 visitorID の設定方法について詳しくは、[ こちら ](#setOptions) を参照してください。

## 使用権限フロー {#entitlement}

A. [ 前提条件 ](#prereqs)</br>
B. [ 起動フロー ](#startup_flow) </br>
C. [Apple SSO を使用しない認証フロー ](#authn_flow_wo_applesso) </br>
D. [iOS上のApple SSO による認証フロー ](#authn_flow_with_applesso) </br>
E. [tvOS のApple SSO での認証フロー ](#authn_flow_with_applesso_tvOS) </br>
F. [ 認証フロー ](#authz_flow) </br>
G. [ メディアフローを表示 ](#media_flow) </br>
H. [Apple SSO を使用しないログアウトフロー ](#logout_flow_wo_AppleSSO)</br>
I. [Apple SSO でのログアウトフロー ](#logout_flow_with_AppleSSO) </br>


### A.前提条件 {#prereqs}

1. コールバック関数を作成します。
   * `setRequestorComplete()` </br>
   * [setRequestor （） ](#$setReq) によってトリガーされ、成功または失敗を返します。</br>
   * 成功とは、資格コールを続行できることを示します。

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * ユーザーがプロバイダー（MVPD）を選択しておらず、まだ認証されていない場合にのみ、[`getAuthentication()`](#$getAuthN) によってトリガーされます。</br>
      * `mvpds` パラメーターは、ユーザーが使用できるプロバイダーの配列です。

   * `setAuthenticationStatus(status, errorcode)` </br>
      * 毎回 `checkAuthentication()` によってトリガーされます。</br>
      * ユーザーが既に認証済みで、プロバイダーを選択している場合にのみ、[`getAuthentication()`](#$getAuthN) によってトリガーされます。</br>
      * 返されるステータスは成功または失敗です。エラーコードは失敗のタイプを示します。

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * ユーザーがMVPDを選択した後、[`getAuthentication()`](#$getAuthN) によってトリガーされます。 `url` パラメーターは、MVPDのログインページの場所を指定します。

   * `sendTrackingData(event, data)` </br>
      * `checkAuthentication()`、[`getAuthentication()`](#$getAuthN)、`checkAuthorization()`、[`getAuthorization()`](#$getAuthZ)、`setSelectedProvider()` によってトリガーされます。
      * `event` パラメーターは、どの使用権限イベントが発生したかを示します。`data` パラメーターは、イベントに関連する値のリストです。

   * `setToken(token, resource)`

      * リソースの表示が正常に認証された後に、[checkAuthorization （） ](#checkAuthZ) および [getAuthorization （） ](#$getAuthZ) によってトリガーされます。
      * `token` パラメーターは短時間のみ有効なメディアトークンです。`resource` パラメーターは、ユーザーが表示を許可されているコンテンツです。

   * `tokenRequestFailed(resource, code, description)` </br>
      * 認証に失敗した後、[checkAuthorization （） ](#checkAuthZ) および [getAuthorization （） ](#$getAuthZ) によってトリガーされます。
      * `resource` パラメーターは、ユーザーが表示しようとしたコンテンツです。`code` パラメーターは、エラーのタイプを示すエラーコードです。`description` パラメーターは、エラーコードに関連付けられたエラーの説明です。

   * `selectedProvider(mvpd)` </br>
      * [`getSelectedProvider()`](#getSelProv) によってトリガーされます。
      * `mvpd` パラメーターは、ユーザーが選択したプロバイダーに関する情報を提供します。

   * `setMetadataStatus(metadata, key, arguments)`
      * `getMetadata().` によってトリガー
      * `metadata` パラメーターは、要求された特定のデータを提供します。`key` パラメーターは、[getMetadata （） ](#getMeta) 要求で使用されるキーで、`arguments` パラメーターは、[getMetadata （） ](#getMeta) に渡されたディクショナリと同じです。

   * [&#39;preauthorizedResources （authorizedResources）&#39;](#preauthResources)

      * [`checkPreauthorizedResources()`](#checkPreauth) によってトリガーされます。

      * `authorizedResources` パラメーターは、ユーザーを表すリソースを指定します
は閲覧を許可されています。

   * [`presentTvProviderDialog （viewController）`](#presentTvDialog)

      * 現在のリクエスターが [SSO サポートを持つMVPDで少なくともサポートしている場合に、](#getAuthN)getAuthentication （）によってトリガーされます。
      * viewController パラメーターはAppleの SSO ダイアログであり、メインビューコントローラーで表示する必要があります。

   * [&#39;dismissTvProviderDialog （viewController）&#39;](#dismissTvDialog)

      * ユーザーアクションによってトリガーされます（Apple SSO ダイアログで「キャンセル」または「その他のテレビプロバイダー」を選択します）。
      * viewController パラメーターはApple SSO ダイアログであり、メインビューコントローラーから閉じる必要があります。

![](/help//authentication/assets/iOS-flows.png)

### B.起動フロー {#startup_flow}

1. 上位レベルのアプリケーションを起動します。</br>
1. Adobe Pass Authentication </br> の開始

   a. [`init`](#$init) を呼び出して、Adobe Pass Authentication AccessEnabler のインスタンスを 1 つ作成します。
   * **依存関係：** Adobe Pass認証ネイティブ iOS/tvOS ライブラリ （AccessEnabler）

   b. `setRequestor()` を呼び出して、プログラマーの ID を確立します。プログラマーの `requestorID` と（オプションで）Adobe Pass Authentication エンドポイントの配列を渡します。 tvOS の場合は、公開鍵と秘密鍵も指定する必要があります。 詳しくは、[ クライアントレスドキュメント ](#create_dev) を参照してください。

   * **依存関係：** 有効なAdobe Pass認証要求者 ID （Adobe Pass認証アカウントで動作）
マネージャーが手配します）。

   * **トリガー:**
     [setRequestorComplete （） ](#$setReqComplete) コールバック。

   >[!NOTE]
   >
   >要求者 ID が完全に確立されるまでは、使用権限の要求を完了できません。 これは、[`setRequestor()`](#$setReq) の実行中に、後続のすべての使用権限リクエストが行われることを事実上意味します。 例えば、[`checkAuthentication()`](#checkAuthN) はブロックされます。

   2 つの実装オプションがあります。要求者の識別情報がバックエンドサーバーに送信されると、UI アプリケーションレイヤーは次の 2 つの方法のいずれかを選択できます。</br>

   1. [`setRequestorComplete()`](#setReqComplete) コールバックのトリガー（AccessEnabler デリゲートの一部）を待ちます。 このオプションを使用すると、最も確実性の高い処理 [`setRequestor()`](#$setReq) 完了するので、ほとんどの実装に対してこのオプションを使用することをお勧めします。

   1. [`setRequestorComplete()`](#setReqComplete) コールバックのトリガーを待たずに続行し、使用権限リクエストの発行を開始します。 これらの呼び出し（checkAuthentication、checkAuthorization、getAuthentication、getAuthorization、checkPreauthorizedResource、getMetadata、logout）は AccessEnabler ライブラリによってキューに入れられ、[`setRequestor()`](#$setReq) の後で実際のネットワーク呼び出しが行われます。 このオプションは、ネットワーク接続が不安定な場合などに中断されることがあります。

1. 完全な認証フローを開始せずに既存の認証を確認するには、`checkAuthentication()` を呼び出します。  この呼び出しが成功した場合は、認証フローに直接進むことができます。 そうでない場合は、認証フローに進みます。

   * **依存関係：** [setRequestor （）の呼び出しに成功しました ](#$setReq) （この依存関係は、以降のすべての呼び出しにも適用されます）。

   * **トリガー:** [setAuthenticationStatus （） ](#$setAuthNStatus) コールバック。


### C. Apple SSO を使用しない場合の認証フロー {#authn_flow_wo_applesso}

1. [`getAuthentication()`](#$getAuthN) を呼び出して認証フローを開始するか、ユーザーが既に存在することを確認します
認証済み。

   **トリガー:**

   * [setAuthenticationStatus （） ](#$setAuthNStatus) コールバック （ユーザーが既に認証されている場合）。 この場合は、[ 認証フロー ](#authz_flow) に直接進みます。

   * [displayProviderDialog （） ](#$dispProvDialog) コールバック （ユーザーがまだ認証されていない場合）

1. に送信されたプロバイダーのリストをユーザーに表示します
   [`displayProviderDialog()`](#dispProvDialog)。

1. ユーザーがプロバイダーを選択したら、`navigateToUrl:` または `navigateToUrl:useSVC:` コールバックからユーザーのMVPDの URL を取得し、`UIWebView/WKWebView` または `SFSafariViewController` コントローラーを開いて、そのコントローラーを URL に誘導します。

1. 前の手順でインスタンス化した `UIWebView/WKWebView` または `SFSafariViewController` を使用して、MVPDのログインページに移動し、ログイン資格情報を入力します。 コントローラ内では、いくつかのリダイレクト操作が行われます。</br>

>[!NOTE]
>
>この時点で、ユーザーは認証フローをキャンセルできます。 この問題が発生した場合、UI レイヤは、[ をパラメータとして ](#setSelProv)setSelectedProvider （） `null` を呼び出して、AccessEnabler にこのイベントを通知します。 これにより、AccessEnabler は内部状態をクリーンアップし、認証フローをリセットできます。

1. ユーザーが正常にログインすると、アプリケーションレイヤーが特定のカスタム URL の読み込みを検出します。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 認証フローが完了し、`UIWebView/WKWebView` または `SFSafariViewController` のコントローラーを安全に閉じることができることを示すシグナルとして、アプリケーションによって解釈される必要があります。 `SFSafariViewController` コントローラーを使用する必要がある場合、特定のカスタム URL は **`application's custom scheme`** によって定義されます（例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。そうでない場合、この特定のカスタム URL は **`ADOBEPASS_REDIRECT_URL`** 定数によって定義されます（つまり、`adobepass://ios.app`）。

1. UIWebView/WKWebView または SFSafariViewController コントローラを閉じて、AccessEnabler の `handleExternalURL:url` API メソッドを呼び出します。このメソッドは、AccessEnabler にバックエンド サーバから認証トークンを取得するように指示します。

1. （オプション） [`checkPreauthorizedResources(resources)`](#$checkPreauth) を呼び出して、ユーザーが表示を許可されているリソースを確認します。 `resources` パラメーターは、ユーザーの認証トークンに関連付けられた、保護されたリソースの配列です。 ユーザーのMVPDから取得する認証情報の 1 つの用途は、UI の装飾です（例えば、保護されたコンテンツの横にあるロック済み/ロック解除済みの記号など）。

   * **トリガー:** [`preauthorizedResources()`](#preauthResources) コールバック
   * **実行ポイント：** 完了した認証フロー後

1. 認証に成功した場合は、認証フローに進みます。

### D. iOS上のApple SSO での認証フロー {#authn_flow_with_applesso}

1. [`getAuthentication()`](#$getAuthN) を呼び出して認証フローを開始するか、ユーザーが既に認証されていることを確認します。
   **トリガー:**

   * [presentTvProviderDialog （） ](#presentTvDialog) コールバック（ユーザーが認証されておらず、現在のリクエスターに少なくとも SSO をサポートするMVPDがある場合）。 MVPD が SSO をサポートしていない場合は、クラシック認証フローが使用されます。

1. ユーザーがプロバイダを選択すると、AccessEnabler ライブラリは、Appleの VSA フレームワークから提供された情報を使用して認証トークンを取得します。

1. [setAuthenticatiosStatus （） ](#setAuthNStatus) コールバックがトリガーされます。 この時点で、ユーザーはApple SSO で認証される必要があります。

1. [ オプション ] [`checkPreauthorizedResources(resources)`](#$checkPreauth) を呼び出して、ユーザーが表示を許可されているリソースを確認します。 `resources` パラメーターは、ユーザーの認証トークンに関連付けられた、保護されたリソースの配列です。 ユーザーのMVPDから取得する認証情報の 1 つの用途は、UI を装飾することです（例えば、保護されたコンテンツの横にあるロックされた記号やロック解除された記号など）。

   * **トリガー:** [`preauthorizedResources()`](#preauthResources) コールバック
   * **実行ポイント：** 完了した認証フロー後

1. 認証に成功した場合は、認証フローに進みます。

### E. tvOS のApple SSO での認証フロー {#authn_flow_with_applesso_tvOS}

1. [`getAuthentication()`](#$getAuthN) を呼び出して、
認証フロー、またはユーザーが既に存在することを確認するために使用します
認証済み。
   **トリガー:**
   * [`presentTvProviderDialog()`](#presentTvDialog) コールバック（ユーザーが認証されておらず、現在のリクエスターに SSO をサポートするMVPDが少なくとも存在する場合）。 MVPD が SSO をサポートしていない場合は、クラシック認証フローが使用されます。

1. ユーザーがプロバイダーを選択すると、[`status()`](#status_callback_implementation) コールバックが呼び出されます。 登録コードが提供され、AccessEnabler ライブラリが 2 番目の画面の認証を正常に行うためにサーバのポーリングを開始します。

1. 指定した登録コードを使用して 2 番目の画面で正常に認証された場合は、[`setAuthenticatiosStatus()`](#setAuthNStatus) コールバックがトリガーされます。 この時点で、ユーザーはApple SSO で認証される必要があります。
1. [ オプション ] [`checkPreauthorizedResources(resources)`](#$checkPreauth) を呼び出して、ユーザーが表示を許可されているリソースを確認します。 `resources` パラメーターは、ユーザーの認証トークンに関連付けられた、保護されたリソースの配列です。 ユーザーのMVPDから取得する認証情報の 1 つの用途は、UI を装飾することです（例えば、保護されたコンテンツの横にあるロックされた記号やロック解除された記号など）。

   * **トリガー:** [`preauthorizedResources()`](#preauthResources) コールバック

   * **実行ポイント：** 完了した認証フロー後
1. 認証に成功した場合は、認証フローに進みます。

### F.認証フロー {#authz_flow}

1. [getAuthorization （） ](#$getAuthZ) を呼び出して、認証フローを開始します。

   * **依存関係：** 有効な ResourceID がMVPDと合意されました。
   * リソース ID は、他のデバイスまたはプラットフォームで使用される ID と同じである必要があり、MVPD 間でも同じになります。 リソース ID について詳しくは、[ リソース識別子 ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) を参照してください

1. 認証と承認を検証します。

   * [getAuthorization （） ](#$getAuthZ) 呼び出しが成功した場合：ユーザーに有効な AuthN および AuthZ トークンがある（ユーザーは認証され、リクエストされたメディアを監視する権限を持つ）。

   * [getAuthorization （） ](#$getAuthZ) が失敗した場合：スローされた例外を調べて、その型（AuthN、AuthZ など）を特定します。
      * 認証（AuthN）エラーの場合は、認証フローを再開します。
      * 認証（AuthZ）エラーの場合、ユーザーは要求されたメディアを監視する権限がなく、何らかのエラーメッセージがユーザーに表示されます。
      * 他のタイプのエラー（接続エラー、ネットワークエラーなど）がある場合は、適切なエラーメッセージをユーザーに表示します。

1. ショートメディアトークンを検証します。\
   Adobe Pass認証メディアトークンベリファイアライブラリを使用して、上記の [getAuthorization （） ](#$getAuthZ) 呼び出しから返された短時間のみ有効なメディアトークンを確認します。

   * 検証に成功した場合：ユーザーに要求されたメディアを再生します。
   * 検証に失敗した場合：AuthZ トークンが無効でした。メディアリクエストが拒否され、エラーメッセージがユーザーに表示されます。


1. 通常のアプリケーションフローに戻ります。

### G. メディアフローを表示 {#media_flow}

1. 表示するメディアを選択します。
1. メディアは保護されていますか？ 選択したメディアが保護されているかどうかを確認します。

   * 選択したメディアが保護されている場合、アプリケーションは上記の [ 認証フロー ](#authz_flow) を開始します。

   * 選択したメディアが保護されていない場合、メディアを再生する時間
ユーザー。

### H. Apple SSO を使用しないログアウト {#logout_flow_wo_AppleSSO}

1. [`logout()`](#$logout) を呼び出してユーザーをログアウトさせます。 AccessEnabler は、キャッシュされたすべての値とトークンをクリアします。 キャッシュをクリアした後、AccessEnabler はサーバ・コールを実行してサーバ・サイド・セッションをクリーンアップします。 サーバーコールは IdP への SAML リダイレクトを引き起こす可能性があるので（これにより、IdP 側でのセッションクリーンアップが可能になります）、このコールはすべてのリダイレクトに従う必要があります。 このため、この呼び出しは UIWebView/WKWebView または SFSafariViewController コントローラ内で処理する必要があります。

   a.認証ワークフローと同じパターンに従って、AccessEnabler ドメインは `navigateToUrl:` または `navigateToUrl:useSVC:` コールバックを介して UI アプリケーション レイヤに対して UIWebView/WKWebView または SFSafariViewController コントローラの作成を要求し、コールバックの `url` パラメータで指定された URL をロードするように指示します。 これは、バックエンドサーバー上のログアウトエンドポイントの URL です。

   b. アプリケーションは、`UIWebView/WKWebView or SFSafariViewController` コントローラのアクティビティを監視し、特定のカスタム URL を読み込んだ瞬間を検出する必要があります。これは、複数のリダイレクトを実行するためです。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 アプリケーションは、ログアウト フローが完了し、`UIWebView/WKWebView` または `SFSafariViewController` コントローラを安全に閉じられるというシグナルとしてのみ解釈する必要があります。 コントローラがこの特定のカスタム URL を読み込む場合、アプリケーションは `UIWebView/WKWebView or SFSafariViewController` コントローラを閉じて、AccessEnabler の `handleExternalURL:url`API メソッドを呼び出す必要があります。 `SFSafariViewController` コントローラーを使用する必要がある場合、特定のカスタム URL は **`application's custom scheme`** によって定義されます（例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。それ以外の場合、この特定のカスタム URL は **`ADOBEPASS_REDIRECT_URL`** 定数によって定義されます（つまり、`adobepass://ios.app`）。

   >[!NOTE]
   >
   >ログアウトフローは、ユーザーが UIWebView/WKWebView または SFSafariViewController とやり取りする必要がないという点で、認証フローとは異なります。 UI アプリケーションレイヤーは、UIWebView/WKWebView または SFSafariViewController を使用して、すべてのリダイレクトに従っていることを確認します。 そのため、ログアウト処理中にコントローラを非表示（つまり非表示）にすることが可能です（推奨）。


### I. Apple SSO でのログアウトフロー {#logout_flow_with_AppleSSO}

1. [`logout()`](#$logout) を呼び出してユーザーをログアウトさせます。
1. [status （） ](#status_callback_implementation) コールバックは、ID VSA203 で呼び出されます。
1. また、システム設定からもログインするようにユーザーに指示する必要があります。 これをおこなわないと、アプリケーションが再起動された際に再認証が行われます。



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
