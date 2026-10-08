---
title: Apple SSO Cookbook （iOS/tvOS SDK）
description: Apple SSO Cookbook （iOS/tvOS SDK）
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1854'
ht-degree: 0%
---
# （レガシー） Apple SSO クックブック （iOS/tvOS SDK） {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

Adobe Pass Authentication AccessEnabler iOS/tvOS SDKは、iOS、iPadOS、またはtvOSで動作するクライアントアプリケーションのエンドユーザー向けのパートナーシングルサインオン（SSO）をサポートしています。

このドキュメントは、既存のAccessEnabler iOS/tvOS SDK ドキュメントの拡張機能として機能します。このドキュメントは、[こちら](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)にあります。

## Cookbook {#apple-sso-cookbook-iostvos-sdk-cookbook}

Apple SSO ユーザーエクスペリエンスを活用するには、AccessEnabler iOS/tvOS SDKを統合し、以下に示す一連の手順に従う必要があります。

### 前提条件 {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### 権限 {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>プロのヒント：</u>** ストリーミングアプリケーションは、デバイスのカメラやマイクへのアクセスを提供するのと同様に、デバイスレベルで保存されたユーザーのサブスクリプション情報へのアクセスをリクエストする必要があります。この場合、ユーザーはアプリケーションに続行する権限を付与する必要があります。 この権限は、Appleの[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)を使用してアプリケーションごとに要求する必要があり、デバイスはユーザーの選択内容を保存します。

>[!TIP]
>
> **<u>Pro ヒント：</u>** Apple シングルサインオンのユーザーエクスペリエンスのメリットを説明することで、サブスクリプション情報へのアクセスを拒否するユーザーにインセンティブを提供することをお勧めしますが、アプリケーションの設定（TV プロバイダーのアクセス権）に移動するか、iOSおよびiPadOSの&#x200B;*`Settings -> TV Provider`*&#x200B;またはtvOSの&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;に移動することで、ユーザーの判断を変えることができます。

>[!TIP]
>
> **<u>プロのヒント：</u>** ストリーミングアプリケーションは、アプリケーションがフォアグラウンド状態に入ったときに、ユーザーの権限を要求できます。ユーザー認証を必要とする前に、任意の時点で[&#x200B; ユーザーの購読情報にアクセスする権限](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)をアプリケーションが確認できるからです。

>[!TIP]
>
> **<u>Pro ヒント：</u>**&#x200B;お客様がサブスクリプション情報へのアクセスを許可しない場合、またはビデオ購読者アカウントフレームワークとの通信が失敗した場合、AccessEnabler iOS/tvOS SDKは通常の認証フローにフォールバックします。

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
> **<u>プロ向けのヒント：</u>** Apple SSO ワークフローに固有の[&#x200B; コールバック &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)の次のリストを実装します。

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Apple MVPD ピッカーが開くときにコールバックがトリガーされます。
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Apple MVPD ピッカーが閉じるときにコールバックがトリガーされます。

#### エラー報告 {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>プロ向けのヒント：</u>** Apple SSO ワークフローに固有の[詳細エラーコード &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)の次のリストを実装します。

* ***N003*** - Apple MVPD ピッカーから「その他のTV プロバイダー」オプションを選択しました。
* ***N004*** - Apple MVPD ピッカーからTV プロバイダーを選択しましたが、現在の依頼者はサポートしていません（統合またはシングルサインオンが無効）。
* ***N005*** – 通常のMVPD ピッカーまたはApple MVPD ピッカーの解約を決定しました。
* ***VSA403*** - ユーザーのTV プロバイダー権限がアプリケーションに対して拒否されました。
* ***VSA404*** - ユーザーのTV プロバイダー権限がアプリケーションに対して決定されていません。
* ***VSA503*** - ビデオ購読者アカウントのメタデータ要求が失敗しました。詳細については、*メッセージ* フィールドを参照してください。
* ***AAPL / APPL_ERROR*** - ビデオ購読者アカウントのメタデータ要求が失敗しました。詳細については、*詳細* フィールドを参照してください。

### 認証 {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>ヒント：</u>** iOS/iPadOS/tvOSの実装については、次の手順に従ってください。

1. AccessEnabler iOS/tvOS SDKを[初期化](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement)する必要があります。


1. アプリケーションは、現在の依頼者識別子[&#128279;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3)を設定する必要があります。

   **重要：**&#x200B;この2番目の手順では、Apple SSO ワークフローに固有の[高度なエラーコード &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)をトリガーする可能性があります。次のいずれか&#x200B;**がtrue**&#x200B;の場合です。

   * ***VSA403*** - ユーザーのTV プロバイダー権限がアプリケーションに対して拒否されました。
   * ***VSA404*** - ユーザーのTV プロバイダー権限がアプリケーションに対して決定されていません。
   * ***APPL*** - AccessEnabler iOS/tvOS SDKとビデオ サブスクライバーのアカウント フレームワーク間の通信でエラーが発生しました。

   この2番目の手順では、上記の&#x200B;**すべてがfalse**&#x200B;で、**すべてがtrue**&#x200B;の場合、Apple SSO プロファイルをAdobe認証トークンとサイレントに交換しようとします。

   * ユーザーのTV プロバイダー権限がアプリケーションに付与されます。
   * ユーザーは、デバイスシステムレベルでTV プロバイダーアカウントにログインしています。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウントフレームワークからユーザーのTV プロバイダーIDを受け取りました。
   * ユーザーのTV プロバイダーとアプリケーションの統合は、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * アプリケーションを使用したユーザーのTV プロバイダーシングルサインオンは、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * ユーザーのTV プロバイダーは、Adobe Primetime TVE ダッシュボードを通じて劣化しません。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウントフレームワークからユーザーのTV プロバイダーのSAML応答を受け取りました。

   **<u>Pro ヒント：</u>**&#x200B;この2番目の手順では、アプリケーションによってトリガーが明示的に開始されていないため、[setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) コールバック以外のコールバックは認証されません。


1. アプリケーションでは、認証ステータスを[確認](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn)する必要があります。

   **重要：**&#x200B;この3番目の手順では、Apple SSO ワークフローに固有の[詳細エラーコード &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)がトリガーされる可能性があります。次のいずれか&#x200B;**がtrue**&#x200B;の場合です。

   * ***VSA403** - ユーザーは次の場所でTV プロバイダーアカウントにサインインしています
     デバイスシステムレベルですが、ユーザーのTV プロバイダー権限は
     アプリケーションに対して拒否されました。
   * ***VSA404** - ユーザーは次の場所でTV プロバイダーアカウントにサインインしています
     デバイスのシステムレベルではなく、ユーザーのTV プロバイダー権限
     アプリケーションに対して未決定です。
   * ***APPL\_ERROR** - ユーザーはTV プロバイダーにサインインしています
     アカウントはデバイスシステムレベルで使用されますが、アプリ内の
     accessEnabler iOS/tvOS SDKおよびビデオ購読者アカウント
     フレームワークでエラーが発生しました。

   **重要：**&#x200B;この3番目の手順では、[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) コールバックを&#x200B;*status*&#x200B;が0に等しい状態でトリガーします。次の&#x200B;**のいずれかがtrue**&#x200B;の場合です。

   * ユーザーは、デバイスシステムレベルまたは通常の認証フローを通じて、TV プロバイダーアカウントにログインしていません。
   * ユーザーは、デバイスシステムレベルまたは通常の認証フローを通じてTV プロバイダーのアカウントにログインしていますが、ユーザーのTV プロバイダー認証トークン TTLは合格しています。
   * ユーザーは、デバイスシステムレベルまたは通常の認証フローを通じてTV プロバイダーアカウントにログインしていますが、Adobe Primetime TVE ダッシュボードを使用して、ユーザーのTV プロバイダーとアプリケーションとの統合は無効になっています。
   * ユーザーはデバイスシステムレベルでTV プロバイダーアカウントにログインしていますが、アプリケーションを使用したユーザーのTV プロバイダーシングルサインオンは、Adobe Primetime TVE ダッシュボードを使用して無効になっています。
   * ユーザーはデバイスシステムレベルでTV プロバイダーアカウントにログインしていますが、ユーザーのTV プロバイダー権限はアプリケーションに対して拒否されます。
   * ユーザーはデバイスシステムレベルでTV プロバイダーアカウントにログインしていますが、ユーザーのTV プロバイダー権限はアプリケーションに対して決定されていません。
   * ユーザーはデバイスシステムレベルでTV プロバイダーアカウントにログインしていますが、AccessEnabler iOS/tvOS SDKとビデオ加入者アカウントフレームワークの間でエラーが発生しました。

   **重要：**&#x200B;この3番目の手順では、[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) コールバックを&#x200B;*status*&#x200B;が1に等しい状態でトリガーします。上記の&#x200B;**すべてがfalseの場合です。**


1. 以前の認証ステータス確認で&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) コールバックがトリガーされ、*status*&#x200B;が0に等しい場合、アプリケーションは[認証を初期化する必要があります](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn)。

   **<u>Pro ヒント：</u>**&#x200B;次のいずれかのAccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN)または[getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter)を実装します。

   **重要：**&#x200B;この4つ目の手順では、Apple SSO ワークフローに固有の[高度なエラーコード &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)をトリガーする可能性があります。次の&#x200B;**のいずれかがtrue**&#x200B;の場合です。

   * ***VSA403*** - ユーザーのTV プロバイダー権限がアプリケーションに対して拒否されました。
   * ***VSA404*** - ユーザーのTV プロバイダー権限がアプリケーションに対して決定されていません。
   * ***VSA503*** - AccessEnabler iOS/tvOS SDKとビデオ サブスクライバーのアカウント フレームワーク間の通信でエラーが発生しました。
   * ***N003*** - Apple MVPD ピッカーから「その他のTV プロバイダー」オプションを選択しました。
   * ***N004*** - Apple MVPD ピッカーからTV プロバイダーを選択しましたが、現在の依頼者はサポートしていません（統合またはシングルサインオンが無効）。
   * ***N005*** – 通常のMVPD ピッカーまたはApple MVPD ピッカーの解約を決定しました。

   **重要：**&#x200B;この4番目のステップは、上記の[高度なエラーコード &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)のうち[displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) コールバックと&#x200B;**one**&#x200B;をトリガーすることで、通常の認証フローにフォールバックします。上記のいずれかが&#x200B;**true**&#x200B;の場合です。

   **重要：**&#x200B;この4番目のステップは、上記の[高度なエラーコード &#x200B;](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)の[navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url)または[navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) コールバックと&#x200B;**none**&#x200B;をトリガーすることで、通常の認証フローにフォールバックします。これは、ユーザーがApple SSOをサポートしていないが、Apple MVPD ピッカーに存在するTV プロバイダーを選択した場合です。

   **<u>Pro ヒント：</u>** AccessEnabler iOS/tvOS SDKは、ユーザーがApple SSOをサポートしていないがApple MVPD ピッカーに存在するTV プロバイダーを選択した場合、[setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) APIをサイレントに呼び出します。

   **重要：**&#x200B;この4番目の手順では、上記の&#x200B;**すべてがfalse**&#x200B;で、次のすべてがtrue **である場合に、Apple SSO プロファイルをAdobe認証トークンとサイレントに交換しようとします。**

   * ユーザーのTV プロバイダー権限がアプリケーションに付与されます。
   * ユーザーは、デバイスシステムレベルでTV プロバイダーアカウントにサインインしています/現在ログインしています。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウントフレームワークからユーザーのTV プロバイダーIDを受け取りました。
   * ユーザーのTV プロバイダーとアプリケーションの統合は、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * アプリケーションを使用したユーザーのTV プロバイダーシングルサインオンは、Adobe Primetime TVE ダッシュボードを通じて有効になります。
   * ユーザーのTV プロバイダーは、Adobe Primetime TVE ダッシュボードを通じて劣化しません。
   * AccessEnabler iOS/tvOS SDKは、ビデオ加入者アカウントフレームワークからユーザーのTV プロバイダーのSAML応答を受け取りました。

   **<u>Pro ヒント：</u>**&#x200B;この4番目のステップでは、アプリケーションによって明示的にトリガーが開始されたため、*status*&#x200B;の結果に関係なく、[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus)&#x200B;のコールバックが認証されます。

### メタデータ {#apple-sso-cookbook-iostvos-sdk-metadata}

AccessEnabler iOS/tvOS SDKの「*tokenSource」* [&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) APIを使用して、パートナーSSOによるログインの結果として認証が行われたかどうかを判断するオプションがあります。

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### ログアウト {#apple-sso-cookbook-iostvos-sdk-logout}

[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)では、デバイス システム レベルでテレビ プロバイダーのアカウントにサインインしたユーザーをプログラムでログアウトするためのAPIが提供されていません。 したがって、ログアウトを完全に有効にするには、iOS/iPadOSの&#x200B;*`Settings -> TV Provider`*&#x200B;から明示的にログアウトするか、tvOSの&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;から明示的にログアウトする必要があります。 ユーザーが持つもう1つのオプションは、特定のアプリケーション設定セクションからユーザーの購読情報にアクセスする権限を取り消すことです（テレビプロバイダーの権限アクセス）。

>[!TIP]
>
> **<u>ヒント：</u>** AccessEnabler iOS/tvOS SDK [&#x200B; ログアウト &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) APIを使用して、これを実装します。

>[!TIP]
>
> **<u>Pro ヒント：</u>** tvOSの実装については、次の手順に従ってください。

* AccessEnabler iOS/tvOS SDKから[&#x200B; ログアウトを開始する必要があります](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout)。 これは、MVPD側のセッションのクリーンアップを容易にするものではありません。
* [*VSA203*&#x200B;のステータスコードがトリガー](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)された場合にのみ、アプリケーションは、tvOSで&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;から明示的にログアウトするようにユーザーに指示または指示する必要があります。

>[!TIP]
>
> **<u>Pro ヒント：</u>** iOS/iPadOSの実装については、次の手順に従ってください。

* AccessEnabler iOS/tvOS SDKから[&#x200B; ログアウトを開始する必要があります](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout)。 これにより、MVPD側のセッションのクリーンアップが容易になります。
* [*VSA203*&#x200B;のステータスコードがトリガー](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)された場合にのみ、iOS/iPadOSの&#x200B;*`Settings -> TV Provider`*&#x200B;から明示的にログアウトするようにユーザーに指示または指示する必要があります。
