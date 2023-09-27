---
title: iOS/tvOS API リファレンス
description: iOS/tvOS API リファレンス
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '7010'
ht-degree: 0%

---

# iOS/tvOS SDK API リファレンス {#iostvos-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#intro}

このページでは、iOS/tvOS のネイティブクライアントでAdobe Pass認証用に公開されるメソッドとコールバック関数について説明します。 ここで説明するメソッドとコールバック関数は、 `AccessEnabler.h` および `EntitlementDelegate.h` ヘッダーファイル。iOS AccessEnabler SDK の次の場所にあります。 `[SDK directory]/AccessEnabler/headers/api/`


関連ドキュメント：

* 基本的なAdobe Pass認証の権限付与フローについて詳しくは、 [権利付与フロー](/help/authentication/entitlement-flow.md).
* この API を使用してAdobe Pass認証の使用権限付与フローを実装する手順については、 [iOS統合クックブック](/help/authentication/iostvos-sdk-cookbook.md).
* 最新のiOS AccessEnabler SDK については、 [iOS Native Access Enabler Library](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobeでは、Adobe Pass認証のみを使用することを推奨します。 *公開* API:
>
>* パブリック API は、サポートされるすべてのクライアントタイプで使用でき、完全にテストされています。 公開機能の場合は、各クライアントタイプに対応するバージョンの関連メソッドがあることを確認します。
>* 後方互換性をサポートし、パートナーの統合が中断しないようにするには、パブリック API は、できる限り安定している必要があります。 ただし、非公開 API の場合、アドビは、将来の任意の時点で署名を変更する権利を保有します。 現在のパブリックAdobe Pass認証 API 呼び出しの組み合わせを通じてサポートできない特定のフローが発生した場合の最善の方法は、お知らせすることです。 アドビでは、お客様のニーズを考慮に入れて、パブリック API を変更し、今後も安定したソリューションを提供できます。

</br>

## API リファレンス {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - AccessEnabler オブジェクトをインスタンス化します。

* **[非推奨]** [init](#init) - AccessEnabler オブジェクトをインスタンス化します。

* [setOptions:options:](#setOptions)  — プロファイルや visitorID などのグローバル SDK オプションを設定します。

* [setRequestor:](#setReqV3)[requestorID](#setReqV3),[setRequestor:requestorID:serviceProviders:](#setReqV3)  — プログラマーの ID を確立します。

* **[非推奨]** [setRequestor:signedRequestorId:](#setReq),[setRequestor:signedRequestorId:serviceProviders:](#setReq)  — プログラマーの ID を確立します。

* **[非推奨]** [setRequestor:signedRequestorId:secret:publicKey](#setReq_tvos), [setRequestor:signedRequestorId:serviceProviders:secret:publicKey](#setReq_tvos) — プログラマーの ID を確立します。

* [setRequestorComplete:](#setReqComplete)  — 設定フェーズが完了したことをアプリケーションに通知します。

* [checkAuthentication](#checkAuthN)  — 現在のユーザーの認証状態を確認します。

* [getAuthentication](#getAuthN), [getAuthentication:withData:](#getAuthN)  — 完全な認証ワークフローを開始します。

* [getAuthentication:filter](#getAuthN_filter),[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter)  — 完全な認証ワークフローを開始します。

* [displayProviderDialog:](#dispProvDialog)  — ユーザーが MVPD を選択するための適切な UI 要素をインスタンス化するように、アプリケーションに通知します。

* [setSelectedProvider:](#setSelProv) - AccessEnabler にユーザーの MVPD 選択を通知します。

* [navigateToUrl:](#nav2url)  — ユーザーに MVPD ログインページを表示する必要があることをアプリケーションに通知します。

* [navigateToUrl:useSVC:](#nav2urlSVC) - SFSafariViewController を使用して、ユーザーに MVPD ログインページを提示する必要があることをアプリケーションに通知します

* [handleExternalURL:url](#handleExternalURL)  — 認証/ログアウトのフローを完了します。

* **[非推奨]** [getAuthenticationToken](#getAuthNToken)  — バックエンドサーバーから認証トークンをリクエストします。

* [setAuthenticationStatus:errorCode:](#setAuthNStatus) ：アプリケーションに認証フローのステータスを通知します。

* [checkPreauthorizedResources:](#checkPreauth)  — 保護された特定のリソースを表示する権限がユーザーに既にあるかどうかを判断します。

* [checkPreauthorizedResources:cache:](#checkPreauthCache)  — 保護された特定のリソースを表示する権限がユーザーに既にあるかどうかを判断します。

* [preauthorizedResources:](#preauthResources)  — ユーザーが既に表示を許可しているリソースのリストを提供します。

* [checkAuthorization:](#checkAuthZ), [checkAuthorization:withData:](#checkAuthZ)  — 現在のユーザーの認証ステータスを確認します。

* [getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)  — 承認フローを開始します。

* [setToken:forResource:](#setToken)  — 承認フローが正常に完了したことをアプリケーションに通知します。

* [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed)  — 承認フローが失敗したことをアプリケーションに通知します。

* [ログアウト](#logout)  — ログアウトフローを開始します。

* [getSelectedProvider](#getSelProv)  — 現在選択されているプロバイダーを決定します。

* [selectedProvider:](#selProv)  — 現在選択されている MVPD に関する情報をアプリケーションに配信します。

* [getMetadata:](#getMeta) - AccessEnabler ライブラリによってメタデータとして公開された情報を取得します。

* [presentTvProviderDialog:](#presentTvDialog)  — アプリケーションにApple SSO ダイアログを表示するように通知します。

* [dismissTvProviderDialog:](#dismissTvDialog)  — アプリケーションにApple SSO ダイアログを非表示にするように通知します。

* [setMetadataStatus:encrypted:forKey:andArguments:](#setMetaStatus)  — がリクエストしたメタデータを配信します [getMetadata:](#getMeta) を呼び出します。

* [sendTrackingData:forEventType:](#sendTracking)  — トラッキングデータ情報を配信します。

* [MVPD](#mvpd) - MVPD クラス。 [MVPD に関する情報を含む]

### init:softwareStatement {#initWithSoftwareStatement}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに 1 つの AccessEnabler インスタンスが必要です。

| **API 呼び出し： iOS AccessEnabler コンストラクタ** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**可用性：** v3.0 以降

**パラメーター：**

* **softwareStatement:** アプリケーションのシステム内のAdobeを識別する文字列。 ソフトウェアステートメントの取得方法を確認します。

[トップに戻る…](#apis)



### init - [非推奨]{#init}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに 1 つの AccessEnabler インスタンスが必要です。

| API 呼び出し： iOS AccessEnabler コンストラクタ |
| --- |
| ```- (id) init;``` |

**可用性：** v1.0 以降 **次まで：** v3.0

**パラメーター：** なし

[トップに戻る…](#apis)

</br>

### setOptions:options {#setOptions}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** グローバル SDK オプションを設定します。 NSDictionary を引数として受け付けます。 辞書の値は、SDK がおこなうすべてのネットワーク呼び出しと共に、サーバーに渡されます。

**注意：** 値は、現在のフロー（認証/承認）とは無関係に、サーバーに渡されます。 値を変更する場合は、このメソッドをいつでも呼び出すことができます。

| API 呼び出し： setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**可用性：** v2.3.0 以降

**パラメーター：**

* *options*：グローバル SDK オプションを含む NSDictionary。 現在、次のオプションを使用できます。
   * **applicationProfile**  — この値に基づいてサーバーを設定するために使用できます。
   * **visitorID** -Experience CloudID サービス。 この値は、後で高度な分析レポートに使用できます。
   * **handleSVC**  — プログラマが SFSafariViewControllers を処理するかどうかを示すブール値。 詳しくは、 [iOS SDK 3.2 以降での SFSafariViewController のサポート](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) を参照してください。
      * に設定した場合、 **false** SDK は、SFSafariViewController を使用してエンドユーザーに自動的に提示します。 SDK は、さらに MVPDs ログインページの URL に移動します。
      * に設定した場合、 **真、** SDK は以下を実行します。 **NOT** SFSafariViewController を使用してエンドユーザーを自動的に提示する。 SDK は、さらにトリガー **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info**  — クライアント情報 ( [クライアント情報を渡す](/help/authentication/passing-client-information-device-connection-and-application.md).

[トップに戻る…](#apis)


### setRequestor:requestorID, setRequestor:requestorID:serviceProviders: {#setReqV3}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーの ID を確立します。 各プログラマーは、Adobe Pass認証システムのAdobeに登録する際に、一意の ID を割り当てられます。 SSO とリモートトークンを扱う場合、アプリケーションがバックグラウンドにあるときに認証状態が変わり、システム状態と同期するために、アプリケーションがフォアグラウンドに移行したときに setRequestor を再度呼び出すことができます（SSO が有効な場合はリモートトークンを取得し、ログアウト中にログアウトが発生した場合は削除）。

サーバ応答には、MVPD のリストと、プログラマの ID に付加されるいくつかの設定情報が含まれます。 サーバの応答は、AccessEnabler コードによって内部的に使用されます。 操作のステータス (SUCCESS/FAIL) のみが、 `setRequestorComplete:` コールバック。

次の場合、 `urls` パラメーターを使用しない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダー URL(AdobeRELEASE/実稼動環境 ) をターゲットにします。


値が `urls` パラメーターを渡すと、結果のネットワーク呼び出しでは、 `urls` パラメーター。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする際には、最初の応答が優先されます。 リスト内の各 MVPD に対して、AccessEnabler は関連するサービスプロバイダの URL を記憶します。 以降のすべてのエンタイトルメントリクエストは、設定フェーズでターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスト元の設定 |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**可用性：** v3.0 以降

| API 呼び出し：リクエスト元の設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**可用性：** v3.0 以降

**パラメーター：**

* *requestorID*：プログラマーに関連付けられた一意の ID。 最初にAdobe Pass Authentication サービスを登録する際に、Adobeによって割り当てられた一意の ID をサイトに渡します。
* *url*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます (http://sp.auth.adobe.com/)。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で異なるインスタンスが使用される場合があります）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その際、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。

>[!NOTE]
>
>を指定せずに呼び出した場合、 `serviceProviders` パラメーターを渡す場合、ライブラリはデフォルトのサービスプロバイダー ( `https://sp.auth.adobe.com` （実稼動プロファイルの場合）または `https://sp.auth-staging.adobe.com` （ステージングプロファイル用）。 次の場合、 `serviceProviders` パラメーターを指定する場合は、URL の配列である必要があります。設定情報は、指定されたすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダ応答に重複した情報が存在する場合、競合は解決され、最も速い応答サーバが優先されます（つまり、応答時間が最も短いサーバが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)

[トップに戻る…](#apis)

</br>

### setRequestor:setSignedRequestorId:, setRequestor:setSignedRequestorId:serviceProviders: - [非推奨] {#setReq}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーの ID を確立します。 各プログラマーは、Adobe Pass認証システムのAdobeに登録する際に、一意の ID を割り当てられます。 SSO とリモートトークンを扱う場合、アプリケーションがバックグラウンドになっているときに認証状態が変わる可能性があります。システム状態と同期するために、アプリケーションがフォアグラウンドに移行したときに setRequestor を再び呼び出すことができます。

サーバ応答には、MVPD のリストと、プログラマの ID に付加されるいくつかの設定情報が含まれます。 サーバの応答は、AccessEnabler コードによって内部的に使用されます。 操作のステータス (SUCCESS/FAIL) のみが、 `setRequestorComplete:` コールバック。

次の場合、 `urls` パラメーターを使用しない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダー URL(AdobeRELEASE/実稼動環境 ) をターゲットにします。

値が `urls` パラメーターを渡すと、結果のネットワーク呼び出しでは、 `urls` パラメーター。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする際には、最初の応答が優先されます。 リスト内の各 MVPD に対して、AccessEnabler は関連するサービスプロバイダの URL を記憶します。 以降のすべてのエンタイトルメントリクエストは、設定フェーズでターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスト元の設定 |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**可用性：** v1.0 以降 **次まで：** v3.0

| API 呼び出し：リクエスト元の設定 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**可用性：** v1.0 以降 **次まで：** v3.0

**パラメーター：**

* *requestorID*：プログラマーに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。
* *signedRequestorID*: **このパラメータは、iOS AccessEnabler バージョン 1.2 以降に存在します。** 秘密鍵でデジタル署名された要求者 ID のコピー。 <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます (http://sp.auth.adobe.com/)。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で異なるインスタンスが使用される場合があります）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その際、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。

**メモ：** を指定せずに呼び出した場合、 `serviceProviders` パラメーターを渡す場合、ライブラリはデフォルトのサービスプロバイダー (`https://sp.auth.adobe.com` （実稼動プロファイルの場合）または `https://sp.auth-staging.adobe.com` （ステージングプロファイル用）。 次の場合、 `serviceProviders` パラメーターを指定する場合は、URL の配列である必要があります。設定情報は、指定されたすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダ応答に重複した情報が存在する場合、競合は解決され、最も速い応答サーバが優先されます（つまり、応答時間が最も短いサーバが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)


[トップに戻る…](#apis)

### setRequestor:setSignedRequestorId:secret:publicKey, setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey - [非推奨] {#setReq_tvos}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** プログラマーの ID を確立します。 各プログラマーは、Adobe Pass認証システムのAdobeに登録する際に、一意の ID を割り当てられます。 この設定は、アプリケーションのライフサイクルの間に 1 回だけ実行する必要があります。

サーバ応答には、MVPD のリストと、プログラマの ID に付加されるいくつかの設定情報が含まれます。 サーバの応答は、AccessEnabler コードによって内部的に使用されます。 操作のステータス (SUCCESS/FAIL) のみが、 `setRequestorComplete:` コールバック。

次の場合、 `urls` パラメーターを使用しない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダー URL(AdobeRELEASE/実稼動環境 ) をターゲットにします。

値が `urls` パラメーターを渡すと、結果のネットワーク呼び出しでは、 `urls` パラメーター。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする際には、最初の応答が優先されます。 リスト内の各 MVPD に対して、AccessEnabler は関連するサービスプロバイダの URL を記憶します。 以降のすべてのエンタイトルメントリクエストは、設定フェーズでターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：リクエスト元の設定</th>
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


**可用性：** v2.0 以降 **次まで：** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：リクエスト元の設定</th>
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

**可用性：** v2.0 以降 **次まで：** v3.0

**パラメーター：**

* *requestorID*：プログラマーに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。
* *signedRequestorID*: **このパラメータは、iOS AccessEnabler バージョン 1.2 以降に存在します。** 秘密鍵でデジタル署名された要求者 ID のコピー。 <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます (http://sp.auth.adobe.com/)。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で異なるインスタンスが使用される場合があります）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その際、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。
* secret と publicKey:2 回目の画面呼び出しへの署名に使用する秘密鍵と公開鍵。 詳しくは、 [クライアントレスドキュメント](#create_dev).

を指定せずに呼び出した場合、 `serviceProviders` パラメーターを渡す場合、ライブラリはデフォルトのサービスプロバイダーから設定を取得します ( `https://sp.auth.adobe.com` 実稼動プロファイルの場合はhttps://sp.auth-staging.adobe.com 、ステージングプロファイルの場合は )。 次の場合、 `serviceProviders` パラメーターを指定する場合は、URL の配列である必要があります。設定情報は、指定されたすべてのエンドポイントから取得され、結合されます。 異なるサービスプロバイダ応答に重複した情報が存在する場合、競合は解決され、最も速い応答サーバが優先されます（つまり、応答時間が最も短いサーバが優先されます）。

**コールバックがトリガーされました：** [`setRequestorComplete:`](#setReqComplete)

[トップに戻る…](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 構成フェーズが完了したことをアプリケーションに通知する AccessEnabler によってトリガーされるコールバック。 これは、アプリがエンタイトルメントリクエストの発行を開始できることを示すシグナルです。 設定フェーズが完了するまで、アプリケーションはエンタイトルメントリクエストを発行できません。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：リクエスト元の設定が完了しました</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0 以降

**パラメーター**:

* *ステータス*：次のいずれかの値を取ることができます。
   * `ACCESS_ENABLER_STATUS_SUCCESS`  — 構成フェーズが正常に完了しました
   * `ACCESS_ENABLER_STATUS_ERROR`  — 構成フェーズが失敗しました

**トリガー元：**
`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[トップに戻る…](#apis)

</br>

### checkAuthentication {#checkAuthN}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** 現在のユーザーの認証状態を確認します。
これをおこなうには、ローカルトークンストレージスペースで有効な認証トークンを検索します。 このメソッドを呼び出しても、ネットワーク呼び出しは実行されません。 アプリケーションは、ユーザーの認証状態を問い合わせ、それに応じて UI を更新するために使用します（つまり、ログイン/ログアウト UI を更新します）。 認証ステータスは、 [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) コールバック。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証状態の確認</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[トップに戻る…](#apis)

</br>

### getAuthentication, getAuthentication:withData: {#getAuthN}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** 完全な認証ワークフローを開始します。 まず、認証状態を確認します。 認証済みでない場合は、認証フロー状態マシンが起動します。

* 最後の認証が成功した場合は、MVPD の選択フェーズがスキップされ、 [`navigateToUrl:`](#nav2url) コールバックがトリガーされます。 このコールバックを使用して、MVPD のログインページをユーザーに表示する WebView コントロールをインスタンス化します。 **[注： Access Enabler 1.5 の時点では、SDK の制限により、この機能は使用できません].**
* 最後の認証が失敗した場合、またはユーザーが明示的にログアウトした場合、 [`displayProviderDialog:`](#dispProvDialog) コールバックがトリガーされます。 アプリケーションは、このコールバックを使用して MVPD selection UI を表示します。 また、AccessEnabler ライブラリに対して、 [`setSelectedProvider:`](#setSelProv) メソッド。

ユーザーの資格情報が MVPD ログインページで検証されるので、ユーザーが MVPD のログインページで認証を行う際に実行される複数のリダイレクト操作を監視するには、アプリケーションが必要です。 正しい資格情報が入力されると、WebView コントロールは、 `ADOBEPASS_REDIRECT_URL` 定数。 この URL は、WebView によって読み込まれるものではありません。 アプリケーションは、この URL を切断し、このイベントをログインフェーズが完了したシグナルとして解釈する必要があります。 次に、認証フローを完了するために、AccessEnabler に制御を渡す必要があります ( [handleExternalURL](#handleExternalURL) メソッド )。

最後に、認証ステータスは、 [setAuthenticationStatus:errorCode:](#setAuthNStatus) コールバック。

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

**可用性：** v1.0 以降

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



**可用性：** v1.9 以降

**パラメーター：**

* *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
* *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される辞書。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。

**コールバックがトリガーされました：** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[トップに戻る…](#apis)

</br>

### getAuthentication:filter, getAuthentication:withData:andFilter {#getAuthN_filter}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** 完全な認証ワークフローを開始します。 まず、認証状態を確認します。 認証済みでない場合は、認証フロー状態マシンが起動します。

* [presentTvProviderDialog()](#presentTvDialog) 現在のリクエスト元に SSO をサポートする MVPD が 1 つ以上ある場合に呼び出されます。 MVPD が SSO をサポートしていない場合は、従来の認証フローが開始され、フィルタパラメータは無視されます。
* ユーザーがApple SSO フローを完了した後 [dismissTvProviderDialog()](#dismissTvDialog) がトリガーされ、認証プロセスが完了します。

最後に、認証ステータスは、 [setAuthenticationStatus:errorCode:](#setAuthNStatus) コールバック。

**可用性：** v2.4 以降

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

* *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
* *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される辞書。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。
* filter: Apple SSO ダイアログに表示する MVPD id の 2 つのリストを含む辞書。 SSO をサポートしていない MVPD は無視されますが、順序は考慮されます。 辞書には 2 つのキーが必要です。
   * TV\_PROVIDERS：ピッカーに表示されるすべての MVPD を含むリスト
   * おすすめ\_TV\_PROVIDERS：ピッカーで特集としてマークする必要があるすべての MVPD を含むリスト。 このリストの MVPD は、TV\_PROVIDERS リストでも指定する必要があります。

**可用性：** v2.0 - v2.3.1


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

* *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
* *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される辞書。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。
* filter: Apple SSO ダイアログに表示する MVPD id のリストです。 SSO をサポートしていない MVPD は無視されますが、順序は考慮されます。

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[トップに戻る…](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnabler によってトリガーされるコールバック。ユーザーが目的の MVPD を選択できるように、適切な UI 要素をインスタンス化する必要があることをアプリケーションに通知します。 このコールバックは、選択 UI パネルを正しく構築するのに役立つ追加情報と共に、MVPD オブジェクトのリストを提供します（MVPD のロゴを示す URL、わかりやすい表示名など）。

ユーザーが目的の MVPD を選択したら、上位レイヤーアプリケーションは、 `setSelectedProvider:` ユーザーの選択に対応する MVPD の ID を渡す。

**認証フローの中止**  — これは、ユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このシナリオでは、アプリケーションは [setSelectedProvider:](#setSelProv) メソッドで null をパラメータとして渡し、AccessEnabler に認証状態マシンをリセットする機会を与えます。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック： MVPD selection UI を表示します。</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0 以降

**パラメーター**:

* *mvpds*:MVPD 選択 UI 要素の構築にアプリケーションが使用できる MVPD 関連の情報を保持する MVPD オブジェクトのリスト。

**トリガー元：** ` getAuthentication, `[getAuthentication:withData:](#getAuthN),` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[トップに戻る…](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、ユーザーの MVPD 選択を Access Enabler に通知するために、アプリケーションによって呼び出されます。 アプリケーションは、このメソッドを使用して、認証に使用するサービスプロバイダーを選択または変更できます。

選択した MVPD が TempPass MVPD の場合、後で getAuthentication() を呼び出す必要なく、その MVPD で自動的に認証されます。

getAuthentication() メソッドで追加のパラメータが指定されるプロモーション一時パスでは、これは使用できないことに注意してください。

を渡す場合 *null* パラメータとして、Access Enabler は、ユーザーが認証フローをキャンセルした（つまり「戻る」ボタンを押した）と仮定し、認証状態マシンをリセットし、 [setAuthenticationStatus:errorCode:](#setAuthNStatus) コールバック `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` エラーコード。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[トップに戻る…](#apis)

</br>

#### navigateToUrl: {#nav2url}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** AccessEnabler によってトリガーされ、アプリケーションに対して UIWebView/WKWebView コントローラーをインスタンス化し、コールバックの **`url`** パラメーター。 このコールバックは、 **`url`** 認証エンドポイントの URL またはログアウトエンドポイントの URL を表すパラメーター。

UIWebView/WKWebView として` `コントローラは、複数のリダイレクトを経由します。アプリケーションは、コントローラのアクティビティを監視し、 `ADOBEPASS_REDIRECT_URL `定数 ( `adobepass://ios.app`) をクリックします。 この特定のカスタム URL は実際には無効で、コントローラが実際に読み込むことを意図していないことに注意してください。 認証フローまたはログアウトフローが完了し、コントローラを安全に閉じられることを示すシグナルとして、アプリケーションでのみ解釈する必要があります。 コントローラがこの特定のカスタム URL を読み込むとき、アプリケーションは UIWebView/WKWebView を閉じて、AccessEnabler の `handleExternalURL:url `API メソッド。

**注意：** 認証フローの場合、これは、ユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このシナリオでは、アプリケーションは [setSelectedProvider:](#setSelProv) メソッドの受け渡し **`nil`** をパラメータとして追加し、AccessEnabler が認証状態マシンをリセットする機会を与える。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック： MVPD ログインページを表示します。</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0 以降

**パラメーター**:

* *url*:MVPD のログインページを指す URL

**トリガー元：** [setSelectedProvider:](#setSelProv)



[トップに戻る…](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** AccessEnabler ではなく、 `navigateToUrl:` アプリケーションが以前に手動で Safari ビューコントローラ (SVC) を有効にしていた場合に備え、 [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) を呼び出し、Safari ビューコントローラ (SVC) を必要とする MVPD の場合にのみ呼び出します。 他のすべての MVPD に対して、 `navigateToUrl:` コールバックが呼び出されます。 詳しくは、[iOS SDK 3.2 以降での SFSafariViewController のサポート](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) Safari View Controller(SVC) の管理方法の詳細。

次に類似 `navigateToUrl:` コールバック `navigateToUrl:useSVC:` を呼び出し、AccessEnabler によってアプリケーションにインスタンス化を要求する `SFSafariViewController` コントローラー、およびコールバックの **`url`** パラメーター。 このコールバックは、 **`url`** 認証エンドポイントの URL またはログアウトエンドポイントの URL を表すパラメーター、および **`useSVC`** アプリケーションが `SFSafariViewController`.

を `SFSafariViewController` コントローラは、複数のリダイレクトを経由します。アプリケーションは、コントローラのアクティビティを監視し、お客様の `application's custom scheme` ( 例：** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`) をクリックします。 この特定のカスタム URL は実際には無効で、コントローラが実際に読み込むことを意図していないことに注意してください。 認証フローまたはログアウトフローが完了し、コントローラを安全に閉じられることを示すシグナルとして、アプリケーションでのみ解釈する必要があります。 コントローラーがこの特定のカスタム URL を読み込むと、アプリケーションは `SFSafariViewController` AccessEnabler の `handleExternalURL:url `API メソッド。

**注意：** 認証フローの場合、これは、ユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このシナリオでは、アプリケーションは [setSelectedProvider:](#setSelProv) メソッドの受け渡し **`nil`** をパラメータとして追加し、AccessEnabler が認証状態マシンをリセットする機会を与える。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック： SFSafariViewController で MVPD ログインページを表示します。</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**利用可能：**v 3.2 以降

**パラメーター**:

* *url:* MVPD のログインページを示す URL
* *useSVC:* url を SFSafariViewController に読み込むかどうか。

**トリガー元：**[ setOptions:](#setOptions) 前 [setSelectedProvider:](#setSelProv)

[トップに戻る…](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、認証フローまたはログアウトフローを完了するために、アプリケーションによって呼び出されます。 このメソッドは、アプリケーションが `UIWebView/WKWebView or SFSafariViewController` コントローラが特定のカスタム URL にリダイレクトされる。 アプリケーションで `SFSafariViewController `コントローラー：指定したカスタム URL は、 `application's custom scheme` ( 例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`) が含まれていない場合、この特定のカスタム URL は `ADOBEPASS_REDIRECT_URL `定数 ( `adobepass://ios.app`) をクリックします。

認証フローが発生した場合、AccessEnabler は、バックエンドサーバから認証トークンを取得し、トークンストレージにローカルに保存することで、フローを完了します。 AccessEnabler は、 `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> ステータスコードが 1 のコールバック。成功を示します。 これらの手順の実行中にエラーが発生した場合、 `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> コールバックは、ステータスコード 0 でトリガーされ、認証の失敗を示し、対応するエラーコードも示します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：認証またはログアウトフローの完了</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v3.0 以降

**パラメーター：**

* *url*：から傍受された URL ` UIWebView/WKWebView or SFSafariViewController ` 文字列として制御します。


**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[トップに戻る…](#apis)

</br>

#### getAuthenticationToken - [非推奨] {#getAuthNToken}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** バックエンドサーバーから認証トークンをリクエストして、認証フローを完了します。 このメソッドは、MVPD ログインページをホストする WebView コントロールが、 `ADOBEPASS_REDIRECT_URL` 定数。


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

**可用性：** v1.0 以降 **次まで：** v3.0

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[トップに戻る…](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 認証フローの状態をアプリケーションに通知する AccessEnabler によってトリガーされるコールバック。 ユーザーの操作や、その他の予期しないシナリオ（ネットワーク接続の問題など）の結果、このフローが失敗する場所は多数あります。 このコールバックは、アプリケーションに認証フローの成功/失敗ステータスを通知し、必要に応じて失敗理由に関する追加情報も提供します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：認証フローのステータスのレポート</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0 以降

**パラメーター**:

* *ステータス*：次のいずれかの値を取ることができます。
   * `ACCESS_ENABLER_STATUS_SUCCESS`  — 認証フローが正常に完了しました
   * `ACCESS_ENABLER_STATUS_ERROR`  — 認証フローに失敗しました
* *コード*：エラーの理由。 次の場合 *ステータス* 次に該当 `ACCESS_ENABLER_STATUS_SUCCESS`を、 *コード* が空の文字列 ( `USER_AUTHENTICATED` 定数 ) です。 失敗した場合、このパラメーターは次のいずれかの値を取ることができます。
   * `USER_NOT_AUTHENTICATED_ERROR` ：ユーザーは認証されていません。 に応じて、 [checkAuthentication:](#checkAuthN) ローカルトークンキャッシュに有効な認証トークンがない場合にメソッドを呼び出します。
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler は、上位レイヤ・アプリケーションが渡された後に認証状態マシンをリセットしました。 *null* から [`setSelectedProvider:`](#setSelProv) 認証フローを中止する。  おそらく、ユーザーが認証フローをキャンセルした（つまり、「戻る」ボタンを押した）と思われます。
   * `GENERIC_AUTHENTICATION_ERROR`  — ネットワークが利用できない、またはユーザーが認証フローを明示的にキャンセルしたなどの理由で、認証フローが失敗しました。

**トリガー元：** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN),` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[トップに戻る…](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、ユーザーが既に特定の保護されたリソースの表示を許可されているかどうかを判断するために、アプリケーションで使用されます。 このメソッドの主な目的は、UI の装飾に使用する情報を取得することです **（例えば、ロックアイコンとロック解除アイコンを使用したアクセスステータスを示す）。**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.3 以降

**パラメーター：**

* *リソース：* 承認を確認する必要があるリソースの配列。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID は、呼び出しのリソース ID と同じ制限の対象となります。つまり、プログラマーと MVPD またはメディア RSS フラグメントとの間で確立された値に合意されている必要があります。

**コールバックがトリガーされました：** [`preauthorizedResources:`](#preauthResources)

[トップに戻る…](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、ユーザーが既に特定の保護されたリソースの表示を許可されているかどうかを判断するために、アプリケーションで使用されます。 このメソッドの主な目的は、UI の修飾に使用する情報（例えば、ロックアイコンとロック解除アイコンによるアクセスステータスを示す情報）を取得することです。 The **キャッシュ** パラメータは、リソースの解決に内部キャッシュを使用するかどうかを制御します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**可用性：** v3.1 以降



**パラメーター：**

* *リソース：* 承認を確認する必要があるリソースの配列。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、 `getAuthorization:` 呼び出し、つまり、プログラマと MVPD またはメディア RSS フラグメントとの間に確立された値に合意する必要があります。
* *キャッシュ：* リソースの解決に内部キャッシュを使用するかどうかを指定するブール値。 false の場合、キャッシュはバイパスされ、この API が呼び出されるたびにサーバー呼び出しがおこなわれます。

**コールバックがトリガーされました：** [`preauthorizedResources:`](#preauthResources)

[トップに戻る…](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明：** 次によってトリガーされるコールバック： `checkPreauthorizedResources:`. ユーザーが既に表示を許可しているリソースのリストを提供します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されているプロバイダーを設定</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.3 以降

**パラメーター：**

* `resources`：ユーザーが既に表示する権限を持つリソースの配列。

**トリガー元：** [`checkPreauthorizedResources:`](#checkPreauth)



[トップに戻る…](#apis)

</br>

### checkAuthorization:, checkAuthorization:withData: {#checkAuthZ}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、アプリケーションが認証ステータスを確認するために使用します。 まず、認証状態を最初に確認します。 認証されていない場合、 [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) コールバックがトリガーされ、メソッドが終了します。 ユーザーが認証されると、認証フローもトリガーされます。 詳細は、 [`getAuthorization:`](#getAuthZ) メソッド。


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

**可用性：** v1.0 以降

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

**可用性：** v1.9 以降

**パラメーター：**

* *リソース*：ユーザーが認証を要求するリソースの ID。
* *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される辞書。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。

**コールバックがトリガーされました：**
[tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[トップに戻る…](#apis)

</br>

### getAuthorization:, getAuthorization:withData: {#getAuthZ}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、アプリケーションで承認フローを開始するために使用されます。 ユーザーがまだ認証されていない場合は、認証フローも開始します。 ユーザーが認証されると、AccessEnabler は認証トークン（有効な認証トークンがローカルトークンキャッシュに存在しない場合）と短時間有効なメディアトークンの要求を発行します。 ショートメディアトークンが取得されると、認証フローは完了と見なされます。 The [setToken:forResource:](#setToken) コールバックがトリガーされ、ショートメディアトークンがアプリケーションのパラメーターとして配信されます。 何らかの理由で認証が失敗した場合、 [tokenRequestFailed:forEventType:](#tokenReqFailed) コールバックがトリガーされ、エラーコード/詳細が表示されます。

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

**可用性：** v1.0 以降

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



**可用性：** v1.9 以降

**パラメーター：**

* *リソース*：ユーザーが認証を要求するリソースの ID。
* *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される辞書。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。

**コールバックがトリガーされました：** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**追加のコールバックがトリガーされました：**\
このメソッドは、次のコールバックをトリガーすることもできます（認証フローも開始する場合）。 `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

**注意： checkAuthorization: / checkAuthorization を使用してください。:withData: getAuthorization の代わりに： / getAuthorization:withData: 可能な限り getAuthorization: / getAuthorization:withData: メソッドは完全な認証フローを開始し（ユーザが認証されていない場合）、その結果、プログラマ側で複雑な実装が行われる可能性があります。**

[トップに戻る…](#apis)

</br>

### setToken:forResource: {#setToken}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 承認フローが正常に完了したことをアプリケーションに通知する、AccessEnabler によってトリガーされるコールバック。 短時間のみ有効なメディアトークンは、パラメーターとしても提供されます。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：承認フローが正常に完了しました</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0 以降

**パラメーター**:

* *トークン*：短時間のみ有効なメディアトークン
* *リソース*：認証が取得されたリソース

**トリガー元：** [checkAuthorization:](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ),` `[getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)

[トップに戻る…](#apis)

</br>

### tokenRequestFailed:errorCode:errorDescription: {#tokenReqFailed}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 認証フローが失敗したことを上位レイヤ・アプリケーションに通知する、AccessEnabler によってトリガーされるコールバック。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>コールバック：承認フローに失敗しました</th>
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

**可用性：** v1.0 以降

**パラメーター**:

* *リソース*：認証が取得されたリソース。
* *コード*：失敗シナリオに関連付けられたエラーコードです。 可能な値：
   * `USER_NOT_AUTHORIZED_ERROR`  — ユーザーは指定されたリソースを承認できませんでした
* *説明*：失敗シナリオに関する追加の詳細。 何らかの理由でこの説明文字列を使用できない場合、Adobe Pass Authentication は空の文字列を送信します **(&quot;&quot;)**.\
  この文字列は、MVPD がカスタムエラーメッセージや販売関連メッセージを渡すために使用できます。 例えば、あるリソースに対する承認が購読者によって拒否された場合、MVPD は次のようなメッセージを送信します。「現在、パッケージ内のこのチャネルにアクセスできません。 パッケージをアップグレードする場合は、 **ここ**.&quot; メッセージは、このコールバックを通じてAdobe Pass Authentication によってプログラマーに渡され、プログラマーは、メッセージを表示または無視するオプションを持ちます。 Adobe Pass認証では、このパラメーターを使用して、エラーの原因となった可能性のある条件を通知することもできます。 例えば、「プロバイダーの認証サービスとの通信中にネットワークエラーが発生しました」などです。

**トリガー元：** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[トップに戻る…](#apis)

</br>

### ログアウト {#logout}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドは、アプリケーションによって呼び出され、ログアウトフローを開始します。 ログアウトは、一連の HTTP リダイレクト操作の結果です。これは、ユーザーがAdobe Pass認証サーバーと MVPD サーバーの両方からログアウトする必要があるためです。 このフローは、AccessEnabler ライブラリによって発行された単純な HTTP リクエストでは完了できないため、 `UIWebView/WKWebView or SFSafariViewController` HTTP リダイレクト操作に従うには、コントローラをインスタンス化する必要があります。

ログアウトフローは、ユーザーが `UIWebView/WKWebView or SFSafariViewController`  コントローラーを使用します。 したがって、Adobeでは、ログアウトプロセス中にコントロールを非表示（非表示）にすることをお勧めします。

認証フローと同様のパターンを用いる。 iOS AccessEnabler は、 `navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` を作成するには、 `UIWebView/WKWebView or SFSafariViewController` コントローラー、およびコールバックの `url` パラメーター。 これは、バックエンドサーバー上のログアウトエンドポイントの URL です。 tvOS AccessEnabler の場合、 `navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` コールバックが呼び出されます。

複数のリダイレクトを経る場合、アプリケーションは、 `UIWebView/WKWebView or SFSafariViewController `コントローラーを使用し、特定のカスタム URL を読み込むときのタイミングを検出します。 この特定のカスタム URL は実際には無効で、コントローラが実際に読み込むことを意図していないことに注意してください。 ログアウトフローが完了し、コントローラを安全に閉じられることを示すシグナルとして、アプリケーションでのみ解釈する必要があります。 コントローラがこの特定のカスタム URL を読み込むと、アプリケーションはコントローラを閉じて、AccessEnabler の `handleExternalURL:url `API メソッド。 アプリケーションで `SFSafariViewController `コントローラー：指定したカスタム URL は、 `application's custom scheme` ( 例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`) が含まれていない場合、この特定のカスタム URL は `ADOBEPASS_REDIRECT_URL `定数 ( `adobepass://ios.app`) をクリックします。

最後に、AccessEnabler が [`setAuthenticationStatus()`](#setAuthNStatus) ステータスコード 0 のコールバック。ログアウトフローの成功を示します。

**注意：** ユーザーがApple SSO を使用してログインすると、VSA203 ステータスがトリガーされます。 この場合、ユーザーに対して、システム設定からログアウトするように指示する必要があります。 そうしないと、アプリケーションが再起動されたときに再認証がおこなわれます。


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

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[トップに戻る…](#apis)

</br>

### getSelectedProvider {#getSelProv}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** このメソッドを使用して、現在選択されているプロバイダーを特定します。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：現在選択されている MVPD を特定します。</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** [`selectedProvider:`](#selProv)

[トップに戻る…](#apis)

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

**可用性：** v1.0 以降

**パラメーター**:

* *mvpd*：現在選択されている MVPD に関する情報を含むオブジェクト

**トリガー元：** [`getSelectedProvider`](#getSelProv)

[トップに戻る…](#apis)

</br>

### getMetadata: {#getMeta}

**ファイル：** AccessEnabler/headers/AccessEnabler.h

**説明：** AccessEnabler ライブラリによってメタデータとして公開された情報を取得するには、このメソッドを使用します。 アプリケーションは、辞書ベースの *key* 入力パラメーター。

プログラマーは、次の 2 種類のメタデータを使用できます。

* 静的メタデータ（認証トークン TTL、認証トークン TTL およびデバイス ID）
* ユーザーメタデータ（ユーザー ID、郵便番号などのユーザー固有の情報。認証および承認フロー中に MVPD からユーザーのデバイスに渡すことができます）

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API 呼び出し：メタデータの AccessEnabler に対するクエリ</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0 以降

**パラメーター：**

* *keyDictionary*：次の形式のディクショナリデータ構造。
   * キーが `METADATA_OPCODE_KEY` 値は `METADATA_AUTHENTICATION`に設定された場合、認証トークンの有効期限を取得するためのクエリが実行されます。
   * キーが `METADATA_OPCODE_KEY` 値は `METADATA_AUTHORIZATION` **および**\
     キーは `METADATA_RESOURCE_ID_KEY` の値が特定のリソース ID の場合、クエリが実行され、指定したリソースに関連付けられた認証トークンの有効期限が取得されます。
   * キーが `METADATA_OPCODE_KEY` 値は `METADATA_DEVICE_ID`をクエリして、現在のデバイス ID を取得します。 この機能はデフォルトで無効になっています。有効化と料金については、Adobeにお問い合わせください。
   * キーが `METADATA_OPCODE_KEY` 値は `METADATA_USER_META` **および** キーは `METADATA_USER_META_KEY` 値はメタデータの名前です。その後、ユーザーのメタデータに対してクエリが実行されます。 使用可能なユーザーメタデータタイプのリスト：
      * `zip`  — 郵便番号のリスト
      * `householdID`  — 世帯識別子。 MVPD がサブアカウントをサポートしない場合、これは `userID`.
      * `maxRating`  — ユーザーの親の格付けの最大数のコレクション
      * `userID`  — ユーザー識別子。 MVPD がサブアカウントをサポートし、ユーザがメインアカウントではない場合、 `userID` 次とは異なる `householdID.`
      * `channelID`  — ユーザーが表示する権限を持つチャネルのリスト。

  >[!NOTE]
  >
  >プログラマーが利用できる実際のユーザメタデータは、MVPD が利用できるものによって異なります。 新しいメタデータが使用可能になり、Adobe Pass Authentication システムに追加されると、このリストは展開されます。

**コールバックがトリガーされました：** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**詳細情報：** [ユーザーメタデータ](/help/authentication/user-metadata.md)

[トップに戻る…](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 呼び出し後に AccessEnabler によってトリガーされるコールバック[getAuthentication()](#getAuthN) 現在のリクエスト元が SSO をサポートする MVPD を少なくとも 1 つサポートしている場合。

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

**可用性：** v2.0 以降

**パラメーター**:

* viewController: Apple SSO ダイアログを表します。 この viewController は、画面に表示する必要があります。

**トリガー元：** [`getAuthentication`](#getAuthN)

**詳細情報：** [iOS/tvOS シングルサインオン](#presentTvDialog)

[トップに戻る…](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** ユーザーがApple SSO ダイアログを閉じた後に AccessEnabler によってトリガーされるコールバック。

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

**可用性：** v2.0 以降

**パラメーター**:

* viewController: Apple SSO ダイアログを表します。 この viewController は、画面から削除する必要があります。

**トリガー元：** ユーザーアクション

**詳細情報：** [iOS/tvOS シングルサインオン](#presentTvDialog)

[トップに戻る…](#apis)

</br>

### setMetadataStatus:encrypted:forKey:andArguments: {#setMetaStatus}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** AccessEnabler によってトリガーされるコールバック。 [`getMetadata:`](#getMeta) を呼び出します。

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

**可用性：** v1.0 以降

**パラメーター**:

* *メタデータ*：リクエストされたメタデータ。 この値は `NSString` 静的メタデータ（認証 TTL、認証 TTL、デバイス ID）の場合。  ユーザー固有のメタデータを要求する場合、これは複雑なオブジェクトです。 この複雑なオブジェクトは、通常、JSON ペイロードの Objective-C 表現です（例： 「{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;buildings&quot;）: [&quot;150&quot;, &quot;320&quot;]}&#39;は、Objective-C で NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;buildings&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;)) として翻訳されます。   サンプルのメタデータ JSON オブジェクト：

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

* *暗号化*：取得したメタデータを暗号化するかどうかを指定する Boolean 値です。 このパラメーターは、ユーザーメタデータ要求でのみ重要です。常に暗号化されていない静的メタデータ（認証 TTL など）を受け取る意味はありません。 このパラメータを true に設定した場合、プログラマは、ホワイトリストの秘密鍵を使用して RSA 復号を実行し、暗号化されていない User Metadata 値を取得します ( [`setRequestor:setSignedRequestorId:`](#setReq) および `setRequestor:setSignedRequestorId:serviceProviders: `呼び出し )。

* *key*：メタデータ取得リクエストの作成に使用するキー。

* *引数*：に渡されたのと同じ辞書 [`getMetadata:`](#getMeta) を呼び出します。 これは、アプリケーションが要求と応答を照合できるようにするために提供されます。

**トリガー元：** [`getMetadata:`](#getMeta)

**詳細情報：** [ユーザーメタデータ](/help/authentication/user-metadata.md)


[トップに戻る…](#apis)

</br>

### MVPD {#mvpd}

**ファイル：** AccessEnabler/headers/model/MVPD.h

**説明** MVPD オブジェクトを表します。 MVPD のプロパティに関する情報を取得するために使用できます。

**可用性：** v1.0 以降 [boardingStatus プロパティは v2.2 から使用可能です。]

**プロパティ**:

* (NSString) ID - MVPD Id。
* (NSString) displayName - MVPD 名。 [これは、ピッカーに表示するために使用する必要があります]
* (NSString) logoURL - MVPD のロゴアドレス。
* (BOOL) enablePlatformServices - true の場合、MVPD は次のような SSO サービスをサポートします。 [Apple SSO](#presentTvDialog).
* (NSString)boardingStatus — 次の 3 つの値を持つことができます。
   * nil - MVPD はApple SSO をサポートしていません。
   * PICKER - MVPD はAppleピッカーに表示される場合がありますが、認証フローはAdobeでおこなわれます。
   * サポート — MVPD はAppleで完全にサポートされ、Appleの SSO トークンを使用します。

[トップに戻る…](#apis)

</br>

## イベントの追跡 {#tracking}

AccessEnablerトリガーは、エンタイトルメントフローとは必ずしも関連しない追加のコールバックです。 の実装 [`sendTrackingData()`](#sendTracking) コールバック関数はオプションですが、アプリケーションは、特定のイベントを追跡し、認証/承認の試行の成功/失敗の回数などの統計をコンパイルできます。

### sendTrackingData:forEventType: {#sendTracking}

**ファイル：** AccessEnabler/headers/EntitlementDelegate.h

**説明** 認証/承認フローの完了/失敗など、様々なイベントの発生をアプリケーションに対する AccessEnabler シグナリングによってトリガーされるコールバック。 Adobe Pass Authentication 1.6 では、デバイスの種類、AccessEnabler のクライアントの種類、オペレーティング・システムは、次の方法で報告されます。 [`sendTrackingData()`](#sendTracking). The [`sendTrackingData()`](#sendTracking) コールバックは後方互換性を維持します。

**コールバック：イベントの追跡**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**可用性：** v1.0 以降

**注意：** デバイスタイプとオペレーティングシステムは、パブリック Java ライブラリ (<http://java.net/projects/user-agent-utils>) およびユーザーエージェント文字列。 この情報は、運用指標をAdobeのカテゴリに分類する簡単な方法としてのみ提供されますが、誤った結果に対する責任はに与えられません。 適宜、新しい機能を使用してください。

* デバイスタイプに指定できる値：
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* AccessEnabler クライアント・タイプに指定可能な値：
   * `flash`
   * `html5`
   * `ios`
   * `android`


**パラメーター**:

* *イベント*：追跡されるイベントのコード。 次の 3 つのタイプのトラッキングイベントが使用可能です。
   * **authorizationDetection:** 認証トークンリクエストが返されたとき ( イベントは `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** 認証チェックが発生した場合 ( イベントが `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** ユーザーが MVPD 選択フォームで MVPD を選択したとき ( イベントが `TRACKING_GET_SELECTED_PROVIDER`)
* *データ*：レポートされたイベントに関連付けられた追加データ。 このデータは、値のリストの形式で表示されます。

**トリガー元：** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN), `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ), `setSelectedProvider:`

の値を解釈するための手順 *データ* 配列：

* trackingEventType の場合 `TRACKING_AUTHENTICATION:`
   * **0**  — トークンリクエストが成功したか (true/false)、成功した場合は：
   * **1** - MVPD ID 文字列
   * **2** - GUID （md5 ハッシュ化）
   * **3**  — トークンは既にキャッシュに存在します (true/false)
   * **4**  — デバイスタイプ
   * **5** - AccessEnabler クライアント・タイプ
   * **6**  — オペレーティングシステムの種類

* trackingEventType の場合 `TRACKING_AUTHORIZATION:`
   * **0**  — トークンリクエストが成功したか (true/false)、成功した場合は：
   * **1** - MVPD ID
   * **2** - GUID （md5 ハッシュ化）
   * **3**  — トークンは既にキャッシュに存在します (true/false)
   * **4**  — エラー
   * **5**  — 詳細
   * **6**  — デバイスタイプ
   * **7** - AccessEnabler クライアント・タイプ
   * **8**  — オペレーティングシステムの種類
* trackingEventType の場合 `TRACKING_GET_SELECTED_PROVIDER:`
   * **0**  — 現在選択されている MVPD の ID
   * **1**  — デバイスタイプ
   * **2** - AccessEnabler クライアント・タイプ
   * **3**  — オペレーティングシステムの種類

</br>
