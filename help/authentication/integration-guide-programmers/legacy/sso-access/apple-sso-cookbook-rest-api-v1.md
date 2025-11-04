---
title: Apple SSO クックブック（REST API V1）
description: Apple SSO クックブック（REST API V1）
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# （従来の）Apple SSO クックブック（REST API V1） {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

Adobe Pass認証 REST API V1 は、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザー向けに、パートナーシングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の REST API V1 ドキュメントの拡張機能として機能します。このドキュメントは [&#x200B; こちら &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md) で参照できます。

## クックブック {#apple-sso-cookbook-rest-api-v1-cookbook}

Apple SSO のユーザーエクスペリエンスを活用するには、Appleが開発した [&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) を組み込む必要があります。一方、Adobe Pass認証 REST API V1 通信の場合は、以下に示す一連の手順に従う必要があります。

### 権限 {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>ヒント：</u>** ストリーミングアプリケーションは、デバイスレベルで保存されたユーザーの購読情報へのアクセスをリクエストする必要があります。これに対して、ユーザーは、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、続行する権限をアプリケーションに与える必要があります。 この権限は、Apple[&#x200B; ビデオ購読者のアカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) を使用しているアプリケーションごとにリクエストされる必要があります。

>[!TIP]
>
> **<u>ヒント：</u>** Appleのシングルサインオンユーザーエクスペリエンスの利点を説明することで、サブスクリプション情報へのアクセスを拒否するユーザーをインセンティブすることをお勧めしますが、アプリ設定（TV プロバイダーの権限アクセス）またはiOSと iPadOS または tvOS の *`Settings -> TV Provider`* に移動することで、ユーザーの判断を変 *`Settings -> Accounts -> TV Provider`* ることができます。

>[!TIP]
>
> **<u>ヒント：</u>** アプリケーションがフォアグラウンド状態になると、ユーザー認証を要求する前にいつでもユーザーの購読情報に対する [&#x200B; アクセス許可 &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認できるので、ユーザーの許可を要求することをお勧めします。

### 認証 {#apple-sso-cookbook-rest-api-v1-authentication}

* [有効なAdobe認証トークンはありますか？](#step1)
* [ユーザーはパートナー SSO を使用してログインしていますか？](#step2)
* [Adobe設定の取得](#step3)
* [Adobe設定を使用したパートナー SSO ワークフローの開始](#step4)
* [ユーザーログインに成功しましたか？](#step5)
* [選択したMVPDのAdobeからのプロファイルリクエストの取得](#step6)
* [Adobe リクエストをパートナー SSO に転送してプロファイルを取得](#step7)
* [パートナー SSO プロファイルをAdobe認証トークンと交換します](#step8)
* [Adobe トークンは正常に生成されましたか？](#step9)
* [通常の認証ワークフローの開始](#step10)
* [承認フローの続行](#step11)

![](/help/authentication/assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### 手順：「有効なAdobe認証トークンがあるか」 {#step1}

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass認証 [&#x200B; 認証トークンを確認 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)API サービスを使用して、これを実装します。

#### 手順：「ユーザーはパートナー SSO を使用してログインしていますか？」 {#step2}

>[!TIP]
>
> **<u>ヒント：</u>** [&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) のメディアを使用して、これを実装します。

* アプリケーションは、ユーザーの購読情報 [&#x200B; アクセス権限 &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
* アプリケーションは、購読者のアカウント情報用に [&#x200B; リクエスト &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
* アプリケーションは、[&#x200B; メタデータ &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。

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
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### 手順：「Adobe設定を取得」 {#step3}

>[!TIP]
>
> **<u>ヒント：</u>** これをAdobe Pass認証 [MVPD リストの提供 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) API サービスで実装します。

>[!TIP]
>
> **<u>ヒント：</u>** MVPDのプロパティ *`enablePlatformServices`*、*`boardingStatus`*、*`displayInPlatformPicker`*、*`platformMappingId`*、*`requiredMetadataFields`* に注意し、他の手順のコードスニペットに表示されるコメントには特に注意してください。

#### 手順「Adobe設定を使用したパートナー SSO ワークフローの開始」 {#step4}

>[!TIP]
>
> **<u>ヒント：</u>** [&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) のメディアを使用して、これを実装します。

* アプリケーションは、ユーザーの購読情報 [&#x200B; アクセス権限 &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
* アプリケーションは、VSAccountManager に [delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
* アプリケーションは、購読者のアカウント情報用に [&#x200B; リクエスト &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
* アプリケーションは、[&#x200B; メタデータ &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。

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
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### 手順：「ユーザーログインに成功しましたか？」 {#step5}

>[!TIP]
>
> **<u>ヒント：</u>**&#x200B;[&#x200B; 「Adobe設定でパートナー SSO ワークフローを開始する」 &#x200B;](#step4) ステップのコードスニペットに注意してください。 *`vsaMetadata!.accountProviderIdentifier`* に有効な値が含まれ、現在の日付が *`vsaMetadata!.authenticationExpirationDate`* 値を渡していない場合、ユーザーログインは成功します。

#### 手順「選択したMVPDのAdobeからプロファイルリクエストを取得」 {#step6}

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass Authentication [&#x200B; プロファイルリクエスト &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md) API サービスを使用して、これを実装します。

>[!TIP]
>
> **<u>ヒント：</u>** ビデオ購読者のアカウントフレームワークから取得されたプロバイダー ID は、Adobe Pass Authentication configuration の *`platformMappingId`* を表していることに注意してください。 そのため、アプリケーションは、Adobe Pass Authentication *`platformMappingId`* MVPD List の提供 [&#x200B; API サービスを通じて、](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) 値を使用してMVPD ID プロパティ値を決定する必要があります。

#### 手順：「Adobe リクエストをパートナー SSO に転送してプロファイルを取得する」 {#step7}

>[!TIP]
>
> **<u>ヒント：</u>** [&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) のメディアを使用して、これを実装します。


* アプリケーションは、ユーザーの購読情報 [&#x200B; アクセス権限 &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
* アプリケーションは、購読者のアカウント情報用に [&#x200B; リクエスト &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
* アプリケーションは、[&#x200B; メタデータ &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。

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
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### 手順：「パートナー SSO プロファイルをAdobe認証トークンと交換する」 {#step8}

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass Authentication [&#x200B; トークン交換 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) API サービスを使用して、これを実装します。

>[!TIP]
>
> **<u>ヒント：</u>**&#x200B;[&#x200B; 「Adobe リクエストをパートナー SSO に転送してプロファイルを取得する」 &#x200B;](#step7) 手順のコードスニペットに注意してください。 この *`vsaMetadata!.samlAttributeQueryResponse!`* は、*`SAMLResponse`* トークン交換 [&#x200B; で渡す必要がある &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) を表し、呼び出しを行う前に文字列操作とエンコード（*Base64* エンコードされ、*URL* 後でエンコードされた）が必要です。

#### 手順：「Adobe トークンは正常に生成されましたか？」 {#step9}

>[!TIP]
>
> **<u>ヒント：</u>** トークンが正常に作成され、認証フローに使用する準備ができていることを示す [&#x200B; ールであるAdobe Pass Authentication &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) トークン交換 *`204 No Content`* successful response を介して、これを実装します。

#### 手順：「通常の認証ワークフローの開始」 {#step10}

>[!TIP]
>
> **<u>ヒント：</u>** これを実装するには、Adobe Pass Authentication [Registration Code Request](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)、[Initiate Authentication](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) および [Retrieve Authentication Token](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) または [Check Authentication Token](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) API サービスを使用します。

>[!TIP]
>
> **<u>プロのヒント：</u>** tvOS の実装については、次の手順に従ってください。

* アプリケーションは、[&#x200B; 登録コードを取得 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) し、1 番目のデバイス（画面）でエンドユーザーに提示する必要があります。
* アプリケーションは、登録コードが取得された後、1 台目のデバイス（画面）で [&#x200B; ポーリングして認証状態を確認する &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) 必要があります。
* 別のアプリケーションは、登録コードが使用される場合、2 番目のデバイス（画面）で [&#x200B; 認証を開始 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) する必要があります。
* 認証トークンが生成されると、アプリケーションは最初のデバイス [&#x200B; 画面）で &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) ポーリング）を停止する必要があります。

>[!TIP]
>
> **<u>プロのヒント：</u>** iOS/iPadOS を実装するには、次の手順に従います。

* アプリケーションは、[&#x200B; 登録コードを取得 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) する必要があります。このコードは、1 番目のデバイス（画面）でエンドユーザーに表示されるべきではありません。
* アプリケーションは、登録コードと [WKWebView](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) または [SFSafariViewController](https://developer.apple.com/documentation/webkit/wkwebview) コンポーネントを使用して、1 番目のデバイス（画面）で [&#x200B; 認証を開始 &#x200B;](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) する必要があります。
* アプリケーションは、[WKWebView](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) または [SFSafariViewController](https://developer.apple.com/documentation/webkit/wkwebview) コンポーネントが閉じた後、最初のデバイス（画面）で [&#x200B; 認証状態に関する情報のポーリング &#x200B;](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) を開始する必要があります。
* 認証トークンが生成されると、アプリケーションは最初のデバイス [&#x200B; 画面）で &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) ポーリング）を停止する必要があります。

#### 手順：「認証フローを続行」 {#step11}

>[!TIP]
>
> **<u>ヒント：</u>** これをAdobe Pass認証 [&#x200B; 認証の開始 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) および [&#x200B; ショートメディアトークンの取得 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)API サービスを通じて実装します。

### ログアウト {#apple-sso-cookbook-rest-api-v1-logout}

[&#x200B; ビデオ購読者アカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) には、デバイスシステムレベルで TV プロバイダーアカウントにログインしたユーザーをプログラムでログアウトする API はありません。 そのため、ログアウトを完全に有効にするには、エンドユーザーはiOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* から明示的にログアウトする必要があります。 ユーザーが持つもう 1 つのオプションは、特定のアプリケーション設定セクション（TV プロバイダーへのアクセス）からユーザーの購読情報にアクセスするための権限を取り消すことです。

>[!TIP]
>
> **<u>ヒント：</u>** Adobe Pass認証 [&#x200B; ユーザーメタデータ呼び出し &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) および [&#x200B; ログアウト &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) API サービスを使用して、これを実装します。

>[!TIP]
>
> **<u>プロのヒント：</u>** tvOS の実装については、次の手順に従ってください。

* アプリケーションは、Adobe Pass Authentication サービスの「*tokenSource」*[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) を使用して、認証がパートナー SSO を介したログインの結果として発生したかどうかを判断する必要があります。
* *`Settings -> Accounts -> TV Provider`*&quot;tokenSource&quot;**の値が「** Apple *に等しい場合、tvOS の* で明示的にログアウトするようにユーザーに指示またはプロンプトを表示する必要があります *のみ*。
* アプリケーションは、直接 HTTP 呼び出しを使用して、Adobe Pass Authentication サービスから [&#x200B; ログアウトを開始 &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) する必要があります。 これは、MVPD側でのセッションクリーンアップを容易にするものではありません。

>[!TIP]
>
> **<u>プロのヒント：</u>** iOS/iPadOS を実装するには、次の手順に従います。

* アプリケーションは、Adobe Pass Authentication サービスの「*tokenSource」*[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) を使用して、認証がパートナー SSO を介したログインの結果として発生したかどうかを判断する必要があります。
* *`Settings -> TV Provider`*&quot;tokenSource&quot;**の値が**&quot;Apple&quot;*に等しい場合、iOS/iPadOS* のみ *で* から明示的にログアウトするようにユーザーに指示またはプロンプトする必要があります。
* アプリケーションは、[WKWebView](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) または [SFSafariViewController](https://developer.apple.com/documentation/webkit/wkwebview) コンポーネントを使用して、Adobe Pass Authentication サービスから [&#x200B; ログアウトを開始 &#x200B;](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) する必要があります。 これにより、MVPD側のクリーンアップが容易になります。
