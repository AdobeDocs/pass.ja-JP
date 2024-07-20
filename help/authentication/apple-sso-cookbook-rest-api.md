---
title: Apple SSO クックブック（REST API）
description: Apple SSO クックブック（REST API）
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Apple SSO クックブック（REST API） {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#Introduction}

Adobe Pass認証 REST API は、Apple SSO ワークフローと呼ばれるワークフローを通じて、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザーに対して、Platform シングルサインオン（SSO）認証をサポートできます。

このドキュメントは、既存の REST API ドキュメント（[ こちら ](/help/authentication/rest-api-reference.md) の拡張機能として機能することに注意してください。

## クックブック {#Cookbooks}

Apple SSO のユーザーエクスペリエンスを活用するには、Appleが開発した [ ビデオ購読者アカウント ](https://developer.apple.com/documentation/videosubscriberaccount) フレームワークを 1 つのアプリケーションで統合する必要があります。一方、Adobe Pass認証 REST API 通信については、以下に示す一連のヒントに従う必要があります。

### 認証 {#Authentication}

- [有効なAdobe認証トークンはありますか？](#Is_there_a_valid_Adobe_authentication_token)
- [ユーザーは Platform SSO を使用してログインしていますか？](#Is_the_user_logged_in_via_Platform_SSO)
- [Adobe設定を取得](#Fetch_Adobe_configuration)
- [Adobe設定を使用した Platform SSO ワークフローの開始](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [ユーザーログインに成功しましたか？](#Is_user_login_successful)
- [選択した MVPD のAdobeからのプロファイルリクエストの取得](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Adobeリクエストを Platform SSO に転送してプロファイルを取得します](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Platform SSO プロファイルをAdobe認証トークンと交換します](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [Adobeトークンは正常に生成されましたか？](#Is_Adobe_token_generated_successfully)
- [2 番目の画面認証ワークフローの開始](#Initiate_second_screen_authentication_workflow)
- [承認フローの続行](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### 手順：「有効なAdobe認証トークンがあるか」 {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>ヒント：</u>** [Adobe Pass Authentication](/help/authentication/check-authentication-token.md) サービスを使用して、これを実装します。


#### 手順：「ユーザーは Platform SSO を使用してログインしていますか？」 {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>ヒント：</u>** ビデオ購読者アカウント [ フレームワークを通じて実装 ](https://developer.apple.com/documentation/videosubscriberaccount) ます。

- アプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
- アプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
- アプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。



>[!TIP]
>
> **<u>プロのヒント：</u>** コードスニペットに従い、コメントに特に注意を払ってください。

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### 手順：「Adobe設定を取得」 {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>ヒント：</u>** [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) サービスを使用して、これを実装します。


>[!TIP]
>
> **<u>Pro ヒント：</u>** MVPD プロパティ（*`enablePlatformServices`*、*`boardingStatus`*、*`displayInPlatformPicker`*、*`platformMappingId`*、*`requiredMetadataFields`*）に注意し、他の手順のコードスニペットに表示されるコメントに特に注意してください。

#### 手順「Adobe設定を使用して Platform SSO ワークフローを開始する」 {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>ヒント：</u>** ビデオ購読者アカウント [ フレームワークを通じて実装 ](https://developer.apple.com/documentation/videosubscriberaccount) ます。

- アプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
- アプリケーションは、VSAccountManager に [delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
- アプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
- アプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。



>[!TIP]
>
> **<u>プロのヒント：</u>** コードスニペットに従い、コメントに特に注意を払ってください。


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 手順：「ユーザーログインに成功しましたか？」 {#Is_user_login_successful}

>[!TIP]
>
> **<u>ヒント：</u>**[ 「Adobe設定を使用して Platform SSO ワークフローを開始する」 ](#Initiate_Platform_SSO_workflow_with_Adobe_config) ステップのコードスニペットに注意してください。 *`vsaMetadata!.accountProviderIdentifier`* に有効な値が含まれ、現在の日付が *`vsaMetadata!.authenticationExpirationDate`* 値を渡していない場合、ユーザーログインは成功します。

#### 手順「選択した MVPD のAdobeからプロファイルリクエストを取得する」 {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass Authentication [ プロファイルリクエスト ](/help/authentication/retrieve-profilerequest.md) サービスを使用して、これを実装します。

>[!TIP]
>
> **<u>ヒント：</u>** ビデオ購読者のアカウントフレームワークから取得されたプロバイダー ID は、Adobe Pass Authentication configuration の *`platformMappingId`* を表していることに注意してください。 そのため、Adobe Pass Authentication [Provide MVPD List](/help/authentication/provide-mvpd-list.md) サービスを通じて、*`platformMappingId`* の値を使用して MVPD id プロパティの値を判断する必要があります。

#### 手順：「Adobeリクエストを Platform SSO に転送してプロファイルを取得する」 {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>ヒント：</u>** ビデオ購読者アカウント [ フレームワークを通じて実装 ](https://developer.apple.com/documentation/videosubscriberaccount) ます。


- アプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
- アプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
- アプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。



>[!TIP]
>
> **<u>プロのヒント：</u>** コードスニペットに従い、コメントに特に注意を払ってください。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 手順：「Platform SSO プロファイルをAdobe認証トークンと交換する」 {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass Authentication [ トークン交換 ](/help/authentication/token-exchange.md) サービスを使用して、これを実装します。


>[!TIP]
>
> **<u>ヒント：</u>**[ 「プロファイルを取得するためにAdobeリクエストを Platform SSO に転送」ステップのコードスニペットに注意してください ](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile) この *`vsaMetadata!.samlAttributeQueryResponse!`* は、[ トークン交換 ](/help/authentication/token-exchange.md) で渡す必要がある *`SAMLResponse`* を表し、呼び出しを行う前に文字列操作とエンコード（*Base64* がエンコードされ、*URL* が後でエンコードされた）が必要です。

</br>

#### 手順：「Adobeトークンは正常に生成されましたか？」 {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>ヒント：</u>** トークンが正常に作成され、認証フローに使用する準備ができていることを示す *`204 No Content`* ールであるAdobe Pass認証 [ トークン交換 ](/help/authentication/token-exchange.md) 成功応答を介して、これを実装します。

</br>

#### 手順：「2 番目の画面認証ワークフローの開始」 {#Initiate_second_screen_authentication_workflow}

**重要：** 「2 番目の画面の認証ワークフロー」という用語は AppleTV に適していますが、「最初の画面の認証ワークフロー」/「通常の認証ワークフロー」という用語は iPhone と iPad に適しています。


>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass認証を使用して、これを実装します

[ 登録コードリクエスト ](/help/authentication/registration-code-request.md)、[ 認証の開始 ](/help/authentication/initiate-authentication.md) および [REST API 認証トークンの取得 ](/help/authentication/retrieve-authentication-token.md) または [ 認証トークンの確認 ](/help/authentication/check-authentication-token.md) サービス。


>[!TIP]
>
> **<u>プロのヒント：</u>** tvOS の実装については、次の手順に従ってください。

- アプリケーションは、[ 登録コードを取得 ](/help/authentication/registration-code-request.md) し、1 番目のデバイス（画面）でエンドユーザーに提示する必要があります。
- アプリケーションは、登録コードが取得された後、1 台目のデバイス（画面）で [ ポーリングして認証状態を確認する ](/help/authentication/retrieve-authentication-token.md) 必要があります。
- 別のアプリケーションは、登録コードが使用される場合、2 番目のデバイス（画面）で [ 認証を開始 ](/help/authentication/initiate-authentication.md) する必要があります。
- 認証トークンが生成されると、アプリケーションは最初のデバイス ](/help/authentication/retrieve-authentication-token.md) 画面）で [ ポーリング）を停止する必要があります。



>[!TIP]
>
> **<u>プロのヒント：</u>** iOS/iPadOS を実装するには、次の手順に従います。

- アプリケーションは、[ 登録コードを取得 ](/help/authentication/registration-code-request.md) する必要があります。このコードは、1 番目のデバイス（画面）でエンドユーザーに表示されるべきではありません。
- アプリケーションは、登録コードと [WKWebView](/help/authentication/initiate-authentication.md) または [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) コンポーネントを使用して、1 番目のデバイス（画面）で [ 認証を開始 ](https://developer.apple.com/documentation/webkit/wkwebview) する必要があります。
- アプリケーションは、[WKWebView[ または [SFSafariViewController](/help/authentication/retrieve-authentication-token.md) コンポーネントが閉じた後、最初のデバイス（画面）で ](https://developer.apple.com/documentation/webkit/wkwebview) 認証状態に関する情報のポーリング ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) を開始する必要があります。
- 認証トークンが生成されると、アプリケーションは最初のデバイス ](/help/authentication/retrieve-authentication-token.md) 画面）で [ ポーリング）を停止する必要があります。

</br>

#### 手順：「認証フローを続行」 {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>ヒント：</u>** これをAdobe Pass認証 [ 認証の開始 ](/help/authentication/initiate-authorization.md) および [ ショートメディアトークンの取得 ](/help/authentication/obtain-short-media-token.md) サービスを通じて実装します。

</br>

### ログアウト {#Logout}

[ ビデオ購読者アカウント ](https://developer.apple.com/documentation/videosubscriberaccount) フレームワークには、デバイスシステムレベルで TV プロバイダーアカウントにログインしたユーザーをプログラムでログアウトする API はありません。 そのため、ログアウトを完全に有効にするには、エンドユーザーはiOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* から明示的にログアウトする必要があります。 ユーザーが持つもう 1 つのオプションは、特定のアプリケーション設定セクション（TV プロバイダーへのアクセス）からユーザーの購読情報にアクセスするための権限を取り消すことです。

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass認証 [ ユーザーメタデータ呼び出し ](/help/authentication/user-metadata.md) および [ ログアウト ](/help/authentication/initiate-logout.md) サービスを使用して、この機能を実装します。


>[!TIP]
>
> **<u>プロのヒント：</u>** tvOS の実装については、次の手順に従ってください。


- アプリケーションは、Adobe Pass Authentication サービスの「*tokenSource」*[ ユーザーメタデータ ](/help/authentication/user-metadata.md) を使用して、Platform SSO を使用したログインの結果として認証が発生したかどうかを判断する必要があります。
- *&quot;tokenSource&quot;* の値が「*Apple **に等しい場合、tvOS の&#x200B;*`Settings -> Accounts -> TV Provider`*で明示的にログアウトするようにユーザーに指示またはプロンプトを表示する必要があります**のみ*。
- アプリケーションは、直接 HTTP 呼び出しを使用して、Adobe Pass Authentication サービスから [ ログアウトを開始 ](/help/authentication/initiate-logout.md) する必要があります。 これは、MVPD 側でのセッションのクリーンアップを容易にするものではありません。



>[!TIP]
>
> **<u>プロのヒント：</u>** iOS/iPadOS を実装するには、次の手順に従います。

- アプリケーションは、Adobe Pass Authentication サービスの「*tokenSource」*[ ユーザーメタデータ ](/help/authentication/user-metadata.md) を使用して、Platform SSO にログインした結果として認証が発生したかどうかを判断する必要があります。
- *&quot;tokenSource&quot;* の値が *&quot;Apple&quot;* に等しい場合、iOS/iPadOS **のみ** で *`Settings -> TV Provider`* から明示的にログアウトするようにユーザーに指示またはプロンプトする必要があります。
- アプリケーションは、[WKWebView[ または [SFSafariViewController](/help/authentication/initiate-logout.md) コンポーネントを使用して、Adobe Pass Authentication サービスから ](https://developer.apple.com/documentation/webkit/wkwebview) ログアウトを開始 ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) する必要があります。 これにより、MVPD 側のセッションクリーンアップが容易になります。

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
