---
title: iOS/tvOS API リファレンス
description: iOS/tvOS API リファレンス
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# iOS/tvOS SDK API リファレンス {#iostvos-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#intro}

ここでは、Adobe Pass認証用のiOS/tvOS ネイティブクライアントで公開されるメソッドとコールバック関数について説明します。 ここで説明するメソッドとコールバック関数は、`AccessEnabler.h` および `EntitlementDelegate.h` ヘッダーファイルで定義されます。これらは、iOS AccessEnabler SDK の次の場所にあります。`[SDK directory]/AccessEnabler/headers/api/`


関連ドキュメント：

* 基本的なAdobe Pass認証の使用権限フローについては、[ 使用権限のフロー ](/help/authentication/entitlement-flow.md) を参照してください。
* Adobe Passの実装方法の段階的な説明
この API を使用した認証使用権限のフローについては、[iOS統合クックブック ](/help/authentication/iostvos-sdk-cookbook.md) を参照してください。
* 最新のiOS AccessEnabler SDK については、[iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library) を参照してください。

>[!NOTE]
>
>Adobeでは、Adobe Pass認証 *パブリック* API のみを使用することをお勧めします。
>
>* パブリック API は利用可能で、サポートされるすべてのクライアントタイプで完全にテストされています。 すべての公開機能で、各クライアントタイプに、関連するメソッドの対応するバージョンがあることを確認します。
>* 公開 API は、後方互換性をサポートし、パートナー統合が損なわれないようにするために、できるだけ安定している必要があります。 ただし、公開されていない API については、今後いつでも署名を変更する権利を留保します。 現在のパブリック Adobe Pass認証 API 呼び出しの組み合わせではサポートできない特定のフローが発生した場合は、お知らせいただくのが最善の方法です。 お客様のニーズを考慮して、公開 API を変更し、今後も安定したソリューションを提供できます。

</br>

## API リファレンス {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - AccessEnabler オブジェクトをインスタンス化します。

* **[非推奨]** [init](#init) - AccessEnabler オブジェクトをインスタンス化します。

* [`setOptions:options:`](#setOptions) - profile や visitorID などのグローバル SDK オプションを設定します。

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - プログラマの ID を設定します。

* **[非推奨]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - プログラマーの ID を設定します。

* **[非推奨]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos)、[`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos) - プログラマーの ID を設定します。

* [`setRequestorComplete:`](#setReqComplete) – 設定フェーズが完了したことをアプリケーションに通知します。

* [`checkAuthentication`](#checkAuthN) – 現在のユーザーの認証ステータスを確認します。

* [`getAuthentication`](#getAuthN)、[`getAuthentication:withData:`](#getAuthN) – 完全な認証ワークフローを開始します。

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) – 完全な認証ワークフローを開始します。

* [`displayProviderDialog:`](#dispProvDialog) - ユーザーが MVPD を選択するために適切な UI 要素をインスタンス化するように、アプリケーションに通知します。

* [`setSelectedProvider:`](#setSelProv) - ユーザーの MVPD 選択を AccessEnabler に通知します。

* [`navigateToUrl:`](#nav2url) - MVPD ログインページをユーザーに提示する必要があることをアプリケーションに通知します。

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - SFSafariViewController を使用して、MVPD ログインページをユーザーに提示する必要があることをアプリケーションに通知します。

* [`handleExternalURL:url`](#handleExternalURL) – 認証/ログアウトフローを完了します。

* **[非推奨]** [`getAuthenticationToken`](#getAuthNToken) - バックエンドサーバーから認証トークンをリクエストします。

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) – 認証フローのステータスをアプリケーションに通知します。

* [`checkPreauthorizedResources:`](#checkPreauth) – 特定の保護されたリソースの表示がユーザーに既に許可されているかどうかを判断します。

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) – 特定の保護されたリソースの表示がユーザーに既に許可されているかどうかを判断します。

* [`preauthorizedResources:`](#preauthResources) - ユーザーが既に表示を許可されているリソースのリストを提供します。

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) – 現在のユーザーの認証ステータスをチェックします。

* [`getAuthorization:`](#getAuthZ)、[`getAuthorization:withData:`](#getAuthZ) – 認証フローを開始します。

* [`setToken:forResource:`](#setToken) – 認証フローが正常に完了したことをアプリケーションに通知します。

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) – 認証フローが失敗したことをアプリケーションに通知します。

* [`logout`](#logout) - ログアウトフローを開始します。

* [`getSelectedProvider`](#getSelProv) – 現在選択されているプロバイダを指定します。

* [`selectedProvider:`](#selProv) – 現在選択されている MVPD に関する情報をアプリケーションに提供します。

* [`getMetadata:`](#getMeta) - AccessEnabler ライブラリによってメタデータとして公開された情報を取得します。

* [`presentTvProviderDialog:`](#presentTvDialog) - Apple SSO ダイアログを表示するようにアプリケーションに通知します。

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Apple SSO ダイアログを非表示にするようにアプリケーションに通知します。

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - [`getMetadata:`](#getMeta) 呼び出しでリクエストされたメタデータを配信します。

* [`sendTrackingData:forEventType:`](#sendTracking) - トラッキングデータ情報を配信します。

* [`MVPD`](#mvpd) - MVPD クラス。 [MVPD に関する情報が含まれます ]

### init:softwareStatement {#initWithSoftwareStatement}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler オブジェクトをインスタンス化します。 アプリケーション・インスタンスごとに 1 つの AccessEnabler インスタンスが必要です。

| **API 呼び出し：iOS AccessEnabler コンストラクター** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**提供：** v3.0 以降

**パラメーター：**

* **softwareStatement:** Adobeのシステム内のアプリケーションを識別する文字列。 ソフトウェア ステートメントの取得方法を確認してください。

[先頭に戻る…](#apis)



### init - [ 非推奨 ]{#init}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler オブジェクトをインスタンス化します。 アプリケーション・インスタンスごとに 1 つの AccessEnabler インスタンスが必要です。

| API 呼び出し：iOS AccessEnabler コンストラクター |
| --- |
| ```- (id) init;``` |

**提供期間：** v1.0 以降 **終了：** v3.0 まで

**パラメーター：** なし

[先頭に戻る…](#apis)

</br>

### setOptions：オプション {#setOptions}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** グローバル SDK オプションを設定します。 引数として NSDictionary を受け入れます。 ディクショナリの値は、SDK が行うすべてのネットワーク呼び出しと共にサーバーに渡されます。

**注意：** 値は、現在のフロー（認証/承認）とは無関係にサーバーに渡されます。 値を変更する場合は、このメソッドをいつでも呼び出すことができます。

| API 呼び出し：setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**提供：** v2.3.0 以降

**パラメーター：**

* *options*：グローバル SDK オプションを含む NSDictionary。 現在、次のオプションを使用できます。
   * **applicationProfile** – この値に基づいてサーバーを設定するために使用できます。
   * **visitorID** -Experience CloudID サービス。 この値は、後で高度な分析レポートに使用できます。
   * **handleSVC** - プログラマが SFSafariViewControllers を処理するかどうかを示すブール値。 詳しくは、iOS SDK 3.2 以降での [SFSafariViewController のサポート ](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) を参照してください。
      * **false** に設定すると、SDK によって自動的に SFSafariViewController がエンドユーザーに表示されます。 SDK は、MVPD のログインページの URL にさらに移動します。
      * **true** に設定すると、SDK によって SFSafariViewController が自動的にエンドユーザーに表示されます **表示されません**。 SDK によってさらにトリガーが行われます **navigate （toUrl:{url}, useSVC:YES）**。
* **device\_info** - [ クライアント情報の受け渡し ](/help/authentication/passing-client-information-device-connection-and-application.md) で説明されているクライアント情報。

[先頭に戻る…](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーの ID を確立します。 各プログラマーには、Adobe Pass Authentication System のAdobeに登録すると、一意の ID が割り当てられます。 SSO およびリモートトークンを処理する場合、アプリケーションがバックグラウンドの場合は認証状態を変更でき、アプリケーションがフォアグラウンドに移行された場合はシステム状態と同期するために setRequestor を再度呼び出すことができます（SSO が有効な場合はリモートトークンを取得、その間にログアウトが発生した場合はローカルトークンを削除）。

サーバー応答には、MVPD のリストと、プログラマーの ID に添付されたいくつかの設定情報が含まれています。 サーバ応答は、AccessEnabler コードによって内部的に使用されます。 `setRequestorComplete:` コールバックを介してアプリケーションに表示されるのは、操作のステータス（成功/失敗）のみです。

`urls` パラメーターを使用しない場合、結果として生成されるネットワーク呼び出しは、デフォルトのサービスプロバイダー URL （Adobe RELEASE/実稼動環境）をターゲットにします。


`urls` パラメーターに値を指定すると、結果として得られるネットワーク呼び出しは、`urls` パラメーターで指定されたすべての URL をターゲットにします。 すべての設定要求が、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする場合は、最初のレスポンダーが優先されます。 AccessEnabler は、リスト内の各 MVPD に対して、関連するサービス プロバイダの URL を記憶します。 以降のすべての使用権限リクエストは、設定段階でターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスター設定 |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**提供：** v3.0 以降

| API 呼び出し：リクエスター設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**提供：** v3.0 以降

**パラメーター：**

* *requestorID*：プログラマーに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録する際に、Adobeによって割り当てられた一意の ID をサイトに渡します。
* *urls*：オプションのパラメーターです。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと承認サービスのエンドポイントを指定できます（デバッグ目的で別のインスタンスを使用することもできます）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その場合、MVPD リストはすべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダー、つまり、最初に応答し、その MVPD をサポートするプロバイダーに関連付けられます。

>[!NOTE]
>
>`serviceProviders` パラメーターを指定せずに呼び出した場合、ライブラリはデフォルトのサービスプロバイダー（実稼動プロファイルの場合は `https://sp.auth.adobe.com`、ステージングプロファイルの場合は `https://sp.auth-staging.adobe.com`）から設定を取得します。 `serviceProviders` パラメーターを指定する場合、URL の配列である必要があります。設定情報は、指定したすべてのエンドポイントから取得され、結合されます。 異なるサービス プロバイダ応答に重複した情報が存在する場合、競合は最も応答の速いサーバー（つまり、応答時間が最も短いサーバーが優先されます）を優先して解決されます。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)

[先頭に戻る…](#apis)

</br>

### `setRequestor:setSignedRequestorId:`、`setRequestor:setSignedRequestorId:serviceProviders:` - [ 非推奨 ] {#setReq}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーの ID を確立します。 各プログラマーには、Adobe Pass Authentication System のAdobeに登録すると、一意の ID が割り当てられます。 SSO およびリモート・トークンを処理する場合、アプリケーションがバックグラウンドにある場合に認証状態が変更される可能性があります。アプリケーションがフォアグラウンドになると、システム状態と同期するために setRequestor を再度呼び出すことができます（SSO が有効な場合はリモート・トークンを取得し、その間にログアウトが発生した場合はローカル・トークンを削除します）。

サーバー応答には、MVPD のリストと、プログラマーの ID に添付されたいくつかの設定情報が含まれています。 サーバ応答は、AccessEnabler コードによって内部的に使用されます。 `setRequestorComplete:` コールバックを介してアプリケーションに表示されるのは、操作のステータス（成功/失敗）のみです。

`urls` パラメーターを使用しない場合、結果として生成されるネットワーク呼び出しは、デフォルトのサービスプロバイダー URL （Adobe RELEASE/実稼動環境）をターゲットにします。

`urls` パラメーターに値を指定すると、結果として得られるネットワーク呼び出しは、`urls` パラメーターで指定されたすべての URL をターゲットにします。 すべての設定要求が、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする場合は、最初のレスポンダーが優先されます。 AccessEnabler は、リスト内の各 MVPD に対して、関連するサービス プロバイダの URL を記憶します。 以降のすべての使用権限リクエストは、設定段階でターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスター設定 |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**提供期間：** v1.0 以降 **終了：** v3.0 まで

| API 呼び出し：リクエスター設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**提供期間：** v1.0 以降 **終了：** v3.0 まで

**パラメーター：**

* *requestorID*：プログラマーに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。
* *signedRequestorID*: **このパラメーターは、iOS AccessEnabler バージョン 1.2 以降に存在します。** 秘密鍵でデジタル署名された要求者 ID のコピー。<!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->。
* *urls*：オプションのパラメーターです。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと承認サービスのエンドポイントを指定できます（デバッグ目的で別のインスタンスを使用することもできます）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その場合、MVPD リストはすべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダー、つまり、最初に応答し、その MVPD をサポートするプロバイダーに関連付けられます。

**注意：** `serviceProviders` パラメーターを指定せずに呼び出した場合、ライブラリはデフォルトのサービスプロバイダー（実稼動プロファイルの場合は `https://sp.auth.adobe.com`、ステージングプロファイルの場合は `https://sp.auth-staging.adobe.com`）から設定を取得します。 `serviceProviders` パラメーターを指定する場合、URL の配列である必要があります。設定情報は、指定したすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダーの応答に重複する情報が存在する場合、競合は最も応答の速いサーバーを優先して解決されます（つまり、応答時間が最も短いサーバーが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)


[先頭に戻る…](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`、`setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [ 非推奨 ] {#setReq_tvos}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーの ID を確立します。 各プログラマーには、Adobe Pass Authentication System のAdobeに登録すると、一意の ID が割り当てられます。 この設定は、アプリケーションのライフサイクル中に 1 回だけ実行する必要があります。

サーバー応答には、MVPD のリストと、プログラマーの ID に添付されたいくつかの設定情報が含まれています。 サーバ応答は、AccessEnabler コードによって内部的に使用されます。 `setRequestorComplete:` コールバックを介してアプリケーションに表示されるのは、操作のステータス（成功/失敗）のみです。

`urls` パラメーターを使用しない場合、結果として生成されるネットワーク呼び出しは、デフォルトのサービスプロバイダー URL （Adobe RELEASE/実稼動環境）をターゲットにします。

`urls` パラメーターに値を指定すると、結果として得られるネットワーク呼び出しは、`urls` パラメーターで指定されたすべての URL をターゲットにします。 すべての設定要求が、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする場合は、最初のレスポンダーが優先されます。 AccessEnabler は、リスト内の各 MVPD に対して、関連するサービス プロバイダの URL を記憶します。 以降のすべての使用権限リクエストは、設定段階でターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：リクエスター設定</th>
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


**提供期間：** v2.0 以降 **終了：** v3.0 まで

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：リクエスター設定</th>
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

**提供期間：** v2.0 以降 **終了：** v3.0 まで

**パラメーター：**

* *requestorID*：プログラマーに関連付けられた一意の ID。 最初に、Adobeによって割り当てられた一意の ID をサイトに渡す   Adobe Pass Authentication サービスに登録されている。
* *signedRequestorID*: **このパラメーターはiOS AccessEnabler に存在します   バージョン 1.2 以降。** 秘密鍵でデジタル署名された要求者 ID のコピー。<!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->。
* *urls*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーです   が使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと承認サービスのエンドポイントを指定できます（デバッグ目的で別のインスタンスを使用することもできます）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その場合、MVPD リストはすべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダー、つまり、最初に応答し、その MVPD をサポートするプロバイダーに関連付けられます。
* secret と publicKey: 2 番目の画面呼び出しの署名に使用する秘密鍵と公開鍵。 詳しくは、[ クライアントレスドキュメント ](#create_dev) を参照してください。

`serviceProviders` パラメーターを指定せずに呼び出した場合、ライブラリはデフォルトのサービスプロバイダー（実稼動プロファイルの場合は `https://sp.auth.adobe.com`、ステージングプロファイルの場合はhttps://sp.auth-staging.adobe.com）から設定を取得します。 `serviceProviders` パラメーターを指定する場合、URL の配列である必要があります。設定情報は、指定したすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダーの応答に重複する情報が存在する場合、競合は最も応答の速いサーバーを優先して解決されます（つまり、応答時間が最も短いサーバーが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)

[先頭に戻る…](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnabler によってトリガーされるコールバックで、設定フェーズが完了したことをアプリケーションに通知します。 これは、アプリが使用権限リクエストの発行を開始できることを示すシグナルです。 設定フェーズが完了するまで、アプリケーションが使用権限リクエストを発行することはできません。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：リクエスターの設定完了</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**提供：** v1.0 以降

**パラメーター**:

* *ステータス*：次のいずれかの値を取ることができます。
   * `ACCESS_ENABLER_STATUS_SUCCESS` – 構成フェーズが正常に完了しました
   * `ACCESS_ENABLER_STATUS_ERROR` – 構成フェーズが失敗しました

**トリガー：**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[先頭に戻る…](#apis)

</br>

### checkAuthentication {#checkAuthN}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** 現在のユーザーの認証ステータスを確認します。
これを行うには、ローカルで有効な認証トークンを検索します
トークンのストレージスペース。 このメソッドはネットワーク呼び出しを実行しないので、メインスレッドで呼び出すことをお勧めします。
アプリケーションがユーザーの認証ステータスを問い合わせるために使用されます。
ui を適宜更新します（ログイン/ログアウト UI を更新します）。 この
次を介して認証ステータスがアプリケーションに通知されます
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバック。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証ステータスの確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

**パラメーター：** なし

**トリガーされたコールバック：**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[先頭に戻る…](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** 完全認証ワークフローを開始します。 まず、認証ステータスを確認します。 まだ認証されていない場合は、認証フロー state-machine が起動します。

* 最後の認証試行が成功した場合、MVPD は   選択フェーズはスキップされ、   [`navigateToUrl:`](#nav2url) コールバックがトリガーされます。 この   アプリケーションは、このコールバックを使用して、MVPD のログインページを持つユーザーを提示する WebView コントロールをインスタンス化します。 **[注意：Access Enabler 1.5 以降、SDK の制限により、この機能は使用できません ]。**
* 最後の認証が失敗した場合、またはユーザーが明示的にログアウトした場合、[`displayProviderDialog:`](#dispProvDialog) のコールバックは   トリガー。 アプリケーションは、このコールバックを使用して MVPD 選択 UI を表示します。 また、[`setSelectedProvider:`](#setSelProv) メソッドを使用してユーザーの MVPD 選択を AccessEnabler ライブラリに通知することにより、認証フローを再開する場合にも、アプリケーションが必要になります。

ユーザーの資格情報は MVPD のログインページで検証されるため、ユーザーが MVPD のログインページで認証されている間に行われる複数のリダイレクト操作をアプリケーションで監視する必要があります。 正しい資格情報を入力すると、WebView コントロールは、`ADOBEPASS_REDIRECT_URL` 定数によって定義されたカスタム URL にリダイレクトされます。 この URL は、WebView によって読み込まれることを意図していません。 アプリケーションはこの URL をインターセプトし、ログインフェーズが完了したことを示すシグナルとしてこのイベントを解釈する必要があります。 次に、認証フローを完了するために、AccessEnabler に制御を引き渡す必要があります（[handleExternalURL](#handleExternalURL) メソッドを呼び出します）。

最後に、[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバックを介して認証ステータスがアプリケーションに伝えられます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証フローを開始します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証フローを開始します</th>
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



**提供：** v1.9 以降

**パラメーター：**

* *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始する必要があるかどうかを指定するフラグ。
* *data*：有料テレビパスサービスに送信されるキーと値のペアで構成される辞書。 Adobeはこのデータを使用して、SDK を変更せずに将来の機能を有効にできます。

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:`、[`displayProviderDialog:`](#dispProvDialog)、`sendTrackingData:forEventType:`


[先頭に戻る…](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** 完全認証ワークフローを開始します。 まず、認証ステータスを確認します。 まだ認証されていない場合は、認証フロー state-machine が起動します。

* 現在の要求者に SSO をサポートする MVPD が 1 つ以上ある場合、[presentTvProviderDialog （） ](#presentTvDialog) が呼び出されます。 MVPD が SSO をサポートしていない場合、クラシック認証フローが開始され、フィルターパラメーターは無視されます。
* ユーザーが完了すると、Apple SSO フロー [`dismissTvProviderDialog()`](#dismissTvDialog) トリガーされ、認証プロセスが終了します。

最後に、[`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバックを介して認証ステータスがアプリケーションに伝えられます。

**提供：** v2.4 以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証フローを開始します</th>
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
<th>API 呼び出し：認証フローを開始します</th>
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

* *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始する必要があるかどうかを指定するフラグ。
* *data*：有料テレビパスサービスに送信されるキーと値のペアで構成される辞書。 Adobeはこのデータを使用して、SDK を変更せずに将来の機能を有効にできます。
* フィルター：Apple SSO ダイアログに表示される 2 つの MVPD ID のリストを持つ辞書。 SSO をサポートしていない MVPD は無視されますが、順序は尊重されます。 辞書には次の 2 つのキーが必要です。
   * TV\_PROVIDERS: ピッカーに表示されるすべての MVPD のリスト
   * FEATURED\_TV\_PROVIDERS: ピッカーでアイキャッチとしてマークする必要のあるすべての MVPD を含むリスト。 この一覧の MVPD は、TV\_PROVIDERS 一覧でも指定する必要があります。

**提供：** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証フローを開始します</th>
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
<th>API 呼び出し：認証フローを開始します</th>
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

* *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始する必要があるかどうかを指定するフラグ。
* *data*：有料テレビパスサービスに送信されるキーと値のペアで構成される辞書。 Adobeはこのデータを使用して、SDK を変更せずに将来の機能を有効にできます。
* フィルター：Apple SSO ダイアログに表示される MVPD ID のリスト。 SSO をサポートしていない MVPD は無視されますが、順序は尊重されます。

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[先頭に戻る…](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnabler によってトリガーされるコールバックは、適切な UI 要素をインスタンス化して、ユーザーが目的の MVPD を選択できるようにする必要があることをアプリケーションに通知します。 コールバックは、MVPD オブジェクトのリストに、選択 UI パネルを正しく構築するのに役立つ追加情報（MVPD のロゴを指す URL、わかりやすい表示名など）を提供します。

ユーザが所望の MVPD を選択したら、上位レイヤのアプリケーションは、`setSelectedProvider:` を呼び出して、ユーザの選択に対応する MVPD の ID を渡すことにより、認証フローを再開する必要がある。

**認証フローの中止** - ユーザーが「戻る」ボタンを押せる点です。これは、認証フローの中止と同じです。 このシナリオでは、アプリケーションは [setSelectedProvider:](#setSelProv) メソッドを呼び出し、null をパラメータとして渡して、AccessEnabler に認証状態マシンをリセットできるようにする必要があります。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：MVPD 選択 UI の表示</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**提供：** v1.0 以降

**パラメーター**:

* *mvpds*: MVPD 選択 UI 要素を構築するためにアプリケーションが使用できる、MVPD 関連の情報を保持する MVPD オブジェクトのリスト。

**トリガー：** `getAuthentication`、[`getAuthentication:withData:`](#getAuthN)、`getAuthorization:`、[`getAuthorization:withData:`](#getAuthZ)


[先頭に戻る…](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、ユーザーの MVPD 選択をアクセス イネーブラに通知するためにアプリケーションによって呼び出されます。 アプリケーションはこの方法を使用して、認証に使用するサービスプロバイダーを選択または変更できます。

選択した MVPD が TempPass MVPD の場合、後で getAuthentication （）を呼び出すことなく、その MVPD で自動的に認証されます。

getAuthentication （） メソッドに追加のパラメーターが指定されている場合、プロモーションの一時パスでは使用できないことに注意してください。

パラメータとして *null* を渡した場合、アクセス イネーブラは、ユーザーが認証フローをキャンセルした（「戻る」ボタンを押した）と見なし、認証の状態マシンをリセットし、`AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` エラーコードの [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバックを呼び出して応答します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[先頭に戻る…](#apis)

</br>

#### navigateToUrl: {#nav2url}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** AccessEnabler によってトリガーされるコールバック。アプリケーションに UIWebView/WKWebView コントローラーのインスタンス化を要求し、コールバックの **`url`** パラメーターで指定された URL を読み込みます。 コールバックは、認証エンドポイントの URL またはログアウトエンドポイントの URL を表す **`url`** パラメーターを渡します。

UIWebView/WKWebView` `controller は複数のリダイレクトを実行するので、アプリケーションはコントローラのアクティビティを監視し、`ADOBEPASS_REDIRECT_URL ` 定数（`adobepass://ios.app` など）で定義された特定のカスタム URL を読み込む瞬間を検出する必要があります。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 認証またはログアウトの流れが完了し、コントローラを安全に閉じられるというシグナルとしてアプリケーションが解釈する必要があります。 コントローラがこの特定のカスタム URL を読み込むとき、アプリケーションは UIWebView/WKWebView を閉じて、AccessEnabler の `handleExternalURL:url `API メソッドを呼び出す必要があります。

**注：** 認証フローの場合、認証フローの中止に相当する「戻る」ボタンを押せる点に注意してください。 このようなシナリオでは、**`nil`** をパラメーターとして渡し、AccessEnabler に認証状態マシンをリセットする機会を与えるために、アプリケーションで [setSelectedProvider:](#setSelProv) メソッドを呼び出す必要があります。

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

**提供：** v1.0 以降

**パラメーター**:

* *url*:MVPD のログインページを指す URL

**トリガー：** [setSelectedProvider:](#setSelProv)



[先頭に戻る…](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** アプリケーションで以前に [setOptions （\[&quot;handleSVC&quot;:true&quot;\]）呼び出しを使用して Safari ビューコントローラー（SVC）の手動処理が有効になり、MVPD で Safari ビューコントローラー（SVC）が必要な場合のみ、`navigateToUrl:` コールバックの代わりに AccessEnabler によってトリガーされたコールバック ](#setOptions)。 他のすべての MVPD では、`navigateToUrl:` コールバックが呼び出されます。 Safari ビューコントローラー（SVC）の管理方法について詳しくは、iOS SDK 3.2 以降 ](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) での [SFSafariViewController のサポートを参照してください。

`navigateToUrl:` コールバックと同様に、`navigateToUrl:useSVC:` は AccessEnabler によってトリガーされ、アプリケーションに `SFSafariViewController` コントローラのインスタンス化と、コールバックの **`url`** パラメータで指定された URL のロードを要求します。 コールバックは、認証エンドポイントの URL またはログアウトエンドポイントの URL を表す **`url`** パラメーターと、アプリケーションが `SFSafariViewController` を使用する必要があることを指定する **`useSVC`** パラメーターを渡します。

`SFSafariViewController` コントローラは複数のリダイレクトを実行するので、アプリケーションはコントローラのアクティビティを監視し、`application's custom scheme` ーザーが定義した特定のカスタム URL を読み込む瞬間を検出する必要があります（例：** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 認証またはログアウトの流れが完了し、コントローラを安全に閉じられるというシグナルとしてアプリケーションが解釈する必要があります。 コントローラがこの特定のカスタム URL を読み込むとき、アプリケーションは `SFSafariViewController` を閉じて AccessEnabler の `handleExternalURL:url `API メソッドを呼び出す必要があります。

**注：** 認証フローの場合、認証フローの中止に相当する「戻る」ボタンを押せる点に注意してください。 このようなシナリオでは、**`nil`** をパラメーターとして渡し、AccessEnabler に認証状態マシンをリセットする機会を与えるために、アプリケーションで [setSelectedProvider:](#setSelProv) メソッドを呼び出す必要があります。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：SFSafariViewController で MVPD ログイン ページを表示する</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**提供：**v 3.2 以降

**パラメーター**:

* *url:* MVPD のログインページを指す URL
* *useSVC:* SFSafariViewController で URL を読み込む必要があるかどうかを示します。

**setSelectedProvider:](#setOptions) より前の [**[ setOptions:](#setSelProv) によってトリガー

[先頭に戻る…](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、認証またはログアウトのフローを完了するためにアプリケーションによって呼び出されます。 このメソッドは、`UIWebView/WKWebView or SFSafariViewController` コントローラーが特定のカスタム URL にリダイレクトされる瞬間をアプリケーションが検出した直後に呼び出す必要があります。 アプリケーションで `SFSafariViewController `controller を使用する必要がある場合、特定のカスタム URL はお使いの `application's custom scheme` で定義されます（例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。そうでない場合、この特定のカスタム URL は `ADOBEPASS_REDIRECT_URL ` 定数で定義されます（例：`adobepass://ios.app`）。

認証フローの場合、AccessEnabler はバックエンド・サーバから認証トークンを取得し、そのトークンをトークン・ストレージにローカルに保存することによって、フローを完了します。 AccessEnabler は、ステータス・コードが 1 の `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> コールバックを呼び出して認証フローが完了したことをアプリケーションに通知し、成功を示します。 これらの手順の実行中にエラーが発生した場合、`setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> コールバックは、認証失敗を示すステータスコード 0 と対応するエラーコードでトリガーされます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証またはログアウトのフローを完了する</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v3.0 以降

**パラメーター：**

* *url*:` UIWebView/WKWebView or SFSafariViewController ` コントロールからインターセプトされた URL を文字列として指定します。


**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[先頭に戻る…](#apis)

</br>

#### getAuthenticationToken - [ 非推奨 ] {#getAuthNToken}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** バックエンドサーバーに認証トークンをリクエストして、認証フローを完了します。 このメソッドは、MVPD ログインページをホストする WebView コントロールが `ADOBEPASS_REDIRECT_URL` 定数によって定義されるカスタム URL にリダイレクトされるイベントに対する応答としてのみ、アプリケーションで呼び出す必要があります。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証トークンの取得</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**提供期間：** v1.0 以降 **終了：** v3.0 まで

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[先頭に戻る…](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 認証フローのステータスをアプリケーションに通知する AccessEnabler によってトリガーされるコールバック。 ユーザーのインタラクションの結果として、または他の予期しないシナリオ（ネットワーク接続の問題など）が原因で、このフローが失敗する可能性がある場所は多くあります。 このコールバックは、認証フローの成功/失敗ステータスをアプリケーションに通知すると同時に、必要に応じて失敗の理由に関する追加情報も提供します。

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


**提供：** v1.0 以降

**パラメーター**:

* *ステータス*：次のいずれかの値を取ることができます。
   * `ACCESS_ENABLER_STATUS_SUCCESS` – 認証フローが正常に完了しました
   * `ACCESS_ENABLER_STATUS_ERROR` – 認証フローが失敗しました
* *コード*：失敗の理由。 *status* が `ACCESS_ENABLER_STATUS_SUCCESS` の場合、*code* は空の文字列です（つまり、`USER_AUTHENTICATED` 定数によって定義されます）。 エラーが発生した場合、このパラメーターには次のいずれかの値を指定できます。
   * `USER_NOT_AUTHENTICATED_ERROR` - ユーザーが認証されていません。 ローカルトークンキャッシュに有効な認証トークンがない場合の [checkAuthentication:](#checkAuthN) メソッド呼び出しに応答して。
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler が       上位層アプリケーション後の認証状態マシン       [`setSelectedProvider:`](#setSelProv) に *null* を渡して、認証フローを中止します。  ユーザーが認証フローをキャンセルした（つまり、「戻る」ボタンを押した）と考えられます。
   * `GENERIC_AUTHENTICATION_ERROR` - ネットワークが利用できないか、ユーザーが認証フローを明示的に取り消したなどの理由により、認証フローが失敗しました。

**トリガー：** `checkAuthentication`、`getAuthentication`、[`getAuthentication:withData:`](#getAuthN)、`checkAuthorization:`、[`checkAuthorization:withData:`](#checkAuthZ)

[先頭に戻る…](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、保護された特定のリソースを表示する権限がユーザーに既にあるかどうかを判断するためにアプリケーションで使用されます。 このメソッドの主な目的は、UI **の修飾に使用する情報を取得することです（例えば、ロックアイコンとロック解除アイコンでアクセスステータスを示すなど**。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.3 以降

**パラメーター：**

* *resources:* 認証をチェックする必要があるリソースの配列。 リストの各要素は、リソース ID を表す文字列である必要があります。 この     リソース ID には、呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマーと MVPD またはメディア RSS フラグメントとの間で確立された合意値である必要があります。

**コールバックがトリガーされました：** [`preauthorizedResources:`](#preauthResources)

[先頭に戻る…](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、保護された特定のリソースを表示する権限がユーザーに既にあるかどうかを判断するためにアプリケーションで使用されます。 このメソッドの主な目的は、UI の修飾に使用する情報を取得することです（例えば、ロックアイコンとロック解除アイコンでアクセスステータスを示すなど）。 **cache** パラメーターは、リソースの解決に内部キャッシュを使用するかどうかを制御します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**提供：** v3.1 以降



**パラメーター：**

* *resources:* 認証をチェックする必要があるリソースの配列。 リストの各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、`getAuthorization:` 呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマと MVPD またはメディア RSS フラグメントとの間で確立された合意値である必要があります。
* *cache:* リソースを解決するために内部キャッシュを使用するかどうかを指定するブール値。 false の場合、キャッシュがバイパスされ、この API が呼び出されるたびにサーバーが呼び出されます。

**コールバックがトリガーされました：** [`preauthorizedResources:`](#preauthResources)

[先頭に戻る…](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** `checkPreauthorizedResources:` によってトリガーされたコールバック。 ユーザーが既に表示を許可されているリソースのリストを提供します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.3 以降

**パラメーター：**

* `resources`：ユーザーが既に表示を許可されているリソースの配列。

**Trigger by:** [`checkPreauthorizedResources:`](#checkPreauth)



[先頭に戻る…](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、承認ステータスを確認するためにアプリケーションで使用されます。 まず、認証ステータスを最初に確認します。 認証されていない場合、[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) コールバックがトリガーされ、メソッドが終了します。 認証されたユーザーは、認証フローもトリガーします。 [`getAuthorization:`](#getAuthZ) メソッドの詳細を参照してください。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証ステータスの確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証ステータスの確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.9 以降

**パラメーター：**

* *resource*：ユーザーが認証をリクエストするリソースの ID。
* *data*：有料テレビパスサービスに送信されるキーと値のペアで構成される辞書。 Adobeはこのデータを使用して、SDK を変更せずに将来の機能を有効にできます。

**トリガーされたコールバック：**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[先頭に戻る…](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、承認フローを開始するためにアプリケーションによって使用されます。 ユーザーがまだ認証されていない場合は、認証フローも開始します。 ユーザーが認証されると、AccessEnabler は認証トークンの要求（有効な認証トークンがローカル・トークン・キャッシュに存在しない場合）と、短期間有効なメディア・トークンの要求を発行します。 短いメディアトークンが取得されると、認証フローは完了と見なされます。 [`setToken:forResource:`](#setToken) コールバックがトリガーされ、短いメディアトークンがパラメーターとしてアプリケーションに配信されます。 何らかの理由で認証が失敗した場合、[`tokenRequestFailed:forEventType:`](#tokenReqFailed) コールバックがトリガーされ、エラーコード/詳細が提供されます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証フローの開始</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証フローの開始</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**提供：** v1.9 以降

**パラメーター：**

* *resource*：ユーザーが認証をリクエストするリソースの ID。
* *data*：有料テレビパスサービスに送信されるキーと値のペアで構成される辞書。 Adobeはこのデータを使用して、SDK を変更せずに将来の機能を有効にできます。

**コールバックがトリガーされました：** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**追加のコールバックがトリガーされました：**\
このメソッドは、次のコールバックもトリガーします（認証フローも開始される場合）:`setAuthenticationStatus:errorCode:`、`displayProviderDialog:`

>[!NOTE]
>
>可能な限り、`getAuthorization:` / `checkAuthorization:withData:` ではなく `checkAuthorization:` / `getAuthorization:withData:` を使用してください。 `getAuthorization:` / `getAuthorization:withData:` メソッドは、（ユーザーが認証されていない場合は）完全な認証フローを開始し、これにより、プログラマー側で複雑な実装が行われる可能性があります。

[先頭に戻る…](#apis)

</br>

### `setToken:forResource:` {#setToken}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 認証フローが正常に完了したことをアプリケーションに通知する AccessEnabler によってトリガーされるコールバック。 短時間のみ有効なメディアトークンもパラメーターとして配信されます。


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

**提供：** v1.0 以降

**パラメーター**:

* *token*：短時間のみ有効なメディアトークン
* *resource*：認証を取得したリソース

**トリガー：** [`checkAuthorization:`](#checkAuthZ)、[`checkAuthorization:withData:`](#checkAuthZ)、[`getAuthorization:`](#getAuthZ)、[`getAuthorization:withData:`](#getAuthZ)

[先頭に戻る…](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 認証フローが失敗したことを上位層アプリケーションに通知する AccessEnabler によってトリガーされるコールバック。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：認証フローが失敗しました</th>
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

**提供：** v1.0 以降

**パラメーター**:

* *resource*：認証を取得したリソース。
* *コード*：失敗シナリオに関連付けられたエラーコード。 使用可能な値：
   * `USER_NOT_AUTHORIZED_ERROR` - ユーザーは承認できませんでした
指定されたリソースに対して
* *説明*：失敗シナリオに関する追加情報。 何らかの理由でこの記述文字列が使用できない場合、Adobe Pass認証は空の文字列 **（&quot;）** を送信します。\
  この文字列は、MVPD がカスタム エラーメッセージやセールス関連のメッセージを渡すために使用できます。 例えば、サブスクライバーがリソースの認証を拒否された場合、MVPD は次のようなメッセージを送信する場合があります。「現在、パッケージ内のこのチャネルにはアクセスできません。 パッケージをアップグレードする場合は、**ここ** をクリックします。 メッセージは、このコールバックを通じてAdobe Pass認証によってプログラマーに渡され、プログラマーは表示または無視のオプションを持ちます。 Adobe Pass認証では、このパラメーターを使用して、エラーを引き起こした可能性のある条件の通知を提供することもできます。 例えば、「プロバイダーの認証サービスと通信する際にネットワークエラーが発生しました」などです。

**トリガー：** `checkAuthorization:`、[`checkAuthorization:withData:`](#checkAuthZ)、`getAuthorization:`、[`getAuthorization:withData:`](#getAuthZ)

[先頭に戻る…](#apis)

</br>

### ログアウト {#logout}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、ログアウトフローを開始するためにアプリケーションによって呼び出されます。 ログアウトは、ユーザーがAdobe Pass認証サーバーと MVPD の両方のサーバーからログアウトする必要があるという事実による、一連の HTTP リダイレクト操作の結果です。 AccessEnabler ライブラリによって発行された単純な HTTP 要求では、このフローを完了できないため、HTTP リダイレクト操作に従うには、`UIWebView/WKWebView or SFSafariViewController` コントローラをインスタンス化する必要があります。

ログアウトフローは、認証フローとは異なり、ユーザーは `UIWebView/WKWebView or SFSafariViewController` ントロールコントローラーとやり取りする必要がありません。 そのため、Adobeはログアウトプロセス中にコントロールを非表示（つまり非表示）にすることをお勧めします。

認証フローと同様のパターンを採用する。 iOS AccessEnabler は、`navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` をトリガーして、`UIWebView/WKWebView or SFSafariViewController` コントローラを作成し、コールバックの `url` パラメータで指定された URL をロードします。 これは、バックエンドサーバー上のログアウトエンドポイントの URL です。 tvOS AccessEnabler の場合、`navigateToUrl:` コールバックも `navigateToUrl:useSVC:` コールバックも呼び出されません。

複数のリダイレクトを実行する場合、アプリケーションは `UIWebView/WKWebView or SFSafariViewController `controller のアクティビティを監視し、特定のカスタム URL を読み込む瞬間を検出する必要があります。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 これは、ログアウトフローが完了し、コントローラを安全に閉じられるというシグナルとして、アプリケーションによってのみ解釈される必要があります。 コントローラがこの特定のカスタム URL を読み込むとき、アプリケーションはコントローラを閉じて AccessEnabler の `handleExternalURL:url `API メソッドを呼び出す必要があります。 アプリケーションで `SFSafariViewController `controller を使用する必要がある場合、特定のカスタム URL はお使いの `application's custom scheme` で定義されます（例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。そうでない場合、この特定のカスタム URL は `ADOBEPASS_REDIRECT_URL ` 定数で定義されます（例：`adobepass://ios.app`）。

最後に、AccessEnabler はステータス コードが 0 の [`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、ログアウト フローの成功を示します。

**注意：** ユーザーがApple SSO を使用してログインしている場合、VSA203 ステータスがトリガーされます。 その場合は、システム設定からもログアウトするようにユーザーに指示する必要があります。 これをおこなわないと、アプリケーションが再起動される際に再認証が行われます。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：ログアウトフローの開始</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `navigateToUrl:`、[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[先頭に戻る…](#apis)

</br>

### getSelectedProvider {#getSelProv}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドを使用して、現在選択されているプロバイダを確認します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されている MVPD を特定します</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** [`selectedProvider:`](#selProv)

[先頭に戻る…](#apis)

</br>

### selectedProvider {#selProv}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 現在選択されている MVPD に関する情報をアプリケーションに配信する AccessEnabler によってトリガーされるコールバック。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：現在選択されている MVPD に関する情報</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

**パラメーター**:

* *mvpd*：現在選択されている MVPD に関する情報を含むオブジェクト

**Trigger by:** [`getSelectedProvider`](#getSelProv)

[先頭に戻る…](#apis)

</br>

### getMetadata: {#getMeta}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler ライブラリによってメタデータとして公開された情報を取得するには、このメソッドを使用します。 アプリケーションは、辞書ベースの *キー* 入力パラメーターを提供することで、このデータにアクセスできます。

プログラマーが使用できるメタデータには次の 2 種類があります。

* 静的メタデータ（認証トークン TTL、認証トークン TTL およびデバイス ID）
* ユーザーメタデータ（ユーザー ID や郵便番号などのユーザー固有の情報。認証および承認フロー中に MVPD からユーザーのデバイスに渡すことができます）

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：AccessEnabler でメタデータをクエリーする</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**提供：** v1.0 以降

**パラメーター：**

* *keyDictionary*：以下を含むディクショナリデータ構造
形式：
   * key が `METADATA_OPCODE_KEY` で value が `METADATA_AUTHENTICATION` の場合、クエリが実行されて認証トークンの有効期限が取得されます。
   * key が `METADATA_OPCODE_KEY` で value が `METADATA_AUTHORIZATION` **and** の場合\
     キーは `METADATA_RESOURCE_ID_KEY` で、値は特定のリソース ID です。次に、指定されたリソースに関連付けられた認証トークンの有効期限を取得するためにクエリが実行されます。
   * key が `METADATA_OPCODE_KEY` で value が `METADATA_DEVICE_ID` の場合は、現在のデバイス ID を取得するためにクエリが実行されます。 この機能はデフォルトで無効になっており、プログラマーはイネーブルメントと料金についてAdobeに問い合わせる必要があります。
   * key が `METADATA_OPCODE_KEY`、value が `METADATA_USER_META` **、key が `METADATA_USER_META_KEY`** value がメタデータの名前の場合、ユーザーメタデータに対するクエリが実行されます。 使用可能なユーザーメタデータタイプのリストを以下に示します。
      * `zip` – 郵便番号のリスト
      * `householdID` – 世帯の識別子。 MVPD がサブアカウントをサポートしていない場合、これは `userID` と同じです。
      * `maxRating` - ユーザーの保護者による制限の上限のコレクション
      * `userID` - ユーザー ID。 MVPD がサブ勘定科目をサポートし、ユーザーがメイン勘定科目でない場合、`userID` は `householdID.` とは異なります
      * `channelID` - ユーザーが表示できるチャネルのリスト。

  >[!NOTE]
  >
  >プログラマーが使用できる実際のユーザーメタデータは、MVPD が提供する内容によって異なります。 このリストは、新しいメタデータが利用可能になり、Adobe Pass認証システムに追加されると、展開されます。

**コールバックがトリガーされました：** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**詳細情報：**[ ユーザーメタデータ ](/help/authentication/user-metadata.md)

[先頭に戻る…](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 現在のリクエスターが SSO サポートの MVPD を 1 つ以上サポートしている場合、[getAuthentication （）を呼び出した後に AccessEnabler によってトリガーされるコールバック ](#getAuthN)

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

**提供：** v2.0 以降

**パラメーター**:

* viewController: Appleの SSO ダイアログを表します。 この viewController を画面に表示する必要があります。

**Trigger by:** [`getAuthentication`](#getAuthN)

**詳細情報：** [iOS/tvOS のシングルサインオン ](#presentTvDialog)

[先頭に戻る…](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** Appleの SSO ダイアログを閉じた後に AccessEnabler によってトリガーされるコールバック。

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

**提供：** v2.0 以降

**パラメーター**:

* viewController: Appleの SSO ダイアログを表します。 この viewController を画面から削除する必要があります。

**トリガー：** ユーザーアクション

**詳細情報：** [iOS/tvOS のシングルサインオン ](#presentTvDialog)

[先頭に戻る…](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** [`getMetadata:`](#getMeta) 呼び出し経由で要求されたメタデータを配信する AccessEnabler によってトリガーされるコールバック。

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

**提供：** v1.0 以降

**パラメーター**:

* *メタデータ*：リクエストされたメタデータ。 静的メタデータ（認証 TTL、認証 TTL、デバイス ID）の場合、この値は `NSString` です。  ユーザー固有のメタデータをリクエストする場合は、複雑なオブジェクトになります。 この複合オブジェクトは、通常、JSON ペイロードの Objective-C 表現です（例：「{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;buildings&quot;: [&quot;150&quot;, &quot;320&quot;]}」は、Objective-C で NSDictionary （&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;buildings&quot; -> NSArray （&quot;150&quot;, &quot;320&quot;））として変換されます）。   サンプルメタデータ JSON オブジェクト：

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

* *encrypted*：取得したメタデータが暗号化されているかどうかを指定するブール値。 このパラメーターは、ユーザーメタデータリクエストでのみ重要で、常に暗号化されずに受信される静的メタデータ（認証 TTL など）には意味を持ちません。 このパラメーターを true に設定した場合、許可リストの秘密鍵（[`setRequestor:setSignedRequestorId:`](#setReq) および `setRequestor:setSignedRequestorId:serviceProviders: ` 呼び出しの要求者 ID の署名に使用されるのと同じ秘密鍵）を使用して RSA 復号化を実行し、暗号化されていないユーザーメタデータ値を取得するかどうかは、プログラマー次第です。

* *key*：メタデータ取得リクエストの作成に使用するキー。

* *arguments*:[`getMetadata:`](#getMeta) 呼び出しに渡されたのと同じディクショナリ。 これは、アプリケーションがリクエストを応答と照合できるようにするために提供されます。

**Trigger by:** [`getMetadata:`](#getMeta)

**詳細情報：**[ ユーザーメタデータ ](/help/authentication/user-metadata.md)


[先頭に戻る…](#apis)

</br>

### MVPD {#mvpd}

**ファイル：** AccessEnabler/headers/model/MVPD.h

**説明** MVPD オブジェクトについて説明します。 MVPD のプロパティーに関する情報を取得するために使用できます。

**可用性：** v1.0 以降 [boardingStatus プロパティは v2.2 から使用できます ]

**プロパティ**:

* （NSString） ID - MVPD Id。
* （NSString） displayName - MVPD 名。 [ ピッカーに表示する場合に使用します ]
* （NSString） logoURL - MVPD ロゴのアドレス。
* （BOOL） enablePlatformServices - true の場合、MVPD は、[Apple SSO](#presentTvDialog) などの SSO サービスをサポートします。
* （NSString） boardingStatus：次の 3 つの値を指定できます。
   * nil - MVPD はApple SSO をサポートしていません。
   * ピッカー – MVPD はApple ピッカーに表示されますが、認証フローはAdobeで行われます。
   * サポート対象 – MVPD はAppleで完全にサポートされ、Appleの SSO トークンを使用します。

[先頭に戻る…](#apis)

</br>

## 追跡イベント {#tracking}

AccessEnabler は、エンタイトルメント フローに必ずしも関連しない追加のコールバックをトリガーします。 [`sendTrackingData()`](#sendTracking) コールバック関数の実装はオプションですが、この関数を使用すると、アプリケーションは特定のイベントを追跡し、認証/承認の成功/失敗の試行回数などの統計をコンパイルすることができます。

### sendTrackingData:forEventType: {#sendTracking}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnabler によってトリガーされるコールバックは、認証/承認フローの完了/失敗など、さまざまなイベントの発生をアプリケーションに通知します。 Adobe Pass Authentication 1.6 では、デバイスの種類、AccessEnabler クライアントの種類、およびオペレーティング システムが [`sendTrackingData()`](#sendTracking) によって報告されます。 [`sendTrackingData()`](#sendTracking) コールバックは、後方互換性を維持します。

**コールバック：トラッキングイベント**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**提供：** v1.0 以降

**メモ：** デバイスタイプとオペレーティングシステムは、パブリック Java ライブラリ（<http://java.net/projects/user-agent-utils>）とユーザーエージェント文字列を使用して取得されます。 この情報は、操作指標をデバイスカテゴリに分類する粗い方法としてのみ提供されますが、Adobeは誤った結果に対して責任を負うことはありません。 それに応じて新しい機能を使用してください。

* デバイスタイプに指定可能な値：
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* AccessEnabler クライアント・タイプの使用可能な値：
   * `flash`
   * `html5`
   * `ios`
   * `android`


**パラメーター**:

* *event*：トラッキングされるイベントのコード。 次の 3 つのトラッキングイベントタイプが考えられます。
   * **authorizationDetection:** 認証トークンリクエストが返されるたびに発生します（イベントは `TRACKING_AUTHORIZATION`）。
   * **authenticationDetection:** 認証チェックが発生するたびに（イベントは `TRACKING_AUTHENTICATION` です）
   * **mvpdSelection:** ユーザーが MVPD 選択フォームで MVPD を選択したとき（イベントは `TRACKING_GET_SELECTED_PROVIDER`）
* *data*：レポートされたイベントに関連付けられている追加データ。 このデータは、値のリストの形式で表示されます。

**トリガー：** `checkAuthentication`、`getAuthentication`、[`getAuthentication:withData:`](#getAuthN)、`checkAuthorization:`、[`checkAuthorization:withData:`](#checkAuthZ)、`getAuthorization:`、[`getAuthorization:withData:`](#getAuthZ)、`setSelectedProvider:`

*data* 配列の値を解釈する手順：

* trackingEventType の場合 `TRACKING_AUTHENTICATION:`
   * **0** - トークンリクエストが成功したかどうか（true/false）と成功した場合は次の値：
   * **1** - MVPD ID 文字列
   * **2** - GUID （md5 ハッシュ化）
   * **3** - トークンは既にキャッシュにあります（true/false）
   * **4** - デバイスタイプ
   * **5** - AccessEnabler クライアント タイプ
   * **6** - オペレーティングシステムの種類

* trackingEventType の場合 `TRACKING_AUTHORIZATION:`
   * **0** - トークンリクエストが成功したかどうか（true/false）と成功した場合は次の値：
   * **1** - MVPD ID
   * **2** - GUID （md5 ハッシュ化）
   * **3** - トークンは既にキャッシュにあります（true/false）
   * **4** - エラー
   * **5** – 詳細
   * **6** - デバイスタイプ
   * **7** - AccessEnabler クライアント タイプ
   * **8** - オペレーティングシステムの種類
* trackingEventType の場合 `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** – 現在選択されている MVPD の ID
   * **1** - デバイスタイプ
   * **2** - AccessEnabler クライアント タイプ
   * **3** - オペレーティングシステムの種類

</br>
