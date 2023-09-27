---
title: Apple SSO クックブック (iOS/tvOS SDK)
description: Apple SSO クックブック (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Apple SSO クックブック (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#Introduction}

Adobe Primetime Authentication AccessEnabler iOS/tvOS SDK は、Apple SSO ワークフローと呼ばれる機能を通じて、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザーに対する、プラットフォームのシングルサインオン (SSO) 認証をサポートします。

このドキュメントは、既存の AccessEnabler iOS/tvOS SDK ドキュメントの拡張機能として機能します。 [ここ](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## クックブック {#Cookbook}

Apple SSO のユーザーエクスペリエンスを最大限に活用するには、1 つのアプリケーションで AccessEnabler iOS/tvOS SDK を統合し、以下に示すヒントの順序に従う必要があります。

</br>

### 前提条件 {#Prerequisites}

</br>

#### 権限

>[!TIP]
>
> **<u>ヒント：</u>** ユーザーのサブスクリプション情報にアクセスするには、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、ユーザーはアプリケーションに続行の権限を付与する必要があります。 この権限は、アプリケーションごとにリクエストする必要があります。デバイスは、ユーザーの選択内容を保存します。 ユーザーは、アプリケーション設定（TV Provider 権限のアクセス）または次のセクションに移動して、決定を変更できます。 *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* tvOS の場合。

>[!TIP]
>
> **<u>ヒント：</u>** アプリケーションがフォアグラウンド状態になった場合は、ユーザーの権限をリクエストすることをお勧めしますが、アプリケーションはをチェックできるので、これは単なる提案です。 [アクセス権](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) ユーザー認証を必要とする前の任意の時点でのユーザーの購読情報。 また、AccessEnabler iOS/tvOS SDK API は、必要に応じて、ユーザーの権限を自動的に要求します。

>[!TIP]
>
> **<u>ヒント：</u>** ユーザーがサブスクリプション情報へのアクセスを許可しない場合、またはビデオ加入者アカウントフレームワークとの通信に失敗した場合、AccessEnabler iOS/tvOS SDK は通常の認証フローにフォールバックします。

>[!TIP]
>
> **<u>ヒント：</u>** シングルサインオン (SSO) ユーザーエクスペリエンスの利点を説明し、サブスクリプション情報へのアクセス権の付与を拒否するユーザーに対しては、その利点を説明することをお勧めします。 ユーザーは、アプリケーション設定（TV Provider 権限のアクセス）または次のセクションに移動して、決定を変更できます。 *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* tvOS の場合。


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

</br>

#### コールバック

>[!TIP]
>
> **<u>ヒント：</u>** 次の [コールバック](/help/authentication/iostvos-sdk-api-reference.md) これはApple SSO ワークフローに固有のものです。

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Apple MVPD ピッカーが開くときにコールバックがトリガーされます。
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Apple MVPD ピッカーが閉じられるときにコールバックがトリガーされます。

</br>

#### エラーレポート

>[!TIP]
>
> **<u>ヒント：</u>** 次の [詳細なエラーコード](/help/authentication/error-reporting.md) これはApple SSO ワークフローに固有のものです。

- ***N003***  — ユーザーがApple MVPD ピッカーから「その他の TV プロバイダー」オプションを選択しました。
- ***N004***  — ユーザーがApple MVPD ピッカーから TV プロバイダーを選択しました。現在の要求者は、この TV プロバイダーをサポートしていません（統合またはシングルサインオンが無効になっています）。
- ***N005***  — ユーザーは、通常の MVPD ピッカーまたはApple MVPD ピッカーをキャンセルすることにしました。
- ***VSA403***  — ユーザーの TV Provider 権限がアプリケーションで拒否されました。
- ***VSA404***  — ユーザーの TV Provider 権限が、アプリケーションに対して未確定です。
- ***VSA503***  — ビデオ購読者のアカウントメタデータリクエストが失敗しました。 *メッセージ* フィールドに入力します。
- ***AAPL / APPL_ERROR***  — ビデオ購読者のアカウントメタデータリクエストが失敗しました。 *詳細* フィールドに入力します。

</br>

### 認証 {#Authentication}

>[!TIP]
>
> **<u>ヒント：</u>** iOS/iPadOS/tvOS の実装については、以下の手順に従います。

1. この申し込みは、 [initialize](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK
1. この申し込みは、 [現在の要求元識別子を設定](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **重要：** この 2 番目の手順では、トリガーが [詳細なエラーコード](/help/authentication/error-reporting.md) ( 場合によってはApple SSO ワークフローに固有 ) **次のうちの 1 つが真である**:

   - ***VSA403***  — ユーザーの TV Provider 権限がアプリケーションで拒否されました。
   - ***VSA404***  — ユーザーの TV Provider 権限が、アプリケーションに対して未確定です。
   - ***APPL*** - AccessEnabler iOS/tvOS SDK とビデオ加入者アカウントフレームワーク間の通信でエラーが発生しました。

   2 番目の手順では、場合によっては、Apple SSO プロファイルをAdobe認証トークンに対してサイレントに交換しようとします。 **上記のすべてが偽である** および **次のすべてが真**:

   - ユーザーの TV Provider 権限がアプリケーションに対して付与されます。
   - ユーザーは、デバイスのシステムレベルで TV Provider アカウントにサインインしています。
   - AccessEnabler iOS/tvOS SDK は、ビデオ加入者アカウントフレームワークからユーザーの TV プロバイダー ID を受け取りました。
   - ユーザーの TV Provider とアプリケーションの統合は、 Adobe Primetime TVE Dashboard を通じて有効になります。
   - アプリケーションを使用したユーザーの TV Provider のシングルサインオンは、Adobe Primetime TVE ダッシュボードで有効になります。
   - ユーザーの TV Provider は、Adobe Primetime TVE Dashboard を使用して低下しません。
   - AccessEnabler iOS/tvOS SDK は、Video Subscriber Account Framework からユーザーの TV Provider SAML 応答を受け取りました。

   **<u>ヒント：</u>** この 2 番目の手順では、 [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) callback。アプリケーションによって認証が明示的に開始されなかったためです。

1. この申し込みは、 [認証状態の確認](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **重要：** この 3 番目の手順では、 [詳細なエラーコード](/help/authentication/error-reporting.md) ( 場合によってはApple SSO ワークフローに固有 ) **次のうちの 1 つが真である**:

   - ***VSA403**  — ユーザーはデバイスのシステムレベルで TV Provider アカウントにサインインしていますが、ユーザーの TV Provider 権限がアプリケーションで拒否されています。
   - ***VSA404**  — ユーザーがデバイスのシステムレベルで TV Provider アカウントにサインインしていますが、そのアプリケーションに対するユーザーの TV Provider 権限が未確定です。
   - ***APPL\_ERROR**  — ユーザーがデバイスのシステムレベルで TV Provider アカウントにサインインしていますが、AccessEnabler iOS/tvOS SDK とビデオ加入者アカウントフレームワーク間の通信でエラーが発生しました。

   **重要：** この 3 番目の手順では、 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) を使用したコールバック *ステータス* 0 と等しい（場合に応じて） **次のうちの 1 つが真である**:

   - ユーザーは、デバイスのシステムレベルで、または通常の認証フローで、TV Provider アカウントにサインインしていません。
   - ユーザーは、デバイスシステムレベルまたは通常の認証フローで TV Provider アカウントにサインインしていますが、ユーザーの TV Provider 認証トークン TTL が過ぎています。
   - ユーザーは、デバイスシステムレベルまたは通常の認証フローで TV Provider アカウントにサインインしていますが、Adobe Primetime TVE Dashboard を使用すると、アプリケーションとの TV Provider 統合が無効になります。
   - ユーザーは、デバイスのシステムレベルで TV Provider アカウントにサインインしていますが、Adobe Primetime TVE Dashboard を使用すると、アプリケーションでのユーザーの TV Provider のシングルサインオンが無効になります。
   - ユーザーは、デバイスのシステムレベルで TV Provider アカウントにサインインしていますが、そのアプリケーションに対するユーザーの TV Provider 権限が拒否されています。
   - ユーザーがデバイスシステムレベルで TV Provider アカウントにサインインしていますが、アプリケーションに対するユーザーの TV Provider 権限が未確定です。
   - ユーザーがデバイスシステムレベルで TV Provider アカウントにサインインしていますが、AccessEnabler iOS/tvOS SDK と Video Subscriber Account Framework 間の通信でエラーが発生しました。

   **重要：** この 3 番目の手順では、 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) を使用したコールバック *ステータス* 1 と等しい（場合に応じて） **上記のすべては偽です。**


1. この申し込みは、 [認証を初期化する](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) 以前の認証ステータスチェックで [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) を使用したコールバック *ステータス* 0 と等しい。

   **<u>ヒント：</u>** 次の AccessEnabler iOS/tvOS SDK API のいずれかを実装します。 [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) または [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **重要：** この 4 つ目の手順では、次のトリガーを実行できます。 [詳細なエラーコード](/help/authentication/error-reporting.md) ( 場合によってはApple SSO ワークフローに固有 ) **次のうちの 1 つが真である**:

   - ***VSA403***  — ユーザーの TV Provider 権限がアプリケーションで拒否されました。
   - ***VSA404***  — ユーザーの TV Provider 権限が、アプリケーションに対して未確定です。
   - ***VSA503*** - AccessEnabler iOS/tvOS SDK とビデオ加入者アカウントフレームワーク間の通信でエラーが発生しました。
   - ***N003***  — ユーザーがApple MVPD ピッカーから「その他の TV プロバイダー」オプションを選択しました。
   - ***N004***  — ユーザーがApple MVPD ピッカーから TV プロバイダーを選択しました。現在の要求者は、この TV プロバイダーをサポートしていません（統合またはシングルサインオンが無効になっています）。
   - ***N005***  — ユーザーは、通常の MVPD ピッカーまたはApple MVPD ピッカーをキャンセルすることにしました。

   **重要：** この 4 つ目の手順は、 [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) コールバックと **1 つ** 上記の [詳細なエラーコード](/help/authentication/error-reporting.md)（場合に応じて） **上記の一つが真である**.

   **重要：** この 4 つ目の手順は、 [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) または [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) コールバックと **なし** 上記の [詳細なエラーコード](/help/authentication/error-reporting.md)( ユーザーがApple SSO をサポートしないが、Apple MVPD ピッカーに存在する TV プロバイダーを選択した場合 )。

   **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDK は、 [setSelectedProvder](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) ユーザーがApple SSO をサポートしないが、Apple MVPD ピッカーに存在する TV プロバイダーを選択した場合の API。

   **重要：** 4 つ目の手順では、場合に応じて、Apple SSO プロファイルをAdobe認証トークンと無音で交換しようとします。 **上記のすべてが偽である** および **次のすべてが真**:

   - ユーザーの TV Provider 権限がアプリケーションに対して付与されます。
   - ユーザーは、デバイスのシステムレベルで TV Provider アカウントにサインインしているか、現在サインインしています。
   - AccessEnabler iOS/tvOS SDK は、ビデオ加入者アカウントフレームワークからユーザーの TV プロバイダー ID を受け取りました。
   - ユーザーの TV Provider とアプリケーションの統合は、 Adobe Primetime TVE Dashboard を通じて有効になります。
   - アプリケーションを使用したユーザーの TV Provider のシングルサインオンは、Adobe Primetime TVE ダッシュボードで有効になります。
   - ユーザーの TV Provider は、Adobe Primetime TVE Dashboard を使用して低下しません。
   - AccessEnabler iOS/tvOS SDK は、Video Subscriber Account Framework からユーザーの TV Provider SAML 応答を受け取りました。



>**<u>ヒント：</u>** この 4 つ目の手順では、 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) コールバック、 *ステータス* 結果、アプリケーションによって認証が明示的に開始されたため。


</br>

### メタデータ {#Metadata}

アプリケーションには、「*tokenSource&quot;* [ユーザーメタデータ](/help/authentication/iostvos-sdk-api-reference.md#getMeta) AccessEnabler iOS/tvOS SDK からの API

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### ログアウト {#Logout}

The [ビデオ購読者のアカウント](https://developer.apple.com/documentation/videosubscriberaccount) framework は、デバイスのシステムレベルで TV プロバイダーアカウントにサインインしたユーザーをプログラムでログアウトする API を提供しません。 したがって、ログアウトが完全に有効になるには、エンドユーザーは次の場所から明示的にログアウトする必要があります。 *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* tvOS の場合。 ユーザーが持つもう 1 つのオプションは、特定のアプリケーション設定セクション（TV プロバイダーの権限アクセス）からユーザーの購読情報にアクセスする権限を撤回することです。

>[!TIP]
>
> **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDK を使用して実装します。 [ログアウト](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>ヒント：</u>** tvOS の実装については、以下の手順に従います。

- この申し込みは、 [ログアウトの開始](/help/authentication/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK から。 これは、MVPD 側でのセッションのクリーンアップを容易にしません。
- アプリケーションは、ユーザーに対し、明示的にログアウトするように指示/プロンプトを表示する必要があります。 *`Settings -> Accounts -> TV Provider`* tvOS の場合のみ [*VSA203* ステータスコードがトリガーされました](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>ヒント：</u>** iOS/iPadOS の実装については、以下の手順に従います。

- この申し込みは、 [ログアウトの開始](/help/authentication/iostvos-sdk-api-reference.md#logout) AccessEnabler iOS/tvOS SDK から。 これにより、MVPD 側でのセッションのクリーンアップが容易になります。
- アプリケーションは、ユーザーに対し、明示的にログアウトするように指示/プロンプトを表示する必要があります。 *`Settings -> TV Provider`* (iOS/iPadOS の場合のみ ) [*VSA203* ステータスコードがトリガーされました](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
