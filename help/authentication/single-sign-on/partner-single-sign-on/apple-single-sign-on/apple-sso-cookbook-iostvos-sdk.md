---
title: Apple SSO クックブック（iOS/tvOS SDK）
description: Apple SSO クックブック（iOS/tvOS SDK）
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1811'
ht-degree: 0%

---

# Apple SSO クックブック（iOS/tvOS SDK） {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

Adobe Pass Authentication AccessEnabler iOS/tvOS SDK は、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザー向けに、パートナーシングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の AccessEnabler iOS/tvOS SDK ドキュメントの拡張として機能します。このドキュメントについては [ こちら ](/help/authentication/iostvos-sdk-api-reference.md) を参照してください。

## クックブック {#apple-sso-cookbook-iostvos-sdk-cookbook}

Apple SSO のユーザーエクスペリエンスを活用するには、AccessEnabler iOS/tvOS SDK を組み込み、以下に示す一連の手順に従う必要があります。

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
> **<u>ヒント：</u>** サブスクリプション情報へのアクセス権をユーザーが付与していない場合、またはビデオ購読者アカウントフレームワークとの通信に失敗した場合、AccessEnabler iOS/tvOS SDK は通常の認証フローにフォールバックします。

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
> **<u>ヒント：</u>** Apple SSO ワークフローに固有の、次の [callbacks](/help/authentication/iostvos-sdk-api-reference.md) リストを実装します。

* [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Apple MVPD ピッカーが開こうとしたときにトリガーされるコールバック。
* [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Apple MVPD ピッカーが閉じようとしたときにトリガーされるコールバック。

#### エラーレポート {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>ヒント：</u>** Apple SSO ワークフローに固有の [ 高度なエラーコード ](/help/authentication/error-reporting.md) のリストを実装します。

* ***N003*** - Appleの MVPD ピッカーから「その他の TV プロバイダー」オプションを選択した。
* ***N004*** – 現在のリクエスターでサポートされていない（統合またはシングルサインオンが無効になっている）Apple MVPD ピッカーからテレビプロバイダーを選択しました。
* ***N005*** – 通常の MVPD ピッカーまたはApple MVPD ピッカーをキャンセルすることにしました。
* ***VSA403*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が拒否されました。
* ***VSA404*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が不明です。
* ***VSA503*** - ビデオ サブスクライバ アカウントのメタデータ要求に失敗しました。詳細なコンテキストが *message* フィールドに提供されています。
* ***AAPL / APPL_ERROR*** - ビデオ購読者のアカウントメタデータのリクエストに失敗しました。詳細コンテキストが「*details*」フィールドに表示されます。

### 認証 {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>ヒント：</u>** iOS/iPadOS/tvOS の実装では、次の手順に従います。

1. アプリケーションは、AccessEnabler iOS/tvOS SDK を [ 初期化 ](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) する必要があります。


1. アプリケーションは、[ 現在の要求者識別子を設定 ](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3) する必要があります。

   **重要：** 次のいずれかが当てはまる場合、この 2 番目の手順では、Apple SSO ワークフローに固有の [](/help/authentication/error-reporting.md) 高度なエラーコード **をトリガーする場合があります**。

   * ***VSA403*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が拒否されました。
   * ***VSA404*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が不明です。
   * ***APPL*** - AccessEnabler iOS/tvOS SDK とビデオ購読者アカウント フレームワーク間の通信でエラーが発生しました。

   この 2 番目の手順では、**上記のすべてが false** かつ **以下のすべてが true** の場合に、Apple SSO プロファイルをAdobe認証トークンとサイレントに交換しようとします。

   * ユーザーの TV プロバイダー権限がアプリケーションに付与されます。
   * ユーザーは、デバイス システム レベルで TV Provider アカウントにサインインしています。
   * AccessEnabler iOS/tvOS SDK は、ビデオ加入者アカウント フレームワークからユーザーのテレビ プロバイダー識別子を受信しました。
   * アプリケーションと連携するユーザーの TV プロバイダーの統合は、Adobe Primetime TVE Dashboard を介して有効になります。
   * このアプリでのユーザーの TV プロバイダーのシングルサインオンは、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * ユーザーの TV プロバイダは、Adobe Primetime TVE Dashboard を使用しても劣化しない。
   * AccessEnabler iOS/tvOS SDK は、ビデオ加入者アカウント フレームワークからユーザのテレビ プロバイダ SAML 応答を受信しました。

   **<u>ヒント：</u>** この 2 番目の手順では、[setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) コールバック以外のコールバックはトリガーされません。


1. アプリケーションは、[ 認証ステータスを確認 ](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn) する必要があります。

   **重要：** 次のいずれかが当てはまる場合、この 3 番目の手順では、Apple SSO ワークフローに固有の [](/help/authentication/error-reporting.md) 高度なエラーコード **をトリガーする場合があります**。

   * ***VSA403** - ユーザーは、次の場所で TV Provider アカウントにサインインしています：
デバイスのシステム レベルですが、ユーザーのテレビ プロバイダのアクセス許可は
申請が拒否されました。
   * ***VSA404** - ユーザーは、次の場所で TV Provider アカウントにサインインしています：
デバイスのシステム レベル、ただしユーザーのテレビ プロバイダのアクセス許可
アプリケーションに対して未決定です。
   * ***APPL\_ERROR** - ユーザーが TV プロバイダーにサインインしている
デバイスのシステムレベルでの説明ですが、
AccessEnabler iOS/tvOS SDK およびビデオ加入者アカウント
フレームワークでエラーが発生しました。

   **重要：** この 3 番目の手順では、*status*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) が 0 に等しい [*setAuthenticationStatus* コールバックを、**次のいずれかが true である** 場合にトリガーします。

   * ユーザーは、デバイス システム レベルまたは通常の認証フローで TV Provider アカウントにサインインしていません。
   * ユーザーは、デバイスシステムレベルまたは通常の認証フローを通じて TV プロバイダーアカウントにログインしますが、ユーザーの TV プロバイダー認証トークン TTL は合格しています。
   * ユーザーは、デバイス システム レベルまたは通常の認証フローを使用して TV Provider アカウントにサインインしますが、アプリケーションとのユーザーの TV Provider 統合は、Adobe Primetime TVE Dashboard を使用して無効になります。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしますが、Adobe Primetime TVE Dashboard を使用してユーザーの TV Provider Single Sign-On が無効になります。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしていますが、アプリケーションに対するユーザーの TV Provider 権限が拒否されています。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしていますが、そのアプリケーションに対するユーザーの TV Provider 権限は未決定です。
   * ユーザーはデバイス システム レベルで TV Provider アカウントにサインインしていますが、AccessEnabler iOS/tvOS SDK とビデオ加入者アカウント フレームワーク間の通信でエラーが発生しました。

   **重要：**[*この 3 番目の手順では、上記のすべてが false である場合に備えて、* status *](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)が 1 に等しい&#x200B;**setAuthenticationStatus* コールバックをトリガーします。**


1. 以前の認証ステータスチェックで [status](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) が 0 に等しい [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) コールバックがトリガーされた場合、アプリケーションでは *認証の初期化* が必要になります。

   **<u>ヒント：</u>** 次の AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) または [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter) のいずれかを実装します。

   **重要：** この 4 番目の手順では、[ 次のいずれかが当てはまる ](/help/authentication/error-reporting.md) 場合、Apple SSO ワークフローに固有の **高度なエラーコード** をトリガーにすることができます。

   * ***VSA403*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が拒否されました。
   * ***VSA404*** - アプリケーションに対するユーザーのテレビ プロバイダーのアクセス許可が不明です。
   * ***VSA503*** - AccessEnabler iOS/tvOS SDK とビデオ購読者アカウント フレームワーク間の通信でエラーが発生しました。
   * ***N003*** - Appleの MVPD ピッカーから「その他の TV プロバイダー」オプションを選択した。
   * ***N004*** – 現在のリクエスターでサポートされていない（統合またはシングルサインオンが無効になっている）Apple MVPD ピッカーからテレビプロバイダーを選択しました。
   * ***N005*** – 通常の MVPD ピッカーまたはApple MVPD ピッカーをキャンセルすることにしました。

   **重要：** この 4 番目の手順では、上記の [ 高度なエラーコード **いずれかが true** である場合に、**displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) コールバックと [one](/help/authentication/error-reporting.md) をトリガーして、通常の認証フローに戻ります**

   **重要：** この 4 番目の手順は、Apple SSO をサポートしていないがApple MVPD ピッカーに存在する TV プロバイダーを選択した場合、[navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) または [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) callback および **none** 上記の [ 高度なエラーコード ](/help/authentication/error-reporting.md) をトリガーして、通常の認証フローに戻ります。

   **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDK は、Apple MVPD ピッカーに存在するテレビ プロバイダーを選択した場合に、[setSelectedProvder](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API をサイレントに呼び出します。このプロバイダーはApple SSO をサポートしていませんが、

   **重要：** この 4 番目の手順では、**上記のすべてが false** かつ **以下のすべてが true** の場合に、Apple SSO プロファイルをAdobe認証トークンとサイレントに交換しようとします。

   * ユーザーの TV プロバイダー権限がアプリケーションに付与されます。
   * ユーザーはサインインしています/現在、デバイス システム レベルで TV Provider アカウントにサインインしています。
   * AccessEnabler iOS/tvOS SDK は、ビデオ加入者アカウント フレームワークからユーザーのテレビ プロバイダー識別子を受信しました。
   * アプリケーションと連携するユーザーの TV プロバイダーの統合は、Adobe Primetime TVE Dashboard を介して有効になります。
   * このアプリでのユーザーの TV プロバイダーのシングルサインオンは、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * ユーザーの TV プロバイダは、Adobe Primetime TVE Dashboard を使用しても劣化しない。
   * AccessEnabler iOS/tvOS SDK は、ビデオ加入者アカウント フレームワークからユーザのテレビ プロバイダ SAML 応答を受信しました。

   **<u>ヒント：</u>** この 4 番目の手順では、トリガーがアプリケーションによって明示的に開始されたので、*status*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) の結果に関係なく、[*setAuthenticationStatus* コールバックを認証します。

### メタデータ {#apple-sso-cookbook-iostvos-sdk-metadata}

AccessEnabler iOS/tvOS SDK の「*tokenSource」*[user metadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API を使用して、パートナー SSO にログインした結果として認証が発生したかどうかを判断するオプションがあります。

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### ログアウト {#apple-sso-cookbook-iostvos-sdk-logout}

[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) には、デバイスシステムレベルで TV プロバイダーアカウントにログインしたユーザーをプログラムでログアウトする API はありません。 そのため、ログアウトを完全に有効にするには、エンドユーザーはiOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* から明示的にログアウトする必要があります。 ユーザーが持つもう 1 つのオプションは、特定のアプリケーション設定セクション（TV プロバイダーの権限アクセス）からユーザーの購読情報にアクセスするための権限を取り消すことです。

>[!TIP]
>
> **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDK [ ログアウト ](/help/authentication/iostvos-sdk-api-reference.md#logout) API を使用して実装します。

>[!TIP]
>
> **<u>プロのヒント：</u>** tvOS の実装については、次の手順に従ってください。

* アプリケーションは、AccessEnabler iOS/tvOS SDK から [ ログアウトを開始 ](/help/authentication/iostvos-sdk-api-reference.md#logout) する必要があります。 これは、MVPD 側でのセッションのクリーンアップを容易にするものではありません。
* アプリケーションは、[*VSA203* ステータスコードがトリガーされた場合にのみ、tvOS で *`Settings -> Accounts -> TV Provider`* から明示的にログアウトするようにユーザーに指示またはプロンプトを表示する必要があり ](/help/authentication/error-reporting.md) す。

>[!TIP]
>
> **<u>プロのヒント：</u>** iOS/iPadOS を実装するには、次の手順に従います。

* アプリケーションは、AccessEnabler iOS/tvOS SDK から [ ログアウトを開始 ](/help/authentication/iostvos-sdk-api-reference.md#logout) する必要があります。 これにより、MVPD 側のセッションクリーンアップが容易になります。
* [*VSA203* ステータスコードがトリガーされた場合に限り、iOS/iPadOS 上で *`Settings -> TV Provider`* から明示的にログアウトするようにユーザーに指示またはプロンプトを表示する必要があり ](/help/authentication/error-reporting.md) す。
