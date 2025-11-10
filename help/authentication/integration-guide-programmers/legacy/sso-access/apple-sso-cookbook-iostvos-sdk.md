---
title: Apple SSO クックブック（iOS/tvOS SDK）
description: Apple SSO クックブック（iOS/tvOS SDK）
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# （従来の）Apple SSO クックブック（iOS/tvOS SDK） {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

Adobe Pass Authentication AccessEnabler iOS/tvOS SDKは、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザー向けに、パートナーシングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の AccessEnabler iOS/tvOS SDK ドキュメントの拡張として機能します。このドキュメントは [ こちら ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) にあります。

## クックブック {#apple-sso-cookbook-iostvos-sdk-cookbook}

Apple SSO のユーザーエクスペリエンスを活用するには、AccessEnabler iOS/tvOS SDKを統合し、以下に示す一連の手順に従う必要があります。

### 前提条件 {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### 権限 {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>ヒント：</u>** ストリーミングアプリケーションは、デバイスレベルで保存されたユーザーの購読情報へのアクセスをリクエストする必要があります。これに対して、ユーザーは、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、続行する権限をアプリケーションに与える必要があります。 この権限は、Apple[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を使用しているアプリケーションごとにリクエストされる必要があります。

>[!TIP]
>
> **<u>ヒント：</u>** Appleのシングルサインオンユーザーエクスペリエンスの利点を説明することで、サブスクリプション情報へのアクセスを拒否するユーザーをインセンティブすることをお勧めしますが、アプリ設定（TV プロバイダーの権限アクセス）またはiOSと iPadOS または tvOS の *`Settings -> TV Provider`* に移動することで、ユーザーの判断を変 *`Settings -> Accounts -> TV Provider`* ることができます。

>[!TIP]
>
> **<u>ヒント：</u>** ストリーミングアプリケーションは、アプリケーションがフォアグラウンド状態に入ったときにユーザーの許可をリクエストできます。これは、ユーザー認証を必要とする前の任意の時点で、アプリケーションがユーザーの購読情報にアクセスするための [ 許可 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認できるためです。

>[!TIP]
>
> **<u>ヒント：</u>** サブスクリプション情報へのアクセス権がユーザーに付与されていない場合、またはビデオ購読者アカウントフレームワークとの通信に失敗した場合、AccessEnabler iOS/tvOS SDKは通常の認証フローにフォールバックします。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### コールバック {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>ヒント：</u>** Apple SSO ワークフローに固有の、次の [callbacks](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) リストを実装します。

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Apple MVPD ピッカーが開く際にトリガーされるコールバック。
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Apple MVPD ピッカーが閉じようとしたときにトリガーされるコールバック。

#### エラーレポート {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>ヒント：</u>** Apple SSO ワークフローに固有の [ 高度なエラーコード ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) のリストを実装します。

* ***N003*** - Apple MVPDピッカーから「その他のテレビプロバイダー」オプションを選択しました。
* ***N004*** – 現在のリクエスターでサポートされていない（統合またはシングルサインオンが無効になっている）Apple MVPD ピッカーからテレビプロバイダーを選択しました。
* ***N005*** – 通常のApple ピッカーまたはMVPD MVPD ピッカーをキャンセルすることにしました。
* ***VSA403*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が拒否されました。
* ***VSA404*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が不明です。
* ***VSA503*** - ビデオ サブスクライバ アカウントのメタデータ要求に失敗しました。詳細なコンテキストが *message* フィールドに提供されています。
* ***AAPL / APPL_ERROR*** - ビデオ購読者のアカウントメタデータのリクエストに失敗しました。詳細コンテキストが「*details*」フィールドに表示されます。

### 認証 {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>ヒント：</u>** iOS/iPadOS/tvOS の実装では、次の手順に従います。

1. AccessEnabler iOS/tvOS SDKを [ 初期化 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) する必要があります。


1. アプリケーションは、[ 現在の要求者識別子を設定 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3) する必要があります。

   **重要：** 次のいずれかが当てはまる場合、この 2 番目の手順では、Apple SSO ワークフローに固有の [](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) 高度なエラーコード **をトリガーする場合があります**。

   * ***VSA403*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が拒否されました。
   * ***VSA404*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が不明です。
   * ***APPL*** - AccessEnabler iOS/tvOS SDKとビデオ購読者アカウント フレームワーク間の通信でエラーが発生しました。

   この 2 番目の手順では、**上記のすべてが false** かつ **以下のすべてが true** の場合に、Apple SSO プロファイルをAdobe認証トークンとサイレントに交換しようとします。

   * ユーザーの TV プロバイダー権限がアプリケーションに付与されます。
   * ユーザーは、デバイス システム レベルで TV Provider アカウントにサインインしています。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウント フレームワークからユーザのテレビ プロバイダ識別子を受信しました。
   * アプリケーションと連携するユーザーのテレビプロバイダー統合は、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * アプリケーションを使用したユーザーの TV プロバイダーのシングルサインオンは、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * ユーザーの TV プロバイダーは、Adobe Primetime TVE ダッシュボードを使用して機能が低下することはありません。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウント フレームワークからユーザのテレビ プロバイダ SAML 応答を受信しました。

   **<u>ヒント：</u>** この 2 番目の手順では、[setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) コールバック以外のコールバックはトリガーされません。


1. アプリケーションは、[ 認証ステータスを確認 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn) する必要があります。

   **重要：** 次のいずれかが当てはまる場合、この 3 番目の手順では、Apple SSO ワークフローに固有の [](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) 高度なエラーコード **をトリガーする場合があります**。

   * ***VSA403** - ユーザーは、次の場所で TV Provider アカウントにサインインしています：
デバイスのシステム レベルですが、ユーザーのテレビ プロバイダのアクセス許可は
申請が拒否されました。
   * ***VSA404** - ユーザーは、次の場所で TV Provider アカウントにサインインしています：
デバイスのシステム レベル、ただしユーザーのテレビ プロバイダのアクセス許可
アプリケーションに対して未決定です。
   * ***APPL\_ERROR** - ユーザーが TV プロバイダーにサインインしている
デバイスのシステムレベルでの説明ですが、
AccessEnabler iOS/tvOS SDKおよびビデオ加入者アカウント
フレームワークでエラーが発生しました。

   **重要：** この 3 番目の手順では、[*status*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) が 0 に等しい *setAuthenticationStatus* コールバックを、**次のいずれかが true である** 場合にトリガーします。

   * ユーザーは、デバイス システム レベルまたは通常の認証フローで TV Provider アカウントにサインインしていません。
   * ユーザーは、デバイスシステムレベルまたは通常の認証フローを通じて TV プロバイダーアカウントにログインしますが、ユーザーの TV プロバイダー認証トークン TTL は合格しています。
   * ユーザーは、デバイスシステムレベルまたは通常の認証フローで TV プロバイダーアカウントにログインしますが、アプリケーションとのユーザーの TV プロバイダー統合は、Adobe Primetime TVE ダッシュボードで無効になっています。
   * ユーザーはデバイスシステムレベルで TV プロバイダーアカウントにサインインしますが、アプリケーションでのユーザーの TV プロバイダーのシングルサインオンは、Adobe Primetime TVE ダッシュボードで無効になります。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしていますが、アプリケーションに対するユーザーの TV Provider 権限が拒否されています。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしていますが、そのアプリケーションに対するユーザーの TV Provider 権限は未決定です。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしていますが、AccessEnabler iOS/tvOS SDKとビデオ加入者アカウント フレームワーク間の通信でエラーが発生しました。

   **重要：**[*この 3 番目の手順では、上記のすべてが false である場合に備えて、*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) status *が 1 に等しい* setAuthenticationStatus **コールバックをトリガーします。**


1. 以前の認証ステータスチェックで [status](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) が 0 に等しい [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) コールバックがトリガーされた場合、アプリケーションでは *認証の初期化* が必要になります。

   **<u>ヒント：</u>** 次の AccessEnabler iOS/tvOS SDK API[getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) または [getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter) のいずれかを実装します。

   **重要：** この 4 番目の手順では、[ 次のいずれかが当てはまる ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) 場合、Apple SSO ワークフローに固有の **高度なエラーコード** をトリガーにすることができます。

   * ***VSA403*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が拒否されました。
   * ***VSA404*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が不明です。
   * ***VSA503*** - AccessEnabler iOS/tvOS SDKとビデオ購読者アカウント フレームワーク間の通信でエラーが発生しました。
   * ***N003*** - Apple MVPDピッカーから「その他のテレビプロバイダー」オプションを選択しました。
   * ***N004*** – 現在のリクエスターでサポートされていない（統合またはシングルサインオンが無効になっている）Apple MVPD ピッカーからテレビプロバイダーを選択しました。
   * ***N005*** – 通常のApple ピッカーまたはMVPD MVPD ピッカーをキャンセルすることにしました。

   **重要：** この 4 番目の手順では、上記の [ 高度なエラーコード ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) いずれかが true **である場合に、** displayProviderDialog[ コールバックと ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)one **をトリガーして、通常の認証フローに戻ります**

   **重要：** この 4 番目の手順は、Apple SSO をサポートしていないがApple MVPD ピッカーに存在する TV プロバイダーを選択した場合、[navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url) または [navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) コールバックと、上記 **高度なエラーコード** の [none](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) をトリガーして、通常の認証フローに戻ります。

   **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDKは、Apple SSO をサポートしていないテレビプロバイダーを選択した場合に、Apple MVPD ピッカーに存在する [setSelectedProvder](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) API をサイレントに呼び出します。

   **重要：** この 4 番目の手順では、**上記のすべてが false** かつ **以下のすべてが true である** 場合に、Apple SSO プロファイルをAdobe認証トークンとサイレントに交換しようとします。

   * ユーザーの TV プロバイダー権限がアプリケーションに付与されます。
   * ユーザーはサインインしています/現在、デバイス システム レベルで TV Provider アカウントにサインインしています。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウント フレームワークからユーザのテレビ プロバイダ識別子を受信しました。
   * アプリケーションと連携するユーザーのテレビプロバイダー統合は、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * アプリケーションを使用したユーザーの TV プロバイダーのシングルサインオンは、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * ユーザーの TV プロバイダーは、Adobe Primetime TVE ダッシュボードを使用して機能が低下することはありません。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウント フレームワークからユーザのテレビ プロバイダ SAML 応答を受信しました。

   **<u>ヒント：</u>** この 4 番目の手順では、トリガーがアプリケーションによって明示的に開始されたので、[*status*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus) の結果に関係なく、*setAuthenticationStatus* コールバックを認証します。

### メタデータ {#apple-sso-cookbook-iostvos-sdk-metadata}

AccessEnabler iOS/tvOS SDKの「*tokenSource」*[user metadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) API を使用して、パートナー SSO にログインした結果として認証が発生したかどうかを判断するオプションがあります。

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### ログアウト {#apple-sso-cookbook-iostvos-sdk-logout}

[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) には、デバイスシステムレベルで TV プロバイダーアカウントにログインしたユーザーをプログラムでログアウトする API はありません。 そのため、ログアウトを完全に有効にするには、エンドユーザーはiOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* から明示的にログアウトする必要があります。 ユーザーが持つもう 1 つのオプションは、特定のアプリケーション設定セクション（TV プロバイダーの権限アクセス）からユーザーの購読情報にアクセスするための権限を取り消すことです。

>[!TIP]
>
> **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDK [ ログアウト ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) API を使用して実装します。

>[!TIP]
>
> **<u>プロのヒント：</u>** tvOS の実装については、次の手順に従ってください。

* アプリケーションは、AccessEnabler iOS/tvOS SDKから [ ログアウトを開始 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) する必要があります。 これは、MVPD側でのセッションクリーンアップを容易にするものではありません。
* アプリケーションは、*`Settings -> Accounts -> TV Provider`* VSA203 [*ステータスコードがトリガーされた場合にのみ、tvOS で* から明示的にログアウトするようにユーザーに指示またはプロンプトを表示する必要があり ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) す。

>[!TIP]
>
> **<u>プロのヒント：</u>** iOS/iPadOS を実装するには、次の手順に従います。

* アプリケーションは、AccessEnabler iOS/tvOS SDKから [ ログアウトを開始 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) する必要があります。 これにより、MVPD側のクリーンアップが容易になります。
* *`Settings -> TV Provider`* VSA203 [*ステータスコードがトリガーされた場合に限り、iOS/iPadOS 上で* から明示的にログアウトするようにユーザーに指示またはプロンプトを表示する必要があり ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) す。
