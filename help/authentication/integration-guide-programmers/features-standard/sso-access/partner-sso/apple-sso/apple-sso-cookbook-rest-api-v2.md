---
title: Apple SSO クックブック（REST API V2）
description: Apple SSO クックブック（REST API V2）
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '3443'
ht-degree: 0%

---

# Apple SSO クックブック（REST API V2） {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

Adobe Pass認証 REST API V2 は、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザー向けに、パートナーシングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の [REST API V2 概要の拡張機能として機能し ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) 概要の概要と、[ パートナーフローを使用したシングルサインオン ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) を実装する方法を説明するドキュメントを提供します。

## パートナーフローを使用したAppleのシングルサインオン {#cookbook}

### 前提条件 {#prerequisites}

パートナーフローを使用してAppleのシングルサインオンを続行する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションは、Adobe Pass Authentication バックエンドがデバイスプラットフォームとその機能を識別できるように、`X-Device-Info` や `User-Agent` ヘッダーで必要なすべてのデータを収集する必要があります。 ヘッダーについて詳 `X-Device-Info` くは、[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) ドキュメントを参照してください。

* ストリーミングアプリケーションは、デバイスレベルで保存されたユーザーの購読情報へのアクセスをリクエストする必要があります。これに対して、ユーザーは、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、続行を許可するアプリケーション権限を付与する必要があります。 この権限は、Apple[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を使用しているアプリケーションごとにリクエストされる必要があります。

  Appleのシングルサインオンユーザーエクスペリエンスの利点を説明することで、購読情報へのアクセスを拒否するユーザーにインセンティブを与えることをお勧めしますが、アプリ設定（TV プロバイダーのアクセス）またはiOSと iPadOS または tvOS の *`Settings -> Accounts -> TV Provider`* を使用する *`Settings -> TV Provider`* とで、ユーザーが判断を変えることができることに注意してください。

  ストリーミングアプリケーションは、アプリケーションがフォアグラウンド状態になると、ユーザー認証を要求する前の任意の時点でユーザーの購読情報に対する [ アクセス許可 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認できるので、ユーザーの許可を要求できます。

>[!IMPORTANT]
>
> 前提
>
> <br/>
>
> * ストリーミングアプリケーションは、プログラマーに適用され、Appleのシングルサインオンユーザーエクスペリエンスを有効にするために必要な [ オンボーディングの前提条件 ](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) を完了しています。

### ワークフロー {#workflow}

次の図に示すパートナーフローを使用してAppleのシングルサインオンを実装するには、指定された手順を実行します。

![ パートナーフローを使用したAppleのシングルサインオン ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*パートナーフローを使用したAppleのシングルサインオン*

+++A.登録フェーズ

1. **クライアント資格情報の取得：** ストリーミングアプリケーションは、クライアント登録エンドポイントを呼び出してクライアント資格情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ クライアント資格情報の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API ドキュメントを参照してください。
   >
   > * `software_statement` のようなすべての _必須_ パラメーター
   > * `Content-Type`、`X-Device-Info` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **クライアント資格情報を返す：** クライアント登録エンドポイント応答には、受信したパラメーターおよびヘッダーに関連付けられたクライアント資格情報に関する情報が含まれます。

   >[!IMPORTANT]
   >
   > クライアント資格情報応答で提供される情報について詳しくは、[ クライアント資格情報の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > クライアントレジスタがリクエストデータを検証し、基本的な条件が満たされていることを確認します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ クライアント資格情報の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API ドキュメントに従った追加情報が提供されます。

   >[!TIP]
   >
   > 提案：クライアントの資格情報はキャッシュする必要があり、無期限に使用される可能性があります。

1. **アクセストークンを取得：** ストリーミングアプリケーションは、クライアントトークンエンドポイントを呼び出してアクセストークンを取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ アクセストークンの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API ドキュメントを参照してください。
   >
   > * `client_id`、`client_secret`、`grant_type` など、すべての _必須_ パラメーター
   > * `Content-Type`、`X-Device-Info` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **アクセストークンを返す：** クライアントトークンエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられたアクセストークンに関する情報が含まれています。

   >[!IMPORTANT]
   >
   > アクセストークン応答で提供される情報について詳しくは、[ アクセストークンの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > クライアントトークンは、リクエストデータを検証して、基本的な条件が満たされていることを確認します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ アクセストークンの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API ドキュメントに準拠する追加情報が提供されます。

   >[!TIP]
   >
   > 提案：アクセストークンは、指定された期間（例：24 時間の有効期間）内にのみキャッシュおよび使用する必要があります。 有効期限が切れた後、ストリーミングアプリケーションは新しいアクセストークンをリクエストする必要があります。

+++

+++B.認証フェーズを確認

1. **パートナーフレームワークのステータスの取得：** ストリーミングアプリケーションは、Appleが開発した [ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を呼び出して、ユーザー権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) ドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager` に [ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーを中断できないことを示すために、必ず `VSAccountMetadataRequest` オブジェクトの [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティのブール値が `false` に等しいことを指定する必要があります。

1. **パートナーフレームワークのステータス情報を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **プロファイルの取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、すべてのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ プロファイルの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API ドキュメントを参照してください。
   >
   > * `serviceProvider` のようなすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier`、`AP-Partner-Framework-Status` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、取得した応答に「appleSSO」タイプのプロファイルを含められるように、パートナーフレームワークステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **見つかったプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた、見つかったプロファイルに関する情報が含まれます。

1. **プロファイルを選択し、決定フローを続行：** プロファイルエンドポイント応答にプロファイルが含まれている場合、ストリーミングアプリケーションは（最終的にエンドユーザーとやり取りすることで）内部ロジックを使用して、使用可能なプロファイルの 1 つを選択し、後続の決定フローを続行します。

1. **パートナー認証フローを続行：** プロファイルエンドポイント応答にプロファイルが含まれていない場合、ストリーミングアプリケーションはパートナー認証フローを続行します。

+++

+++C.パートナー認証フェーズ

1. **設定の取得：** ストリーミングアプリケーションは、設定エンドポイントにリクエストを送信して、アクティブな統合を持つ MVPD のリストを取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定のサービスプロバイダーの設定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request)API ドキュメントを参照してください。
   >
   > * `serviceProvider` のようなすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier`、`X-Device-Info` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **設定を返す：** 設定エンドポイント応答には、サービスプロバイダーとアクティブに統合されている MVPD に関する情報が含まれています。

   >[!IMPORTANT]
   >
   > 設定応答で提供される情報について詳しくは、[ 特定のサービスプロバイダーの設定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response)API ドキュメントを参照してください。
   >
   > <br/>
   >
   > 設定エンドポイントは、基本条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、先に進む際に、各MVPDに提供される次の詳細を処理する必要があります。
   >
   > * `enablePlatformServices`:MVPDが現在Appleのシングルサインオンをサポートしているかどうかを示します。
   > * `displayInPlatformPicker`:AppleピッカーでMVPDを表示できるかどうかを示します。
   > * `boardingStatus`:MVPDがAppleのシングルサインオンでオンボードされているかどうかを示します。

1. **パートナーフレームワークのステータスの取得：** ストリーミングアプリケーションは、Appleが開発した [ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を呼び出して、ユーザー権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) ドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager` に [ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、ユーザーがこの段階で TV プロバイダーを選択するために中断できることを示すために、`VSAccountMetadataRequest` オブジェクトの [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティのブール値が `true` に等しいことを指定する必要があります。

1. **パートナーフレームワークのステータス情報を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **パートナー認証リクエストの取得：** ストリーミングアプリケーションは、セッションパートナーエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ パートナー認証リクエストの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) API ドキュメントを参照してください。
   >
   > * `serviceProvider` や `partner` など、すべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`、`AP-Partner-Framework-Status` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ ヘッダーとパラメーター
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、取得した応答にパートナー認証要求（SAML 要求）が含まれるように、パートナーフレームワークステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **次のアクションを示す：** セッションパートナーエンドポイント応答には、次のアクションに関するストリーミングアプリケーションをガイドするために必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > セッション応答で提供される情報について詳しくは、[ パートナー認証リクエストの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > セッションパートナーエンドポイントは、基本条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 基本検証が失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   >
   > セッションパートナーエンドポイントは、リクエストデータを検証して、パートナーのシングルサインオン条件が満たされていることを確認します。
   >
   >  * Adobe Pass サーバーのパートナーのシングルサインオン設定が有効であり、有効である必要があります。
   >  * [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ヘッダーを介して受信したパートナーフレームワークステータスペイロードは、有効である必要があります。
   >
   > <br/>
   >
   > パートナーのシングルサインオン検証が失敗した場合、応答はデフォルトで基本認証フローに設定されます。

1. **決定フローで続行：** セッションパートナーエンドポイント応答には、次のデータが含まれます。
   * `actionName` 属性は「authorize」に設定されます。
   * `actionType` 属性は「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを特定した場合、その後の決定フローに使用できるプロファイルが既に存在するので、ストリーミングアプリケーションは選択したMVPDで再認証する必要はありません。

1. **基本認証フローを続行：** セッションパートナーエンドポイント応答には、次のデータが含まれています。
   * `actionName` 属性は、「authenticate」または「resume」に設定されます。
   * `actionType` 属性は、「interactive」または「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別せず、パートナーのシングルサインオン検証に失敗した場合、Adobe Pass サーバーは基本認証フローにフォールバックします。

   基本認証フローについて詳しくは、次のドキュメントを参照してください。
   * [プライマリアプリケーション内での認証の実行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **パートナー認証応答フローを使用したプロファイルの取得に進みます：** セッションパートナーエンドポイント応答には、次のデータが含まれています。
   * `actionName` 属性は「partner_profile」に設定されます。
   * `actionType` 属性は「direct」に設定されます。
   * `authenticationRequest - type` 属性には、MVPD ログイン用にパートナーフレームワークが使用するセキュリティプロトコルが含まれます（現在、SAML のみに設定されています）。
   * `authenticationRequest - request` 属性には、パートナーフレームワークに渡される SAML リクエストが含まれます。
   * `authenticationRequest - attributesNames` 属性には、パートナーフレームワークに渡される SAML 属性が含まれます。

   Adobe Pass バックエンドが有効なプロファイルを識別せず、パートナーのシングルサインオン検証に合格した場合、ストリーミングアプリケーションはアクションとデータを含む応答を受け取り、MVPDとの認証フローを開始するためにパートナーフレームワークに渡します。

1. **パートナーフレームワークでMVPD認証を完了：** 前の手順で取得したパートナー認証要求（SAML 要求）を [ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) に転送します。 認証フローが成功すると、MVPDとの [ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) インタラクションによってパートナー認証応答（SAML 応答）が生成され、パートナーフレームワークのステータス情報と共に返されます。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) ドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager` に [ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があり、さらに、前の手順で取得したパートナー認証要求（SAML 要求）を含める必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーが選択された TV プロバイダーによる認証を中断できることを示すために、`VSAccountMetadataRequest` オブジェクトの [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティのブール値が `true` に等しいことを指定する必要があります。

1. **パートナー認証応答を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。
   * パートナー認証応答（SAML 応答）が存在し、有効です。

1. **パートナー認証応答を使用したプロファイルの取得：** ストリーミングアプリケーションは、プロファイルパートナーエンドポイントを呼び出してプロファイルを作成および取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ パートナー認証応答を使用したプロファイルの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request)API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`partner`、`SAMLResponse` など、すべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`、`AP-Partner-Framework-Status` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ ヘッダーとパラメーター
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、取得した応答に「appleSSO」タイプのプロファイルを含められるように、パートナーフレームワークステータスおよびパートナー認証応答（SAML 応答）の有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **パートナープロファイルに関する情報を返す：** プロファイルエンドポイント応答には、「appleSSO」に設定されている属性 `type` ど、パートナープロファイルに関する情報が含まれています。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[ パートナー認証応答を使用したプロファイルの取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response)API ドキュメントを参照してください。
   >
   > <br/>
   >
   > プロファイルパートナーエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   >
   > プロファイルパートナーエンドポイントは、リクエストデータを検証して、パートナーのシングルサインオン条件が満たされていることを確認します。
   >
   >  * Adobe Pass サーバーのパートナーのシングルサインオン設定が有効であり、有効である必要があります。
   >  * [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ヘッダーを介して受信したパートナーフレームワークステータスペイロードは、有効である必要があります。
   >
   > <br/>
   >
   > パートナーのシングルサインオン検証が失敗した場合、応答はデフォルトで基本プロファイル取得フローに設定されます。

1. **決定フローで続行：** ストリーミングアプリケーションは、後続の決定フローで続行できます。

+++

+++ D.決定フェーズ

1. **パートナーフレームワークのステータスの取得：** ストリーミングアプリケーションは、Appleが開発した [ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を呼び出して、ユーザー権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) ドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager` に [ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーを中断できないことを示すために、必ず `VSAccountMetadataRequest` オブジェクトの [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティのブール値が `false` に等しいことを指定する必要があります。

   >[!TIP]
   > 
   > 提案：ストリーミングアプリケーションでは、パートナーフレームワークのステータス情報にキャッシュされた値を使用できます。アプリケーションがバックグラウンド状態からフォアグラウンド状態に移行する際には、更新することを推奨します。

1. **パートナーフレームワークのステータス情報を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **事前認証決定の取得：** ストリーミングアプリケーションは、決定の事前認証エンドポイントを呼び出すことにより、リソースのリストに対する事前認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd を使用した事前認証の決定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   >
   > 選択したプロファイルが「appleSSO」タイプのプロファイルの場合、ストリーミングアプリケーションがリクエストを送信する前に、パートナーフレームワークステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **再来訪の事前認証の決定：** 決定の事前認証エンドポイント応答には、各リソースの `Permit` または `Deny` の決定が含まれています。
   * `Permit` の決定は、リソースが再生可能であることを意味します。 事前認証フローはリソースの再生に使用できないので、応答にはメディアトークンが含まれていません。
   * `Deny` の決定は、リソースが再生可能でないことを意味します。 応答には、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従ったエラーペイロードが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した事前認証の決定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > 決定の事前認証エンドポイントは、基本条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **パートナーフレームワークのステータスの取得：** ストリーミングアプリケーションは、Appleが開発した [ ビデオ購読者アカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を呼び出して、ユーザー権限とプロバイダー情報を取得します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) ドキュメントを参照してください。
   >
   > <br/>
   >
   > * ストリーミングアプリケーションは、ユーザーの購読情報 [ アクセス権限 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認し、ユーザーが許可した場合にのみ続行する必要があります。
   > * ストリーミングアプリケーションは、`VSAccountManager` に [ デリゲート ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) を提供する必要があります。
   > * ストリーミングアプリケーションは、購読者のアカウント情報用に [ リクエスト ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) を送信する必要があります。
   > * ストリーミングアプリケーションは、[ メタデータ ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata) 情報を待機して処理する必要があります。
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、このフェーズでユーザーを中断できないことを示すために、必ず `VSAccountMetadataRequest` オブジェクトの [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) プロパティのブール値が `false` に等しいことを指定する必要があります。

   >[!TIP]
   >
   > 提案：ストリーミングアプリケーションでは、パートナーフレームワークのステータス情報にキャッシュされた値を使用できます。アプリケーションがバックグラウンド状態からフォアグラウンド状態に移行する際には、更新することを推奨します。

1. **パートナーフレームワークのステータス情報を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd を使用した認証決定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   >
   > 選択したプロファイルが「appleSSO」タイプのプロファイルの場合、ストリーミングアプリケーションがリクエストを送信する前に、パートナーフレームワークステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   >
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **再来訪承認決定：** 決定の承認エンドポイント応答には、特定のリソースの `Permit` または `Deny` の決定が含まれています。
   * `Permit` の決定は、リソースが再生可能であることを意味します。 応答にはメディアトークンが含まれます。
   * `Deny` の決定は、リソースが再生可能でないことを意味します。 応答には、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従ったエラーペイロードが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した認証の決定の取得 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > 決定の認証エンドポイントは、基本条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

+++

+++ D. ログアウトフェーズ

1. **Adobe Pass ログアウトの開始：** ストリーミングアプリケーションは、Adobe Pass ログアウトエンドポイントを呼び出して、ログアウトフローを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd のログアウトの開始 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`redirectUrl` など、すべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **次のアクションを示す：** Adobe Pass ログアウトエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドとして必要なデータが含まれます。
   * ログアウトフローを完了するにはユーザーがパートナー（システム） レベルとやり取りする必要があるので、`url` 属性がありません。
   * `actionName` 属性は「partner_logout」に設定されます。
   * `actionType` 属性は「partner_interactive」に設定されます。

   >[!IMPORTANT]
   >
   > ログアウト応答で提供される情報について詳しくは、[ 特定の mvpd のログアウトの開始 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > Adobe Pass ログアウトエンドポイントは、基本的な条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

   >[!IMPORTANT]
   > 
   > ストリーミングアプリケーションでは、ユーザーがパートナーレベルからさらにログアウトし続けるように指定する必要があります。

+++
