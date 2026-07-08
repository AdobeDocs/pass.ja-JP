---
title: iOS/tvOS API リファレンス
description: iOS/tvOS API リファレンス
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: b6ba687240799d1889302019613f426259f147ad
workflow-type: tm+mt
source-wordcount: '7035'
ht-degree: 0%

---

# （レガシー） iOS/tvOS SDK API リファレンス {#iostvos-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#intro}

ここでは、Adobe Pass Authentication用のiOS/tvOS ネイティブクライアントで公開されるメソッドとコールバック関数について説明します。 ここで説明するメソッドとコールバック関数は、`AccessEnabler.h`および`EntitlementDelegate.h` ヘッダーファイルで定義されています。これらの関数は、iOS AccessEnabler SDKの`[SDK directory]/AccessEnabler/headers/api/`にあります。


関連ドキュメント：

* Adobe Passの実装方法をステップバイステップで学びましょう
このAPIを使用した認証権限フローについては、[iOS統合クックブック ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)を参照してください。
* 最新のiOS AccessEnabler SDKについては、[iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)を参照してください。

>[!NOTE]
>
>Adobeでは、Adobe Pass Authentication *public* APIのみを使用することをお勧めします。
>
>* パブリック APIは利用可能で、サポートされているすべてのクライアントタイプで完全にテストされています。 公開されている機能については、各クライアントタイプに対応するバージョンの関連メソッドが含まれていることを確認します。
>* 下位互換性をサポートし、パートナー統合が壊れないようにするために、パブリック APIは可能な限り安定している必要があります。 ただし、非公開APIについては、将来の時点で署名を変更する権利を留保します。 現在のパブリック Adobe Pass Authentication API呼び出しの組み合わせでサポートできない特定のフローが発生した場合、最善のアプローチは私たちに知らせることです。 お客様のニーズを考慮して、公開APIを変更し、安定したソリューションを提供することができます。

</br>

## API リファレンス {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - AccessEnabler オブジェクトをインスタンス化します。

* **[非推奨]** [init](#init) - AccessEnabler オブジェクトをインスタンス化します。

* [`setOptions:options:`](#setOptions) - プロファイルやvisitorIDなどのグローバル SDK オプションを設定します。

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - プログラマーのIDを確立します。

* **[非推奨]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - プログラマーのIDを確立します。

* **[非推奨]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos)、[`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos) – プログラマーのIDを確立します。

* [`setRequestorComplete:`](#setReqComplete) – 設定フェーズが完了したことをアプリケーションに通知します。

* [`checkAuthentication`](#checkAuthN) – 現在のユーザーの認証ステータスを確認します。

* [`getAuthentication`](#getAuthN)、[`getAuthentication:withData:`](#getAuthN) – 完全な認証ワークフローを開始します。

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) – 完全な認証ワークフローを開始します。

* [`displayProviderDialog:`](#dispProvDialog) - ユーザーがMVPDを選択するための適切なUI要素をインスタンス化するようにアプリケーションに通知します。

* [`setSelectedProvider:`](#setSelProv) - ユーザーのMVPDの選択内容をAccessEnablerに通知します。

* [`navigateToUrl:`](#nav2url) - MVPD ログインページを表示する必要があることをアプリケーションに通知します。

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - SFSafariViewControllerを使用して、MVPD ログインページを表示する必要があることをアプリケーションに通知します

* [`handleExternalURL:url`](#handleExternalURL) – 認証/ログアウトフローを完了します。

* **[非推奨]** [`getAuthenticationToken`](#getAuthNToken) - バックエンドサーバーから認証トークンを要求します。

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) – 認証フローのステータスをアプリケーションに通知します。

* [`checkPreauthorizedResources:`](#checkPreauth) - ユーザーが既に特定の保護されたリソースを表示する権限を持っているかどうかを判断します。

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - ユーザーが既に特定の保護されたリソースを表示する権限を持っているかどうかを判断します。

* [`preauthorizedResources:`](#preauthResources) - ユーザーが既に表示を許可されているリソースのリストを提供します。

* [`checkAuthorization:`](#checkAuthZ)、[`checkAuthorization:withData:`](#checkAuthZ) – 現在のユーザーの認証ステータスを確認します。

* [`getAuthorization:`](#getAuthZ)、[`getAuthorization:withData:`](#getAuthZ) – 認証フローを開始します。

* [`setToken:forResource:`](#setToken) – 認証フローが正常に完了したことをアプリケーションに通知します。

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) – 認証フローが失敗したことをアプリケーションに通知します。

* [`logout`](#logout) - ログアウト フローを開始します。

* [`getSelectedProvider`](#getSelProv) – 現在選択されているプロバイダーを決定します。

* [`selectedProvider:`](#selProv) – 現在選択されているMVPDに関する情報をアプリケーションに配信します。

* [`getMetadata:`](#getMeta) - AccessEnabler ライブラリによってメタデータとして公開された情報を取得します。

* [`presentTvProviderDialog:`](#presentTvDialog) - Apple SSO ダイアログを表示するようにアプリケーションに通知します。

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Apple SSO ダイアログを非表示にするようにアプリケーションに通知します。

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - [`getMetadata:`](#getMeta)呼び出しで要求されたメタデータを配信します。

* [`sendTrackingData:forEventType:`](#sendTracking) – 追跡データ情報を配信します。

* [`MVPD`](#mvpd) - MVPD クラス。 [MVPDに関する情報が含まれています]

### init:softwareStatement {#initWithSoftwareStatement}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに1つのAccessEnabler インスタンスが必要です。

| **API呼び出し：iOS AccessEnabler コンストラクター** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**可用性：** v3.0以降

**パラメーター：**

* **softwareStatement:** Adobe システム内のアプリケーションを識別する文字列。 ソフトウェアステートメントの取得方法を確認してください。

[トップへ戻る…](#apis)



### init - [非推奨]{#init}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに1つのAccessEnabler インスタンスが必要です。

| API呼び出し：iOS AccessEnabler コンストラクター |
| --- |
| `- (id) init;` |

**可用性：** v1.0+ **まで：** v3.0

**パラメーター：**&#x200B;なし

[トップへ戻る…](#apis)

</br>

### setOptions:options {#setOptions}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** グローバル SDK オプションを設定します。 引数としてNSDictionaryを受け付けます。 ディクショナリの値は、SDKが行うすべてのネットワーク呼び出しと共にサーバーに渡されます。

**注意：**&#x200B;値は、現在のフロー（認証/承認）に関係なく、サーバーに渡されます。 値を変更する場合は、任意の時点でこのメソッドを呼び出すことができます。

| API呼び出し：setOptions |
| --- |
| `- (void) setOptions:(NSDictionary *)options;` |

**可用性：** v2.3.0以降

**パラメーター：**

* *options*：グローバル SDK オプションを含むNSDictionary。 現在、次のオプションを使用できます。
   * **applicationProfile** – この値に基づいてサーバー設定を行うために使用できます。
   * **visitorID** - Experience Cloud ID サービス。 この値は、後で高度な分析レポートに使用できます。
   * **handleSVC** - プログラマがSFSafariViewControllersを処理するかどうかを示すブール値。 詳しくは、iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)の[SFSafariViewController サポートを参照してください。
      * **falseに設定すると、SDKは自動的にSFSafariViewControllerをエンドユーザーに表示します。** SDKは、MVPDのログインページ URLにさらに移動します。
      * **trueに設定すると、**&#x200B;のSDKは&#x200B;**NOT**&#x200B;によってSFSafariViewControllerがエンドユーザーに自動的に表示されます。 SDKは&#x200B;**navigate （toUrl:{url}, useSVC:YES）**&#x200B;をさらにトリガーします。
* **device\_info** - [ クライアント情報を渡す](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)の説明に従ったクライアント情報。

[トップへ戻る…](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーのIDを確立します。 各プログラマーには、Adobe Pass認証システムにAdobeに登録する際に一意のIDが割り当てられます。 SSOおよびリモートトークンを扱う場合、アプリケーションがバックグラウンドにあるときに認証状態が変更される可能性があります。システム状態と同期するためにアプリケーションがフォアグラウンドに入ったときにsetRequestorを再度呼び出すことができます（SSOが有効になっている場合はリモートトークンを取得し、その間にログアウトが発生した場合はローカルトークンを削除します）。

サーバー応答には、MVPDのリストと、プログラマーのIDに添付されているいくつかの設定情報が含まれています。 サーバー応答は、AccessEnabler コードによって内部で使用されます。 操作のステータス （つまり、SUCCESS/FAIL）のみが、`setRequestorComplete:` コールバックを介してアプリケーションに表示されます。

`urls` パラメーターが使用されていない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダーURL （Adobe RELEASE/production environment）をターゲットにします。


`urls` パラメーターに値が指定されている場合、結果のネットワーク呼び出しは、`urls` パラメーターで指定されたすべてのURLをターゲットにします。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPDのリストをコンパイルする場合は、最初のレスポンダーが優先されます。 リスト内の各MVPDについて、AccessEnablerは関連するサービスプロバイダーのURLを記憶します。 その後のすべての使用権限リクエストは、設定フェーズでターゲット MVPDとペア設定されたサービスプロバイダーに関連付けられたURLに送信されます。

| API呼び出し：依頼者設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID` |


**可用性：** v3.0以降

| API呼び出し：依頼者設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**可用性：** v3.0以降

**パラメーター：**

* *requestorID*: プログラマーに関連付けられている一意のID。 Adobeで割り当てられた一意のIDを、最初にAdobe Pass Authentication Serviceに登録するときにサイトに渡します。
* *urls*: オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で様々なインスタンスを使用する場合があります）。 これを使用して、複数のAdobe Pass認証サービスプロバイダーインスタンスを指定できます。 この場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。

>[!NOTE]
>
>`serviceProviders` パラメーターなしで呼び出された場合、ライブラリはデフォルトのサービスプロバイダー（実稼動プロファイルの場合は`https://sp.auth.adobe.com`、ステージングプロファイルの場合は`https://sp.auth-staging.adobe.com`）から設定を取得します。 `serviceProviders` パラメーターが指定されている場合は、URLの配列である必要があります。 設定情報は、指定されたすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダーの応答に重複する情報が存在する場合、競合は最速の応答サーバー（つまり、応答時間が最も短いサーバーが優先されます）に優先して解決されます。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)

[トップへ戻る…](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [非推奨] {#setReq}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーのIDを確立します。 各プログラマーには、Adobe Pass認証システムにAdobeに登録する際に一意のIDが割り当てられます。 SSOおよびリモートトークンを扱う場合、アプリケーションがバックグラウンドにあるときに認証状態が変更される可能性があります。システム状態と同期するためにアプリケーションがフォアグラウンドに投入されたときにsetRequestorを再度呼び出すことができます（SSOが有効になっている場合はリモートトークンを取得し、その間にログアウトが発生した場合はローカルトークンを削除します）。

サーバー応答には、MVPDのリストと、プログラマーのIDに添付されているいくつかの設定情報が含まれています。 サーバー応答は、AccessEnabler コードによって内部で使用されます。 操作のステータス （つまり、SUCCESS/FAIL）のみが、`setRequestorComplete:` コールバックを介してアプリケーションに表示されます。

`urls` パラメーターが使用されていない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダーURL （Adobe RELEASE/production environment）をターゲットにします。

`urls` パラメーターに値が指定されている場合、結果のネットワーク呼び出しは、`urls` パラメーターで指定されたすべてのURLをターゲットにします。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPDのリストをコンパイルする場合は、最初のレスポンダーが優先されます。 リスト内の各MVPDについて、AccessEnablerは関連するサービスプロバイダーのURLを記憶します。 その後のすべての使用権限リクエストは、設定フェーズでターゲット MVPDとペア設定されたサービスプロバイダーに関連付けられたURLに送信されます。

| API呼び出し：依頼者設定 |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**可用性：** v1.0+ **まで：** v3.0

| API呼び出し：依頼者設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**可用性：** v1.0+ **まで：** v3.0

**パラメーター：**

* *requestorID*: プログラマーに関連付けられている一意のID。 Adobeで割り当てられた一意のIDを、Adobe Pass Authentication Serviceに初めて登録したときにサイトに渡します。
* *signedRequestorID*: **このパラメーターは、iOS AccessEnabler バージョン 1.2以降に存在します。** 秘密鍵でデジタル署名された依頼者IDのコピー。 <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で様々なインスタンスを使用する場合があります）。 これを使用して、複数のAdobe Pass認証サービスプロバイダーインスタンスを指定できます。 この場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。

**メモ：** `serviceProviders` パラメーターなしで呼び出された場合、ライブラリはデフォルトのサービスプロバイダー（実稼動プロファイルの`https://sp.auth.adobe.com`、ステージングプロファイルの`https://sp.auth-staging.adobe.com`）から設定を取得します。 `serviceProviders` パラメーターが指定されている場合は、URLの配列である必要があります。 設定情報は、指定されたすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダーの応答に重複する情報が存在する場合、競合は最速の応答サーバーに優先して解決されます（つまり、応答時間が最も短いサーバーが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)


[トップへ戻る…](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [非推奨] {#setReq_tvos}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーのIDを確立します。 各プログラマーには、Adobe Pass認証システムにAdobeに登録する際に一意のIDが割り当てられます。 この設定は、アプリケーションのライフサイクル中に1回だけ実行する必要があります。

サーバー応答には、MVPDのリストと、プログラマーのIDに添付されているいくつかの設定情報が含まれています。 サーバー応答は、AccessEnabler コードによって内部で使用されます。 操作のステータス （つまり、SUCCESS/FAIL）のみが、`setRequestorComplete:` コールバックを介してアプリケーションに表示されます。

`urls` パラメーターが使用されていない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダーURL （Adobe RELEASE/production environment）をターゲットにします。

`urls` パラメーターに値が指定されている場合、結果のネットワーク呼び出しは、`urls` パラメーターで指定されたすべてのURLをターゲットにします。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPDのリストをコンパイルする場合は、最初のレスポンダーが優先されます。 リスト内の各MVPDについて、AccessEnablerは関連するサービスプロバイダーのURLを記憶します。 その後のすべての使用権限リクエストは、設定フェーズでターゲット MVPDとペア設定されたサービスプロバイダーに関連付けられたURLに送信されます。



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：依頼者設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v2.0+ **まで：** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：依頼者設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**可用性：** v2.0+ **まで：** v3.0

**パラメーター：**

* *requestorID*: プログラマーに関連付けられている一意のID。 Adobeで割り当てられた一意のIDを、Adobe Pass Authentication Serviceに初めて登録したときにサイトに渡します。
* *signedRequestorID*: **このパラメーターは、iOS AccessEnabler バージョン 1.2以降に存在します。** 秘密鍵でデジタル署名された依頼者IDのコピー。 <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で様々なインスタンスを使用する場合があります）。 これを使用して、複数のAdobe Pass認証サービスプロバイダーインスタンスを指定できます。 この場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。
* secret and publicKey: 2回目のscreen呼び出しに署名するために使用される秘密鍵と公開鍵。 詳しくは、[ クライアントレスのドキュメント ](#create_dev)を参照してください。

`serviceProviders` パラメーターなしで呼び出された場合、ライブラリはデフォルトのサービスプロバイダー（実稼動プロファイルの場合は`https://sp.auth.adobe.com`、ステージングプロファイルの場合はhttps://sp.auth-staging.adobe.com）から設定を取得します。 `serviceProviders` パラメーターが指定されている場合は、URLの配列である必要があります。 設定情報は、指定されたすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダーの応答に重複する情報が存在する場合、競合は最速の応答サーバーに優先して解決されます（つまり、応答時間が最も短いサーバーが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)

[トップへ戻る…](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明**&#x200B;設定フェーズが完了したことをアプリケーションに通知するAccessEnablerによってトリガーされるコールバック。 これは、アプリが使用権限リクエストの発行を開始できることを示すシグナルです。 設定フェーズが完了するまで、アプリケーションは使用権限の要求を発行できません。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：依頼者設定が完了しました</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0以降

**パラメーター**:

* *status*：次のいずれかの値を取ることができます：
   * `ACCESS_ENABLER_STATUS_SUCCESS` – 構成フェーズが正常に完了しました
   * `ACCESS_ENABLER_STATUS_ERROR` – 構成フェーズが失敗しました

**トリガー：**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[トップへ戻る…](#apis)

</br>

### checkAuthentication {#checkAuthN}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**現在のユーザーの認証ステータスを確認します。これは、ローカルの有効な認証トークンを検索することで実現します
トークンのストレージ容量： このメソッドはネットワーク呼び出しを実行せず、メインスレッドで呼び出すことをお勧めします。アプリケーションは、ユーザーの認証ステータスをクエリするために使用され、
それに応じてUIを更新します（ログイン/ログアウト UIの更新など）。 を
認証ステータスは、を介してアプリケーションに通知されます
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバック。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証ステータスの確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：


[トップへ戻る…](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;完全な認証ワークフローを開始します。 まず、認証ステータスを確認します。 まだ認証されていない場合は、認証フローのstate-machineが開始されます。

* 最後の認証が成功した場合、MVPDの選択フェーズはスキップされ、[`navigateToUrl:`](#nav2url) コールバックがトリガーされます。 このコールバックを使用して、MVPDのログインページを表示するWebView コントロールをインスタンス化します。 **[メモ：Access Enabler 1.5の時点では、SDK]の制限により、この機能は使用できません。**
* 最後の認証が失敗した場合、またはユーザーが明示的にログアウトした場合、[`displayProviderDialog:`](#dispProvDialog) コールバックがトリガーされます。 アプリケーションでは、このコールバックを使用してMVPDの選択UIを表示します。 また、AccessEnabler ライブラリに[`setSelectedProvider:`](#setSelProv) メソッドを介してユーザーのMVPDの選択を通知することで、認証フローを再開する必要があります。

ユーザーの資格情報はMVPD ログインページで確認されるため、ユーザーがMVPD ログインページで認証する間に行われる複数のリダイレクト操作をモニターする必要があります。 正しい資格情報を入力すると、WebView コントロールは`ADOBEPASS_REDIRECT_URL`定数で定義されたカスタム URLにリダイレクトされます。 このURLは、WebViewで読み込まれることを意図したものではありません。 アプリケーションはこのURLをインターセプトし、ログインフェーズが完了したことを示すシグナルとしてこのイベントを解釈する必要があります。 次に、認証フローを完了するためにAccessEnablerに制御を渡す必要があります（[handleExternalURL](#handleExternalURL) メソッドを呼び出すことによって）。

最後に、認証ステータスが[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバックを介してアプリケーションに通知されます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**可用性：** v1.9以降

**パラメーター：**

* *forceAuthn*: ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
* *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるディクショナリ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:`、[`displayProviderDialog:`](#dispProvDialog)、`sendTrackingData:forEventType:`


[トップへ戻る…](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;完全な認証ワークフローを開始します。 まず、認証ステータスを確認します。 まだ認証されていない場合は、認証フローのstate-machineが開始されます。

* 現在の依頼者にSSOをサポートするMVPDが1つ以上ある場合、[presentTvProviderDialog （） ](#presentTvDialog)が呼び出されます。 MVPDでSSOがサポートされていない場合、クラシック認証フローが開始され、フィルターパラメーターは無視されます。
* ユーザーが完了すると、Apple SSO フロー[`dismissTvProviderDialog()`](#dismissTvDialog)がトリガーされ、認証プロセスが終了します。

最後に、認証ステータスが[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバックを介してアプリケーションに通知されます。

**可用性：** v2.4以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>
 </div></td>
</tr>
</tbody>
</table>



**パラメーター：**

* *forceAuthn*: ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
* *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるディクショナリ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。
* filter: Apple SSO ダイアログに表示されるMVPD IDの2つのリストを含むディクショナリ。 SSOをサポートしていないMVPDは無視されますが、注文は尊重されます。 辞書には2つのキーが必要です。
   * TV\_PROVIDERS: ピッカーに表示されるすべてのMVPDを含むリスト
   * FEATURED\_TV\_PROVIDERS: ピッカーでフィーチャーとしてマークされるすべてのMVPDを含むリスト。 このリストのMVPDもTV\_PROVIDERS リストで指定する必要があります。

**可用性：** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**パラメーター：**

* *forceAuthn*: ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
* *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるディクショナリ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。
* filter: Apple SSO ダイアログに表示されるMVPD IDのリスト。 SSOをサポートしていないMVPDは無視されますが、注文は尊重されます。

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[トップへ戻る…](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnablerによってトリガーされたコールバックは、ユーザーが目的のMVPDを選択できるようにするために、適切なUI要素をインスタンス化する必要があることをアプリケーションに通知します。 このコールバックは、MVPD オブジェクトのリストに、MVPDのロゴを指すURLやわかりやすい表示名など、選択UI パネルの正しい構築に役立つ情報を提供します。

ユーザーが目的のMVPDを選択したら、上位のアプリケーションは、`setSelectedProvider:`を呼び出して、ユーザーの選択に対応するMVPDのIDを渡すことにより、認証フローを再開する必要があります。

**認証フローの中止** – これは、ユーザーが「戻る」ボタンを押すことができるポイントです。これは、認証フローの中止に相当します。 このシナリオでは、AccessEnablerに認証状態マシンをリセットする機会を与えるために、[setSelectedProvider:](#setSelProv) メソッドを呼び出し、パラメーターとしてnullを渡すアプリケーションが必要です。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：MVPDの選択UIの表示</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0以降

**パラメーター**:

* *mvpds*: アプリケーションがMVPD selection UI要素の構築に使用できる、MVPD関連の情報を保持するMVPD オブジェクトのリスト。

**トリガー：** `getAuthentication`、[`getAuthentication:withData:`](#getAuthN)、`getAuthorization:`、[`getAuthorization:withData:`](#getAuthZ)


[トップへ戻る…](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、ユーザーのMVPDの選択内容をAccess Enablerに通知するためにアプリケーションによって呼び出されます。 アプリケーションは、この方法を使用して、認証に使用するサービスプロバイダーを選択または変更できます。

選択したMVPDがTempPass MVPDの場合、後でgetAuthentication （）を呼び出すことなく、そのMVPDで自動的に認証されます。

getAuthentication （） メソッドに追加のパラメーターが指定されているプロモーション一時パスでは、これは不可能であることに注意してください。

パラメーターとして&#x200B;*null*&#x200B;を渡す場合、Access Enablerは、ユーザーが認証フローをキャンセルしたと仮定し（つまり、「戻る」ボタンを押した）、認証状態マシンをリセットし、[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバックを`AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` エラーコードで呼び出すことによって応答します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:`、`sendTrackingData:forEventType:`、[`navigateToUrl:`](#nav2url)

[トップへ戻る…](#apis)

</br>

#### navigateToUrl: {#nav2url}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** AccessEnablerによってトリガーされたコールバックは、UIWebView/WKWebView コントローラーのインスタンス化と、コールバックの&#x200B;**`url`** パラメーターで指定されたURLの読み込みをアプリケーションに要求します。 コールバックは、認証エンドポイントのURLまたはログアウトエンドポイントのURLを表す&#x200B;**`url`** パラメーターを渡します。

UIWebView/WKWebView` ` コントローラーが複数のリダイレクトを行う場合、アプリケーションはコントローラーのアクティビティを監視し、`ADOBEPASS_REDIRECT_URL `定数（つまり`adobepass://ios.app`）で定義された特定のカスタム URLを読み込む瞬間を検出する必要があります。 この特定のカスタム URLは実際には無効であり、コントローラが実際に読み込むことを意図していないことに注意してください。 認証またはログアウトフローが完了し、コントローラーを安全に閉じることができることを示すシグナルとしてのみ、アプリケーションによって解釈される必要があります。 コントローラーがこの特定のカスタム URLを読み込むと、アプリケーションはUIWebView/WKWebViewを閉じて、AccessEnablerの`handleExternalURL:url `API メソッドを呼び出す必要があります。

**注意：**&#x200B;認証フローの場合、これはユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このようなシナリオでは、アプリケーションは&#x200B;**`nil`**&#x200B;をパラメーターとして渡す[setSelectedProvider:](#setSelProv) メソッドを呼び出し、AccessEnablerに認証状態マシンをリセットする機会を提供する必要があります。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：MVPD ログインページの表示</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター**:

* *url*: MVPDのログインページを指すURL

**トリガー：** [setSelectedProvider:](#setSelProv)



[トップへ戻る…](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** [setOptions （\[&quot;handleSVC&quot;:true&quot;\]） ](#setOptions)呼び出しを介した手動のSafari View Controller （SVC）処理を以前にアプリケーションで有効にした場合に、AccessEnablerではなく`navigateToUrl:` コールバックによってトリガーされるコールバック。また、Safari View Controller （SVC）を必要とするMVPDの場合のみ実行できます。 その他のすべてのMVPDでは、`navigateToUrl:` コールバックが呼び出されます。 Safari View Controller （SVC）の管理方法について詳しくは、[iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)のSFSafariViewController サポートを参照してください。

`navigateToUrl:` コールバックと同様に、`navigateToUrl:useSVC:`はAccessEnablerによってトリガーされ、`SFSafariViewController` コントローラーのインスタンス化と、コールバックの&#x200B;**`url`** パラメーターで指定されたURLの読み込みをアプリケーションにリクエストします。 コールバックは、認証エンドポイントのURLまたはログアウトエンドポイントのURLを表す&#x200B;**`url`** パラメーターと、アプリケーションが`SFSafariViewController`を使用する必要があることを指定する&#x200B;**`useSVC`** パラメーターを渡します。

`SFSafariViewController` コントローラーが複数のリダイレクトを行う場合、アプリケーションはコントローラーのアクティビティを監視し、`application's custom scheme`で定義された特定のカスタム URL （例：** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）を読み込む瞬間を検出する必要があります。 この特定のカスタム URLは実際には無効であり、コントローラが実際に読み込むことを意図していないことに注意してください。 認証またはログアウトフローが完了し、コントローラーを安全に閉じることができることを示すシグナルとしてのみ、アプリケーションによって解釈される必要があります。 コントローラーがこの特定のカスタム URLを読み込むと、アプリケーションは`SFSafariViewController`を閉じて、AccessEnablerの`handleExternalURL:url `API メソッドを呼び出す必要があります。

**注意：**&#x200B;認証フローの場合、これはユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このようなシナリオでは、アプリケーションは&#x200B;**`nil`**&#x200B;をパラメーターとして渡す[setSelectedProvider:](#setSelProv) メソッドを呼び出し、AccessEnablerに認証状態マシンをリセットする機会を提供する必要があります。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: SFSafariViewControllerにMVPD ログインページを表示する</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

** 可用性：**v 3.2以降

**パラメーター**:

* *url:* MVPDのログインページを指すURL
* *useSVC:* URLをSFSafariViewControllerに読み込むかどうかを指定します。

**トリガー：**[ setOptions:](#setOptions)前[setSelectedProvider:](#setSelProv)

[トップへ戻る…](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、認証またはログアウトフローを完了するためにアプリケーションによって呼び出されます。 このメソッドは、`UIWebView/WKWebView or SFSafariViewController` コントローラーが特定のカスタム URLにリダイレクトされた瞬間をアプリケーションが検出した直後に呼び出す必要があります。 アプリケーションで`SFSafariViewController ` コントローラーを使用する必要がある場合、特定のカスタム URLは`application's custom scheme` （例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）によって定義されます。定義されていない場合、この特定のカスタム URLは`ADOBEPASS_REDIRECT_URL `定数（例：`adobepass://ios.app`）によって定義されます。

認証フローの場合、AccessEnablerは、バックエンドサーバーから認証トークンを取得し、トークンのストレージにローカルに保存することで、フローを完了します。 AccessEnablerは、認証フローが完了したことをアプリケーションに通知し、成功を示すステータスコード 1で`setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> コールバックを呼び出します。 これらの手順の実行中にエラーが発生した場合、`setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> コールバックは、認証失敗を示すステータスコード 0と、対応するエラーコードでトリガーされます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証またはログアウトフローを完了します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v3.0以降

**パラメーター：**

* *url*: ` UIWebView/WKWebView or SFSafariViewController ` コントロールから文字列として傍受したURL。


**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[トップへ戻る…](#apis)

</br>

#### getAuthenticationToken - [非推奨] {#getAuthNToken}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** バックエンドサーバーから認証トークンを要求して、認証フローを完了します。 このメソッドは、MVPD ログインページをホストするWebView コントロールが`ADOBEPASS_REDIRECT_URL`定数で定義されたカスタム URLにリダイレクトされるイベントに応じてのみ、アプリケーションで呼び出す必要があります。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証トークンを取得します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+ **まで：** v3.0

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[トップへ戻る…](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明**&#x200B;認証フローのステータスをアプリケーションに通知するAccessEnablerによってトリガーされるコールバック。 ユーザーの操作の結果として、または他の予期せぬシナリオ（ネットワーク接続の問題など）によって、このフローが失敗する可能性がある場所は数多くあります。 このコールバックは、認証フローの成功/失敗ステータスをアプリケーションに通知し、必要に応じて失敗の理由に関する追加情報も提供します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：認証フローのステータスをレポートします</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0以降

**パラメーター**:

* *status*：次のいずれかの値を取ることができます：
   * `ACCESS_ENABLER_STATUS_SUCCESS` – 認証フローが正常に完了しました
   * `ACCESS_ENABLER_STATUS_ERROR` – 認証フローに失敗しました
* *code*：失敗の理由。 *status*&#x200B;が`ACCESS_ENABLER_STATUS_SUCCESS`の場合、*code*&#x200B;は空の文字列です（つまり、`USER_AUTHENTICATED`定数で定義されています）。 失敗した場合、このパラメーターは次のいずれかの値を取ることができます。
   * `USER_NOT_AUTHENTICATED_ERROR` - ユーザーが認証されていません。 ローカル トークン キャッシュに有効な認証トークンがない場合の[checkAuthentication:](#checkAuthN) メソッドの呼び出しに応答します。
   * `PROVIDER_NOT_SELECTED_ERROR` – 上層アプリケーションが&#x200B;*null*&#x200B;を[`setSelectedProvider:`](#setSelProv)に渡して認証フローを中止した後、AccessEnablerは認証状態マシンをリセットしました。  おそらく、ユーザーは認証フローをキャンセルしました（「戻る」ボタンを押しました）。
   * `GENERIC_AUTHENTICATION_ERROR` - ネットワークが利用できないなどの理由で認証フローが失敗したか、ユーザーが認証フローを明示的にキャンセルしました。

**トリガー：** `checkAuthentication`、`getAuthentication`、[`getAuthentication:withData:`](#getAuthN)、`checkAuthorization:`、[`checkAuthorization:withData:`](#checkAuthZ)

[トップへ戻る…](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、ユーザーが特定の保護されたリソースを表示する権限を既に持っているかどうかを判断するために、アプリケーションで使用されます。 このメソッドの主な目的は、UI **の装飾に使用する情報を取得することです（例えば、ロックおよびロック解除アイコンを使用したアクセス状態を示します）。**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.3以降

**パラメーター：**

* *リソース：*&#x200B;認証を確認する必要があるリソースの配列。 リスト内の各要素は、リソース IDを表す文字列である必要があります。 リソース IDは、呼び出し中のリソース IDと同じ制限の対象となります。つまり、プログラマーとMVPDまたはMedia RSS フラグメントの間で設定された合意された値である必要があります。

**コールバックがトリガーされました：** [`preauthorizedResources:`](#preauthResources)

[トップへ戻る…](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、ユーザーが特定の保護されたリソースを表示する権限を既に持っているかどうかを判断するために、アプリケーションで使用されます。 この方法の主な目的は、UIの装飾に使用する情報を取得することです（例えば、ロックアイコンとロック解除アイコンを使用してアクセスステータスを示す）。 **cache** パラメーターは、内部キャッシュをリソースの解決に使用するかどうかを制御します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**可用性：** v3.1以降



**パラメーター：**

* *リソース：*&#x200B;認証を確認する必要があるリソースの配列。 リスト内の各要素は、リソース IDを表す文字列である必要があります。 リソース IDは、`getAuthorization:`呼び出しのリソース IDと同じ制限の対象となります。つまり、プログラマーとMVPDまたはMedia RSS フラグメントの間で設定された合意値である必要があります。
* *cache:* リソースの解決に内部キャッシュを使用するかどうかを指定するブール値。 falseの場合、キャッシュはバイパスされ、このAPIが呼び出されるたびにサーバー呼び出しが行われます。

**コールバックがトリガーされました：** [`preauthorizedResources:`](#preauthResources)

[トップへ戻る…](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** コールバックが`checkPreauthorizedResources:`によってトリガーされました。 ユーザーが既に表示を許可されているリソースのリストを提供します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.3以降

**パラメーター：**

* `resources`: ユーザーが既に表示を許可されているリソースの配列。

**トリガー：** [`checkPreauthorizedResources:`](#checkPreauth)



[トップへ戻る…](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、アプリケーションが認証ステータスを確認するために使用します。 まず、認証ステータスを確認します。 認証されていない場合、[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) コールバックがトリガーされ、メソッドが終了します。 ユーザーが認証された場合は、認証フローもトリガーします。 [`getAuthorization:`](#getAuthZ) メソッドの詳細を参照してください。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証ステータスの確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証ステータスの確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.9以降

**パラメーター：**

* *リソース*: ユーザーが認証を要求するリソースのID。
* *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるディクショナリ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。

**コールバックがトリガーされました：**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[トップへ戻る…](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、アプリケーションが認証フローを開始するために使用します。 ユーザーがまだ認証されていない場合は、認証フローも開始します。 ユーザーが認証された場合、AccessEnablerは認証トークンのリクエスト（有効な認証トークンがローカルトークンキャッシュに存在しない場合）と短期間有効なメディアトークンのリクエストを発行します。 ショートメディアトークンが取得されると、認証フローは完了したものとみなされます。 [`setToken:forResource:`](#setToken) コールバックがトリガーされ、ショートメディアトークンがパラメーターとしてアプリケーションに配信されます。 何らかの理由で認証が失敗した場合は、[`tokenRequestFailed:forEventType:`](#tokenReqFailed) コールバックがトリガーされ、エラーコードまたは詳細が提供されます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**可用性：** v1.9以降

**パラメーター：**

* *リソース*: ユーザーが認証を要求するリソースのID。
* *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるディクショナリ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。

**コールバックがトリガーされました：** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**追加のコールバックがトリガーされました：**\
このメソッドは、次のコールバックをトリガーすることもできます（フローも開始されている場合）: `setAuthenticationStatus:errorCode:`、`displayProviderDialog:`

>[!NOTE]
>
>可能な限り、`getAuthorization:` / `getAuthorization:withData:`の代わりに`checkAuthorization:` / `checkAuthorization:withData:`を使用してください。 `getAuthorization:` / `getAuthorization:withData:` メソッドは、（ユーザーが認証されていない場合）完全な認証フローを開始します。これにより、プログラマー側で複雑な実装が行われる可能性があります。

[トップへ戻る…](#apis)

</br>

### `setToken:forResource:` {#setToken}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明**&#x200B;認証フローが正常に完了したことをアプリケーションに通知するAccessEnablerによってトリガーされるコールバック。 短期間有効なメディアトークンもパラメーターとして配信されます。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：認証フローが正常に完了しました</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター**:

* *トークン*：短期間有効なメディアトークン
* *リソース*：認証を取得したリソース

**トリガー：** [`checkAuthorization:`](#checkAuthZ)、[`checkAuthorization:withData:`](#checkAuthZ)、[`getAuthorization:`](#getAuthZ)、[`getAuthorization:withData:`](#getAuthZ)

[トップへ戻る…](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnablerによってトリガーされたコールバックは、認証フローが失敗したことを上位アプリケーションに通知します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：認証フローに失敗しました</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター**:

* *リソース*：認証を取得したリソース。
* *code*：失敗シナリオに関連付けられたエラーコード。 使用可能な値：
   * `USER_NOT_AUTHORIZED_ERROR` - ユーザーは認証できませんでした
与えられたリソースに対して
* *description*：失敗シナリオに関する追加の詳細。 この記述文字列が何らかの理由で使用できない場合、Adobe Pass Authenticationは空の文字列&#x200B;**（&quot;&quot;）**&#x200B;を送信します。\
  この文字列は、MVPDでカスタムエラーメッセージまたはセールス関連メッセージを渡すために使用できます。 例えば、サブスクライバーがリソースの認証を拒否された場合、MVPDは次のようなメッセージを送信できます。「現在、パッケージ内のこのチャネルにアクセスできません。 パッケージをアップグレードする場合は、**こちら**&#x200B;をクリックしてください。」 メッセージは、このコールバックを通じてAdobe Pass認証によってプログラマーに渡されます。プログラマーは、メッセージを表示または無視するオプションを持っています。 Adobe Pass認証では、このパラメーターを使用して、エラーの原因となった可能性のある条件を通知することもできます。 例えば、「プロバイダーの認証サービスと通信する際にネットワークエラーが発生しました」などです。

**トリガー：** `checkAuthorization:`、[`checkAuthorization:withData:`](#checkAuthZ)、`getAuthorization:`、[`getAuthorization:withData:`](#getAuthZ)

[トップへ戻る…](#apis)

</br>

### ログアウト {#logout}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドは、ログアウトフローを開始するためにアプリケーションによって呼び出されます。 ログアウトは、ユーザーがMVPD認証サーバーとAdobe Pass認証サーバーの両方からログアウトする必要があるため、一連のHTTP リダイレクト操作の結果です。 このフローは、AccessEnabler ライブラリが発行した単純なHTTP リクエストでは完了できないため、HTTP リダイレクト操作に従うには、`UIWebView/WKWebView or SFSafariViewController` コントローラーをインスタンス化する必要があります。

ログアウト フローは、ユーザーが`UIWebView/WKWebView or SFSafariViewController` コントローラーと何らかの操作を行う必要がないという点で、認証フローとは異なります。 したがって、Adobeでは、ログアウトプロセス中にコントロールを非表示（非表示）にすることをお勧めします。

認証フローと類似したパターンを採用する。 IOS AccessEnablerは、`navigateToUrl:` コールバックまたは`navigateToUrl:useSVC:`をトリガーして`UIWebView/WKWebView or SFSafariViewController` コントローラーを作成し、コールバックの`url` パラメーターで指定されたURLを読み込みます。 これは、バックエンドサーバー上のログアウトエンドポイントのURLです。 tvOS AccessEnablerの場合、`navigateToUrl:` コールバックも`navigateToUrl:useSVC:` コールバックも呼び出されません。

複数のリダイレクトを行う場合、アプリケーションは`UIWebView/WKWebView or SFSafariViewController ` コントローラーのアクティビティを監視し、特定のカスタム URLを読み込む瞬間を検出する必要があります。 この特定のカスタム URLは実際には無効であり、コントローラが実際に読み込むことを意図していないことに注意してください。 ログアウトフローが完了し、コントローラーを安全に閉じることができることを示すシグナルとしてのみ、アプリケーションによって解釈される必要があります。 コントローラーがこの特定のカスタム URLを読み込むと、アプリケーションはコントローラーを閉じ、AccessEnablerの`handleExternalURL:url `API メソッドを呼び出す必要があります。 アプリケーションで`SFSafariViewController ` コントローラーを使用する必要がある場合、特定のカスタム URLは`application's custom scheme` （例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）によって定義されます。定義されていない場合、この特定のカスタム URLは`ADOBEPASS_REDIRECT_URL `定数（例：`adobepass://ios.app`）によって定義されます。

最後に、AccessEnablerはステータスコードが0の[`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、ログアウトフローの成功を示します。

**メモ：** ユーザーがApple SSOを使用してログインしている場合、VSA203 ステータスがトリガーされます。 その場合は、システム設定からもログアウトするようにユーザーに指示する必要があります。 これを怠ると、アプリケーションが再起動したときに再認証が行われます。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：ログアウトフローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `navigateToUrl:`、[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[トップへ戻る…](#apis)

</br>

### getSelectedProvider {#getSelProv}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドを使用して、現在選択されているプロバイダーを決定します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：現在選択されているMVPDを決定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** [`selectedProvider:`](#selProv)

[トップへ戻る…](#apis)

</br>

### selectedProvider {#selProv}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

現在選択されているMVPDに関する情報をアプリケーションに配信するAccessEnablerによってトリガーされる&#x200B;**説明** コールバック。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：現在選択されているMVPDに関する情報</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター**:

* *mvpd*：現在選択されているMVPDに関する情報を含むオブジェクト

**トリガー：** [`getSelectedProvider`](#getSelProv)

[トップへ戻る…](#apis)

</br>

### getMetadata: {#getMeta}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：**&#x200B;このメソッドを使用して、AccessEnabler ライブラリによってメタデータとして公開された情報を取得します。 アプリケーションは、辞書ベースの&#x200B;*key*&#x200B;入力パラメーターを指定することで、このデータにアクセスできます。

プログラマーが利用できるメタデータには、次の2種類があります。

* 静的メタデータ（認証トークン TTL、認証トークン TTLおよびデバイス ID）
* ユーザーメタデータ（ユーザーID、郵便番号などのユーザー固有の情報。認証および認証フロー中に、MVPDからユーザーのデバイスに渡すことができます）

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API呼び出し：メタデータのAccessEnablerをクエリします</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター：**

* *keyDictionary*：次のディクショナリ データ構造
フォーマット：
   * キーが`METADATA_OPCODE_KEY`、値が`METADATA_AUTHENTICATION`の場合、認証トークンの有効期限を取得するためにクエリが実行されます。
   * キーが`METADATA_OPCODE_KEY`、値が`METADATA_AUTHORIZATION` **および**&#x200B;の場合\
     キーは`METADATA_RESOURCE_ID_KEY`で、値は特定のリソース IDです。その後、クエリを実行して、指定されたリソースに関連付けられている認証トークンの有効期限を取得します。
   * キーが`METADATA_OPCODE_KEY`、値が`METADATA_DEVICE_ID`の場合、クエリは現在のデバイス IDを取得するために行われます。 この機能はデフォルトで無効になっており、プログラマーは有効化と料金についてAdobeに問い合わせる必要があります。
   * キーが`METADATA_OPCODE_KEY`、値が`METADATA_USER_META` **、** キーが`METADATA_USER_META_KEY`、値がメタデータの名前である場合、ユーザーメタデータに対してクエリが実行されます。 使用可能なユーザーメタデータタイプのリスト：
      * `zip` - Zip コードのリスト
      * `householdID` – 世帯ID。 MVPDがサブアカウントをサポートしていない場合、これは`userID`と同じになります。
      * `maxRating` - ユーザーの親の最大評価のコレクション
      * `userID` - ユーザーID。 MVPDがサブアカウントをサポートしており、ユーザーがメインアカウントでない場合、`userID`は`householdID.`とは異なります
      * `channelID` - ユーザーが表示権限を持つチャネルのリスト。

  >[!NOTE]
  >
  >プログラマが実際に使用できるユーザーメタデータは、MVPDで使用可能なメタデータによって異なります。 このリストは、新しいメタデータが利用可能になり、Adobe Pass Authentication Systemに追加されると拡張されます。

**コールバックがトリガーされました：** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**詳細情報：** [ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[トップへ戻る…](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

現在の依頼者がSSO サポートを持つ少なくとも1つのMVPDをサポートしている場合、**説明**&#x200B;現在の依頼者が[getAuthentication （） ](#getAuthN)を呼び出した後、AccessEnablerによってトリガーされるコールバック。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：SSO フローの結果</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v2.0以降

**パラメーター**:

* viewController: Apple SSO ダイアログを表します。 このviewControllerは画面に表示する必要があります。

**トリガー：** [`getAuthentication`](#getAuthN)

**詳細情報：** [iOS/tvOS シングルサインオン ](#presentTvDialog)

[トップへ戻る…](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

ユーザーがApple SSO ダイアログを閉じた後にAccessEnablerによってトリガーされる&#x200B;**説明** コールバック。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：SSO フローの結果</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v2.0以降

**パラメーター**:

* viewController: Apple SSO ダイアログを表します。 このviewControllerを画面から削除する必要があります。

**トリガー：** ユーザーアクション

**詳細情報：** [iOS/tvOS シングルサインオン ](#presentTvDialog)

[トップへ戻る…](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** コールバックは、[`getMetadata:`](#getMeta)呼び出しを介して要求されたメタデータを配信するAccessEnablerによってトリガーされます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：メタデータ取得リクエストの結果</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0以降

**パラメーター**:

* *metadata*：要求されたメタデータ。 静的メタデータ （認証TTL、認証TTL、デバイス ID）の場合、この値は`NSString`です。  ユーザー固有のメタデータをリクエストする場合は、複雑なオブジェクトです。 この複雑なオブジェクトは通常、JSON ペイロードのObjective-C表現です（例：&#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;buildings&quot;: [&quot;150&quot;, &quot;320&quot;]}&#39;はObjective-CでNSDictionary （&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;buildings&quot; -> NSArray （&quot;150&quot;, &quot;320&quot;））と訳されます）。   メタデータ JSON オブジェクトの例：

```JSON
        {
            updated: 1334243471,
            encrypted: ["encryptedProp"],
            data: {
                zip: ["12345", "34567"],
                maxRating: { 
                    "MPAA": "PG-13",
                    "VCHIP": "TV-Y", 
                    "URL": "http://exam.pl/e/manage/ratings"
                },
                householdID: "3456",
                userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                channelID: ["channel-1", "channel-2"]
            }
```

* *encrypted*：取得したメタデータが暗号化されているかどうかを指定するブール値。 このパラメーターは、ユーザーメタデータ要求に対してのみ有効であり、常に暗号化せずに受信される静的メタデータ（認証TTLなど）に対しては意味がありません。 このパラメーターがtrueに設定されている場合は、ホワイトリストに登録されている秘密鍵（[`setRequestor:setSignedRequestorId:`](#setReq)呼び出しと`setRequestor:setSignedRequestorId:serviceProviders: `呼び出しの依頼者IDの署名に使用されるのと同じ秘密鍵）を使用してRSA復号化を実行し、暗号化されていないユーザーメタデータ値を取得するのはプログラマーの責任です。

* *key*: メタデータ取得要求の策定に使用されるキー。

* *arguments*: [`getMetadata:`](#getMeta)呼び出しに渡された同じディクショナリ。 これは、アプリケーションがリクエストと応答を一致させるために提供されます。

**トリガー：** [`getMetadata:`](#getMeta)

**詳細情報：** [ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[トップへ戻る…](#apis)

</br>

### MVPD {#mvpd}

**ファイル：** AccessEnabler/headers/model/MVPD.h

**説明** MVPD オブジェクトについて説明します。 MVPDのプロパティに関する情報を取得するために使用できます。

**可用性：** v1.0+ [boardingStatus プロパティはv2.2]から利用できます

**プロパティ**:

* （NSString） ID - MVPD Id。
* （NSString） displayName - MVPD名。 [これは、ピッカーに表示するために使用する必要があります]
* （NSString） logoURL - MVPDのロゴアドレス。
* （BOOL） enablePlatformServices - trueの場合、MVPDは[Apple SSO](#presentTvDialog)のようなSSO サービスをサポートします。
* （NSString） boardingStatus - 3つの値を持つことができます。
   * nil - MVPDはApple SSOをサポートしていません。
   * PICKER - MVPDはApple ピッカーに表示されますが、認証フローはAdobeによって実行されます。
   * サポート - MVPDはAppleで完全にサポートされており、AppleのSSO トークンを使用します。

[トップへ戻る…](#apis)

</br>

## トラッキングイベント {#tracking}

AccessEnablerは、必ずしも使用権限フローに関連しない追加のコールバックをトリガーします。 [`sendTrackingData()`](#sendTracking) コールバック関数の実装はオプションですが、特定のイベントを追跡し、認証/承認の試行回数などの統計をコンパイルすることをアプリケーションで有効にします。

### sendTrackingData:forEventType: {#sendTracking}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明**&#x200B;認証/認証フローの完了/失敗など、様々なイベントの発生をアプリケーションに通知するAccessEnablerによってトリガーされるコールバック。 Adobe Pass Authentication 1.6では、デバイスの種類、AccessEnabler クライアントの種類、およびオペレーティング システムが[`sendTrackingData()`](#sendTracking)によって報告されます。 [`sendTrackingData()`](#sendTracking) コールバックは後方互換性を維持します。

**コールバック：トラッキングイベント**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**可用性：** v1.0以降

**注：** デバイスの種類とオペレーティング システムは、パブリック Java ライブラリ （<http://java.net/projects/user-agent-utils>）とユーザーのエージェント文字列を使用して取得されます。 この情報は、操作指標をデバイスカテゴリーに分類する大まかな方法としてのみ提供されますが、Adobeは誤った結果に対して責任を負うことはできません。 それに応じて新しい機能を使用してください。

* デバイスタイプの可能な値：
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* AccessEnabler クライアントの種類に指定できる値：
   * `flash`
   * `html5`
   * `ios`
   * `android`


**パラメーター**:

* *event*：追跡されているイベントのコード。 トラッキングイベントには、次の3つのタイプがあります。
   * **authorizationDetection:**&#x200B;認証トークン要求が返されるたびに（イベントは`TRACKING_AUTHORIZATION`）
   * 認証チェックが発生するたびに&#x200B;**authenticationDetection:** （イベントは`TRACKING_AUTHENTICATION`）です
   * **mvpdSelection:**：ユーザーがMVPD セレクション フォームでMVPDを選択した場合（イベントは`TRACKING_GET_SELECTED_PROVIDER`）
* *data*：報告されたイベントに関連付けられている追加データ。 このデータは、値のリストの形式で表示されます。

**トリガー：** `checkAuthentication`、`getAuthentication`、[`getAuthentication:withData:`](#getAuthN)、`checkAuthorization:`、[`checkAuthorization:withData:`](#checkAuthZ)、`getAuthorization:`、[`getAuthorization:withData:`](#getAuthZ)、`setSelectedProvider:`

*data*&#x200B;配列内の値を解釈する手順：

* trackingEventType `TRACKING_AUTHENTICATION:`の場合
   * **0** - トークン要求が成功したかどうか（true/false）、成功した場合：
   * **1** - MVPD ID文字列
   * **2** - GUID （md5 ハッシュ）
   * **3** - トークンは既にキャッシュ内にあります（true/false）
   * **4** - デバイスの種類
   * **5** - AccessEnabler クライアント タイプ
   * **6** - オペレーティング システムの種類

* trackingEventType `TRACKING_AUTHORIZATION:`の場合
   * **0** - トークン要求が成功したかどうか（true/false）、成功した場合：
   * **1** - MVPD ID
   * **2** - GUID （md5 ハッシュ）
   * **3** - トークンは既にキャッシュ内にあります（true/false）
   * **4** - エラー
   * **5** – 詳細
   * **6** - デバイスの種類
   * **7** - AccessEnabler クライアント タイプ
   * **8** - オペレーティング システムの種類
* trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`の場合
   * **0** – 現在選択されているMVPDのID
   * **1** - デバイスの種類
   * **2** - AccessEnabler クライアント タイプ
   * **3** - オペレーティング システムの種類

</br>

