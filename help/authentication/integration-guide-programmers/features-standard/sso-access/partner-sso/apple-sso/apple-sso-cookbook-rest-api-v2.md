---
title: Apple SSO クックブック （REST API V2）
description: Apple SSO クックブック （REST API V2）
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '3908'
ht-degree: 0%
---
# Apple SSO クックブック （REST API V2） {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

Adobe Pass Authentication REST API V2は、iOS、iPadOS、またはtvOSで動作するクライアントアプリケーションのエンドユーザー向けに、パートナーシングルサインオン（SSO）をサポートしています。

このドキュメントは、上位レベルのビューを提供する既存の[REST API V2概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)と、パートナーフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)を使用して[ シングルサインオンを実装する方法を説明するドキュメントの拡張機能として機能します。

## パートナーフローを使用したApple シングルサインオン {#cookbook}

### 前提条件 {#prerequisites}

パートナーフローを使用してApple シングルサインオンを続行する前に、次の前提条件を満たしていることを確認してください。

* ストリーミングアプリケーションは、Adobe Pass Authentication バックエンドがデバイスプラットフォームとその機能を識別できるように、`X-Device-Info`および/または`User-Agent` ヘッダーで必要なすべてのデータを収集する必要があります。 `X-Device-Info` ヘッダーについて詳しくは、[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ドキュメントを参照してください。

* ストリーミングアプリケーションは、デバイスレベルで保存されたユーザーのサブスクリプション情報へのアクセスを要求する必要があります。この場合、ユーザーは、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、続行するためのアプリケーション権限を付与する必要があります。 この権限は、Appleの[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)を使用してアプリケーションごとに要求する必要があり、デバイスはユーザーの選択内容を保存します。

  Apple シングルサインオンのユーザーエクスペリエンスのメリットを説明して、サブスクリプション情報へのアクセスを拒否するユーザーにインセンティブを提供することをお勧めしますが、アプリケーションの設定（TV プロバイダーのアクセス権）に移動するか、iOSおよびiPadOSの&#x200B;*`Settings -> TV Provider`*&#x200B;またはtvOSの&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;に移動して決定を変更できることに注意してください。

  ストリーミングアプリケーションは、ユーザー認証を必要とする前に、任意の時点で[ ユーザーのサブスクリプション情報にアクセスする権限](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)を確認できるため、アプリケーションがフォアグラウンド状態に入ると、ユーザーの権限を要求できます。

>[!IMPORTANT]
>
> 前提条件
>
> <br/>
>
> * ストリーミングアプリケーションは、プログラマーに適用される[ オンボーディングの前提条件](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer)を完了しました。Apple シングルサインオン ユーザーエクスペリエンスを有効にするために必要です。

### ワークフロー {#workflow}

次の図に示すように、パートナーフローを使用してApple シングルサインオンを実装するには、所定の手順を実行します。

![ パートナーフローを使用したApple シングルサインオン ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*パートナーフローを使用したApple シングルサインオン*

+++イ。登録段階

1. **クライアント資格情報の取得：** ストリーミングアプリケーションは、クライアント登録エンドポイントを呼び出して、クライアント資格情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳しくは、[ クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API ドキュメントを参照してください。
   >
   > * `software_statement`など、すべての&#x200B;_必須_ パラメーター
   > * `Content-Type`、`X-Device-Info`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー

1. **クライアント資格情報を返します：** クライアント登録エンドポイントの応答には、受信したパラメーターとヘッダーに関連付けられたクライアント資格情報に関する情報が含まれます。

   >[!IMPORTANT]
   >
   > クライアント認証情報レスポンスで提供される情報の詳細については、[ クライアント認証情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > クライアントレジスタは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[ クライアント資格情報の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API ドキュメントに準拠する追加情報が提供されます。

   >[!TIP]
   >
   > クライアントの資格情報はキャッシュして無期限に使用する必要があります。

1. **アクセストークンの取得：** ストリーミングアプリケーションは、クライアントトークンエンドポイントを呼び出して、アクセストークンを取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳細については、[ アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API ドキュメントを参照してください。
   >
   > * `client_id`、`client_secret`、`grant_type`など、_必須_&#x200B;のすべてのパラメーター
   > * `Content-Type`、`X-Device-Info`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー

1. **戻りアクセストークン：** クライアントトークンエンドポイントの応答には、受信したパラメーターとヘッダーに関連付けられたアクセストークンに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > アクセストークン応答で提供される情報について詳しくは、[ アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > クライアントトークンは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[ アクセストークンの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API ドキュメントに準拠する追加情報が提供されます。

   >[!TIP]
   >
   > アクセストークンはキャッシュされ、指定された期間内（24時間の有効期間など）にのみ使用する必要があります。 有効期限が切れた後、ストリーミングアプリケーションは新しいアクセストークンをリクエストする必要があります。

+++

+++B.認証フェーズの確認

1. **パートナーフレームワークのステータスを取得：** ストリーミングアプリケーションは、Appleによって開発された[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)を呼び出して、ユーザーの権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)のドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーのサブスクリプション情報](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)にアクセスするための[権限を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager`に[ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者アカウント情報に対して[ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーを中断できないことを示すために、`VSAccountMetadataRequest` オブジェクトの[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティに`false`と等しいブール値を指定する必要があります。

   >[!TIP]
   >
   > **<u>プロ向けのヒント：</u>** コードスニペットに従い、コメントに細心の注意を払います。

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
                            // Continue with the "Retrieve profiles" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Retrieve profiles" step.
                            ...
                        }
                    }
   
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Retrieve profiles" step.
                ...
            }
   }
   ...
   ```

1. **パートナーフレームワークのステータス情報を返します：** ストリーミングアプリケーションは、応答データを検証して、基本的な条件が満たされていることを確認します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **プロファイルの取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信して、すべてのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   > 
   > この手順で&#x200B;**必ず**&#x200B;使用するREST API v2 エンドポイントは、次のいずれかです。
   >
   > * [ プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API
   > 
   > または
   > 
   > * [特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md#Request) APIのプロファイルを取得
   >
   > この手順では、**パートナー認証応答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) APIを使用して[ プロファイルを作成および取得する**&#x200B;を使用しないでください。

   >[!IMPORTANT]
   >
   > 詳しくは、[ プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) APIまたは[特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md#Request) API ドキュメントのプロファイルの取得を参照してください。
   >
   > * `serviceProvider` （または`mvpd`）など、すべての&#x200B;_必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier`、`AP-Partner-Framework-Status`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、取得した応答に「appleSSO」タイプのプロファイルが含まれるように、パートナーフレームワークのステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` ヘッダーについて詳しくは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)のドキュメントを参照してください。

1. **見つかったプロファイルに関する情報を返します：** プロファイル エンドポイントの応答には、受信したパラメーターとヘッダーに関連付けられている見つかったプロファイルに関する情報が含まれます。

1. **プロファイルを選択し、決定フローに進みます：** プロファイル エンドポイントの応答にプロファイルが含まれている場合、ストリーミング アプリケーションは内部ロジック（最終的にはエンドユーザーとのやり取り）を使用して、使用可能なプロファイルの1つを選択し、その後の決定フローを続行します。

1. **パートナー認証フローで続行：** プロファイル エンドポイント応答にプロファイルが含まれていない場合、ストリーミング アプリケーションはパートナー認証フローで続行します。

+++

+++C. パートナー認証フェーズ

1. **設定の取得：** ストリーミングアプリケーションは、構成エンドポイントにリクエストを送信することで、アクティブな統合を持つMVPDのリストを取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳細については、[特定のサービスプロバイダー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) APIの設定の取得に関するドキュメントを参照してください。
   >
   > * `serviceProvider`など、すべての&#x200B;_必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier`、`X-Device-Info`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー

1. **戻り値の設定：**&#x200B;設定エンドポイントの応答には、サービスプロバイダーとのアクティブな統合を持つMVPDに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > 設定応答で提供される情報について詳しくは、特定のサービスプロバイダー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) API ドキュメントの[設定の取得を参照してください。
   >
   > <br/>
   >
   > 設定エンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠する追加情報が提供されます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、さらに進む際に、各MVPDに提供される次の詳細を処理する必要があります。
   >
   > * `enablePlatformServices`: MVPDが現在Apple シングルサインオンをサポートしているかどうかを示します。
   > * `displayInPlatformPicker`: MVPDをApple ピッカーに表示できるかどうかを示します。
   > * `boardingStatus`: MVPDがApple シングルサインオンでオンボーディングされているかどうかを示します。

1. **パートナーフレームワークのステータスを取得：** ストリーミングアプリケーションは、Appleによって開発された[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)を呼び出して、ユーザーの権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)のドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーのサブスクリプション情報](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)にアクセスするための[権限を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager`に[ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者アカウント情報に対して[ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、`VSAccountMetadataRequest` オブジェクトの[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティに`true`に等しいブール値を指定し、このフェーズでTV プロバイダーを選択するためにユーザーを中断できることを示す必要があります。

   >[!TIP]
   >
   > **<u>プロ向けのヒント：</u>** コードスニペットに従い、コメントに細心の注意を払います。

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
   
                        // This can be computed from the Configuration service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
   
                        // This can be computed from the Configuration service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
   
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
                                // Continue with the "Retrieve partner authentication request" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
   
                                        // The requestor.mvpds must be computed during the "Return configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
   
                                        if mvpd != nil {
                                            // Continue with the "Proceed with basic authentication flow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Proceed with basic authentication flow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Proceed with basic authentication flow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Proceed with basic authentication flow" step.
                                    ...
                                }
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **パートナーフレームワークのステータス情報を返します：** ストリーミングアプリケーションは、応答データを検証して、基本的な条件が満たされていることを確認します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **パートナー認証要求を取得：** ストリーミングアプリケーションは、Sessions パートナーエンドポイントを呼び出して、認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳細については、[ パートナー認証リクエストの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) API ドキュメントを参照してください。
   >
   > * `serviceProvider`や`partner`など、_必須_&#x200B;のすべてのパラメーター
   > * _必須_ ヘッダー（`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`、`AP-Partner-Framework-Status`など）
   > * すべての&#x200B;_optional_ ヘッダーとパラメーター
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、取得した応答にパートナー認証要求（SAML リクエスト）が含まれるように、パートナーフレームワークのステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` ヘッダーについて詳しくは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)のドキュメントを参照してください。

1. **次のアクションを示します：** セッション パートナーのエンドポイント応答には、次のアクションに関するストリーミングアプリケーションを導くために必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > セッション応答で提供される情報について詳しくは、[ パートナー認証リクエストの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > セッションパートナーエンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   > * 指定された`serviceProvider`と`mvpd`の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 基本的な検証が失敗した場合は、エラー応答が生成され、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠する追加情報が提供されます。
   >
   > <br/>
   >
   > セッションパートナーエンドポイントは、パートナーシングルサインオン条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   >  * Adobe Pass サーバーのパートナーシングルサインオン設定は、有効で有効である必要があります。
   >  * [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ヘッダーを介して受信したパートナーフレームワークの状態ペイロードは有効である必要があります。
   >
   > <br/>
   >
   > パートナーのシングルサインオンの検証が失敗した場合、応答はデフォルトで基本認証フローになります。

1. **決定フローで続行：** セッション パートナーのエンドポイント応答には、次のデータが含まれています。
   * `actionName`属性が「authorize」に設定されています。
   * `actionType`属性が「direct」に設定されています。

   Adobe Pass バックエンドが有効なプロファイルを識別する場合、後続の意思決定フローに使用できるプロファイルが既に存在するため、ストリーミングアプリケーションは選択したMVPDで再認証する必要がありません。

1. **基本認証フローで続行：** セッション パートナーのエンドポイント応答には、次のデータが含まれています。
   * `actionName`属性が「authenticate」または「resume」に設定されています。
   * `actionType`属性が「インタラクティブ」または「ダイレクト」に設定されています。

   Adobe Pass バックエンドで有効なプロファイルが識別されず、パートナーのシングルサインオン検証が失敗した場合、Adobe Pass サーバーは基本認証フローにフォールバックします。

   基本認証フローについて詳しくは、次のドキュメントを参照してください。
   * [プライマリアプリケーション内で認証を実行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択したmvpdを使用して、セカンダリアプリケーション内で認証を実行します](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択したmvpdを使用せずに、セカンダリアプリケーション内で認証を実行します](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **パートナー認証応答フローを使用してプロファイルの作成と取得を進めます：** セッション パートナーエンドポイント応答には、次のデータが含まれます。
   * `actionName`属性が「partner_profile」に設定されています。
   * `actionType`属性が「direct」に設定されています。
   * `authenticationRequest - type`属性には、パートナーフレームワークがMVPD ログインに使用するセキュリティプロトコルが含まれます（現在はSAMLのみ）。
   * `authenticationRequest - request`属性には、パートナーフレームワークに渡されるSAML リクエストが含まれます。
   * `authenticationRequest - attributesNames`属性には、パートナーフレームワークに渡されるSAML属性が含まれます。

   Adobe Pass バックエンドが有効なプロファイルを識別せず、パートナーのシングルサインオン検証が合格した場合、ストリーミングアプリケーションは、MVPDで認証フローを開始するためのパートナーフレームワークに渡すアクションとデータを含む応答を受け取ります。

1. **パートナーフレームワークを使用してMVPD認証を完了：**&#x200B;前の手順で取得したパートナー認証リクエスト （SAML リクエスト）を[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)に転送します。 認証フローが成功すると、MVPDとの[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)のインタラクションにより、パートナーフレームワークのステータス情報とともに返されるパートナー認証応答（SAML応答）が生成されます。

   >[!IMPORTANT]
   >
   > 詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)のドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーのサブスクリプション情報](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)にアクセスするための[権限を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager`に[ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者アカウント情報に対して[要求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)を送信し、前の手順で取得したパートナー認証要求（SAML要求）を含める必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、`VSAccountMetadataRequest` オブジェクトの[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティに`true`に等しいブール値を指定し、このフェーズで選択したTV プロバイダーとの認証をユーザーが中断できることを示す必要があります。

   >[!TIP]
   >
   > **<u>プロ向けのヒント：</u>** コードスニペットに従い、コメントに細心の注意を払います。

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
   
                        // This are the user metadata fields expected to be available on a successful login and are determined from the Sessions SSO service. Look for the authenticationRequest > attributesNames associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = attributesNames;
   
                        // This is the authenticationRequest > request field from Sessions SSO service.
                        vsaMetadataRequest.verificationToken = authenticationRequestPayload;
   
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
   
                                // Continue with the "Create and retrieve profile using partner authentication response" step.
                                ...
                            } else {
                                // Continue with the "Proceed with basic authentication flow" step.
                                ...
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **パートナー認証応答を返します：** ストリーミングアプリケーションは、応答データを検証して、基本的な条件が満たされていることを確認します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。
   * パートナー認証応答（SAML応答）が存在し、有効です。

1. **パートナー認証応答を使用したプロファイルの作成と取得：** ストリーミングアプリケーションは、プロファイル パートナーエンドポイントを呼び出して、プロファイルの作成と取得に必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳細については、[ パートナー認証応答を使用したプロファイルの作成と取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`partner`、`SAMLResponse`など、_必須_&#x200B;のすべてのパラメーター
   > * `Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`、`AP-Partner-Framework-Status`など、_必須_&#x200B;のすべてのヘッダー
   > * すべての&#x200B;_optional_ ヘッダーとパラメーター
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、取得した応答に「appleSSO」タイプのプロファイルが含まれるように、パートナーフレームワークのステータスとパートナー認証応答（SAML応答）の有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` ヘッダーについて詳しくは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)のドキュメントを参照してください。

1. **パートナープロファイルに関する情報を返します：** プロファイルエンドポイントの応答には、パートナープロファイルに関する情報が含まれます。これには、属性`type`が「appleSSO」に設定されていることが含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[ パートナー認証応答を使用したプロファイルの作成と取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > プロファイルパートナーエンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   > * 指定された`serviceProvider`と`mvpd`の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠する追加情報が提供されます。
   >
   > <br/>
   >
   > プロファイルパートナーエンドポイントは、パートナーシングルサインオン条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   >  * Adobe Pass サーバーのパートナーシングルサインオン設定は、有効で有効である必要があります。
   >  * [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ヘッダーを介して受信したパートナーフレームワークの状態ペイロードは有効である必要があります。
   >
   > <br/>
   >
   > パートナーシングルサインオンの検証が失敗した場合、応答はデフォルトで基本プロファイル取得フローになります。

1. **決定フローで続行：** ストリーミングアプリケーションは、後続の決定フローで続行できます。

+++

+++ D.決定段階

1. **パートナーフレームワークのステータスを取得：** ストリーミングアプリケーションは、Appleによって開発された[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)を呼び出して、ユーザーの権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   > 
   > 選択したユーザープロファイルタイプが「appleSSO」でない場合、ストリーミングアプリケーションはこの手順をスキップできます。

   >[!IMPORTANT]
   >
   > 詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)のドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーのサブスクリプション情報](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)にアクセスするための[権限を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager`に[ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者アカウント情報に対して[ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーを中断できないことを示すために、`VSAccountMetadataRequest` オブジェクトの[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティに`false`と等しいブール値を指定する必要があります。

   >[!TIP]
   >
   > ストリーミングアプリケーションでは、代わりにパートナーフレームワークのステータス情報にキャッシュ値を使用できます。これは、アプリケーションがバックグラウンドからフォアグラウンド状態に移行する際に更新することをお勧めします。 その場合、ストリーミングアプリケーションは、「パートナーフレームワークのステータス情報を返す」ステップで説明されているように、パートナーフレームワークのステータスに対して有効な値のみをキャッシュして使用する必要があります。

1. **パートナーフレームワークのステータス情報を返します：** ストリーミングアプリケーションは、応答データを検証して、基本的な条件が満たされていることを確認します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限が有効です。

   >[!IMPORTANT]
   >
   > 選択したユーザープロファイルタイプが「appleSSO」でない場合、ストリーミングアプリケーションはこの手順をスキップできます。

1. **事前認証の決定を取得：** ストリーミングアプリケーションは、「決定の事前認証エンドポイント」を呼び出して、リソースのリストに対する事前認証の決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳しくは、[特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API ドキュメントを使用した事前承認決定の取得を参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources`など、_必須_&#x200B;のすべてのパラメーター
   > * `Authorization`や`AP-Device-Identifier`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー
   >
   > <br/>
   >
   > 選択したプロファイルが「appleSSO」タイプのプロファイルである場合、リクエストを実行する前に、ストリーミングアプリケーションにパートナーフレームワークのステータスの有効な値が含まれていることを確認する必要があります。 ただし、選択したユーザープロファイルタイプが「appleSSO」でない場合、この手順はスキップできます。
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` ヘッダーについて詳しくは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)のドキュメントを参照してください。

1. **事前承認の決定を返します：** 「決定事前承認」エンドポイント応答には、各リソースに対する`Permit`または`Deny`の決定が含まれています。
   * `Permit`の決定は、リソースが再生可能であることを意味します。 応答にはメディアトークンが含まれていません。事前承認フローを使用してリソースを再生することはできません。
   * `Deny`の決定は、リソースが再生できないことを意味します。 応答には、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠するエラーペイロードが含まれます。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API ドキュメントを使用した事前承認決定の取得を参照してください。
   >
   > <br/>
   >
   > 決定事前認証エンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   > * 指定された`serviceProvider`と`mvpd`の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠する追加情報が提供されます。

1. **パートナーフレームワークのステータスを取得：** ストリーミングアプリケーションは、Appleによって開発された[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)を呼び出して、ユーザーの権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 選択したユーザープロファイルタイプが「appleSSO」でない場合、ストリーミングアプリケーションはこの手順をスキップできます。

   >[!IMPORTANT]
   >
   > 詳しくは、[ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount)のドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーのサブスクリプション情報](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)にアクセスするための[権限を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager`に[ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者アカウント情報に対して[ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーを中断できないことを示すために、`VSAccountMetadataRequest` オブジェクトの[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティに`false`と等しいブール値を指定する必要があります。

   >[!TIP]
   >
   > ストリーミングアプリケーションでは、代わりにパートナーフレームワークのステータス情報にキャッシュ値を使用できます。これは、アプリケーションがバックグラウンドからフォアグラウンド状態に移行する際に更新することをお勧めします。 その場合、ストリーミングアプリケーションは、「パートナーフレームワークのステータス情報を返す」ステップで説明されているように、パートナーフレームワークのステータスに対して有効な値のみをキャッシュして使用する必要があります。

1. **パートナーフレームワークのステータス情報を返します：** ストリーミングアプリケーションは、応答データを検証して、基本的な条件が満たされていることを確認します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限が有効です。

   >[!IMPORTANT]
   >
   > 選択したユーザープロファイルタイプが「appleSSO」でない場合、ストリーミングアプリケーションはこの手順をスキップできます。

1. **承認決定の取得：** ストリーミングアプリケーションは、「決定の承認」エンドポイントを呼び出して、特定のリソースの承認決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳しくは、特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) API ドキュメントを使用した承認決定の取得を参照してください。[
   >
   > * `serviceProvider`、`mvpd`、`resources`など、_必須_&#x200B;のすべてのパラメーター
   > * `Authorization`や`AP-Device-Identifier`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー
   >
   > <br/>
   >
   > 選択したプロファイルが「appleSSO」タイプのプロファイルである場合、リクエストを実行する前に、ストリーミングアプリケーションにパートナーフレームワークのステータスの有効な値が含まれていることを確認する必要があります。 ただし、選択したユーザープロファイルタイプが「appleSSO」でない場合、この手順はスキップできます。
   >
   > <br/>
   >
   > `AP-Partner-Framework-Status` ヘッダーについて詳しくは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)のドキュメントを参照してください。

1. **返品承認決定：**&#x200B;決定承認エンドポイント応答には、特定のリソースに対する`Permit`または`Deny`の決定が含まれています：
   * `Permit`の決定は、リソースが再生可能であることを意味します。 応答には、メディアトークンが含まれます。
   * `Deny`の決定は、リソースが再生できないことを意味します。 応答には、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠するエラーペイロードが含まれます。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API ドキュメントを使用した承認決定の取得を参照してください。
   >
   > <br/>
   >
   > 決定承認エンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   > * 指定された`serviceProvider`と`mvpd`の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠する追加情報が提供されます。

+++

+++ D. ログアウトフェーズ

1. **Adobe Pass ログアウトの開始：** ストリーミング アプリケーションは、Adobe Pass ログアウトエンドポイントを呼び出して、ログアウトフローを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳しくは、[特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) API ドキュメントのログアウトの開始を参照してください。
   >
   > * `serviceProvider`、`mvpd`、`redirectUrl`など、_必須_&#x200B;のすべてのパラメーター
   > * `Authorization`、`AP-Device-Identifier`など、_必須_ ヘッダーすべて
   > * すべての&#x200B;_optional_ パラメーターとヘッダー

1. **次のアクションを示します。** Adobe Pass ログアウトエンドポイントの応答には、次のアクションに関するストリーミングアプリケーションをガイドするために必要なデータが含まれています。
   * ユーザーがログアウトフローを完了するにはパートナー（システム）レベルとやり取りする必要があるため、`url`属性がありません。
   * `actionName`属性が「partner_logout」に設定されています。
   * `actionType`属性が「partner_interactive」に設定されています。

   >[!IMPORTANT]
   >
   > 削除されたユーザープロファイルタイプが「appleSSO」の場合、ストリーミングアプリケーションは、`actionName`および`actionType`属性で指定されているように、パートナーレベルでログアウトプロセスを完了するようにユーザーに促す必要があります。

   >[!IMPORTANT]
   >
   > ログアウト応答で提供される情報について詳しくは、特定のmvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API ドキュメントの[ ログアウトの開始を参照してください。
   >
   > <br/>
   >
   > Adobe Pass ログアウトエンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _必須_ パラメーターとヘッダーは有効である必要があります。
   > * 指定された`serviceProvider`と`mvpd`の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証が失敗すると、エラー応答が生成され、[拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)のドキュメントに準拠する追加情報が提供されます。

+++
