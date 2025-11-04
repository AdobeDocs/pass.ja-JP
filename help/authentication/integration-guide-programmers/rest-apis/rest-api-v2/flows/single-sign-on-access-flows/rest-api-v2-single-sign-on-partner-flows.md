---
title: シングルサインオン – パートナー – フロー
description: REST API V2 - シングルサインオン – パートナー – フロー
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# パートナーフローを使用したシングルサインオン {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

パートナーメソッドを使用すると、複数のアプリケーションでパートナーフレームワークステータスペイロードを使用して、Adobe Pass サービスの使用時にデバイスレベルでシングルサインオン（SSO）を実現できます。

Adobe Pass システム外のパートナー固有のフレームワークやライブラリを使用して、パートナーフレームワークステータスペイロードを取得する役割を持ちます。

アプリケーションは、このパートナーフレームワークステータスペイロードを指定するすべてのリクエストの `AP-Partner-Framework-Status` ヘッダーの一部として含める責任があります。

ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

Adobe Pass認証 REST API V2 は、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザー向けに、パートナーシングルサインオン（SSO）をサポートしています。

Apple プラットフォームのシングルサインオン （SSO）について詳しくは、[Apple SSO クックブック （REST API V2） &#x200B;](/help/premium-workflow/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md) ドキュメントを参照してください。

## パートナー認証要求の取得 {#retrieve-partner-authentication-request}

### 前提条件 {#prerequisites-retrieve-partner-authentication-request}

パートナー認証リクエストを取得する前に、次の前提条件が満たされていることを確認します。

* パートナーフレームワークは、MVPDを選択する必要があります。
* ストリーミングアプリケーションは、パートナーフレームワークからパートナーフレームワークのステータス情報を取得し、Adobe Pass サーバーに渡す必要があります。
* ストリーミングアプリケーションは、Adobe Pass サーバーからパートナー認証リクエストを取得し、パートナーフレームワークに渡す必要があります。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * パートナーフレームワークは、MVPDを選択するためのユーザーインタラクションをサポートします。
> * パートナーフレームワークは、選択したMVPDを使用して認証を行うためのユーザーインタラクションをサポートしています。
> * パートナーフレームワークは、ユーザー権限およびプロバイダー情報を提供します。

### ワークフロー {#workflow-retrieve-partner-authentication-request}

以下の図に示すように、パートナー認証リクエストを取得するには、以下の手順を実行します。

![&#x200B; パートナー認証要求の取得 &#x200B;](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*パートナー認証要求の取得*

1. **パートナーフレームワークステータスの取得：** ストリーミングアプリケーションは、Adobe Pass システムの外部でパートナーフレームワークを呼び出して、ユーザー権限とプロバイダー情報を取得します。

1. **パートナーフレームワークのステータス情報を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **パートナー認証リクエストの取得：** ストリーミングアプリケーションは、セッションパートナーエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; パートナー認証リクエストの取得 &#x200B;](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) API ドキュメントを参照してください。
   >
   > * _や_ など、すべての `serviceProvider` 必須 `partner` パラメーター
   > * _、_、`Authorization`、`AP-Device-Identifier`、`Content-Type` など、すべての `X-Device-Info` 必須 `AP-Partner-Framework-Status` ヘッダー
   > * すべての _オプション_ ヘッダーとパラメーター
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、パートナーフレームワークのステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **次のアクションを示す：** セッションパートナーエンドポイント応答には、次のアクションに関するストリーミングアプリケーションをガイドするために必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > セッション応答で提供される情報について詳しくは、[&#x200B; パートナー認証リクエストの取得 &#x200B;](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   >
   > セッションパートナーエンドポイントは、リクエストデータを検証して、パートナーのシングルサインオン条件が満たされていることを確認します。
   >
   >  * Adobe Pass サーバーのパートナーのシングルサインオン設定が有効であり、有効である必要があります。
   >  * [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ヘッダーを介して受信したパートナーフレームワークステータスペイロードは、有効である必要があります。
   >
   > <br/>
   >
   > パートナーのシングルサインオン検証が失敗した場合、応答はデフォルトで基本認証フローに設定されます。

1. **パートナー認証応答を使用してプロファイル取得フローを続行：** セッションパートナーエンドポイント応答には、次のデータが含まれています。
   * `actionName` 属性は「partner_profile」に設定されます。
   * `actionType` 属性は「direct」に設定されます。
   * `authenticationRequest - type` 属性には、MVPD ログイン用にパートナーフレームワークが使用するセキュリティプロトコルが含まれます（現在、SAML のみに設定されています）。
   * `authenticationRequest - request` 属性には、パートナーフレームワークに渡される SAML リクエストが含まれます。
   * `authenticationRequest - attributesNames` 属性には、パートナーフレームワークに渡される SAML 属性が含まれます。

   Adobe Pass バックエンドが有効なプロファイルを識別せず、パートナーのシングルサインオン検証に合格した場合、ストリーミングアプリケーションはアクションとデータを含む応答を受け取り、MVPDとの認証フローを開始するためにパートナーフレームワークに渡します。

   パートナー認証応答を使用したプロファイル取得フローについて詳しくは、[&#x200B; パートナー認証応答を使用したプロファイルの作成と取得 &#x200B;](#create-and-retrieve-profile-using-partner-authentication-response) を参照してください。

1. **基本認証フローを続行：** セッションパートナーエンドポイント応答には、次のデータが含まれています。
   * `actionName` 属性は、「authenticate」または「resume」に設定されます。
   * `actionType` 属性は、「interactive」または「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別せず、パートナーのシングルサインオン検証に失敗した場合、Adobe Pass サーバーは基本認証フローにフォールバックします。

   基本認証フローについて詳しくは、次のドキュメントを参照してください。
   * [プライマリアプリケーション内での認証の実行](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **決定フローで続行：** セッションパートナーエンドポイント応答には、次のデータが含まれます。
   * `actionName` 属性は「authorize」に設定されます。
   * `actionType` 属性は「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを特定した場合、その後の決定フローに使用できるプロファイルが既に存在するので、ストリーミングアプリケーションは選択したMVPDで再認証する必要はありません。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、パートナーフレームワークのステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

## パートナー認証応答を使用したプロファイルの作成と取得 {#create-and-retrieve-profile-using-partner-authentication-response}

### 前提条件 {#prerequisites-create-and-retrieve-profile-using-partner-authentication-response}

パートナー認証応答を使用してプロファイルを取得する前に、次の前提条件が満たされていることを確認します。

* パートナーフレームワークは、選択したMVPDで認証を実行する必要があります。
* ストリーミングアプリケーションは、パートナーフレームワークから、パートナーフレームワークステータス情報と共に、パートナー認証応答を取得して、Adobe Pass サーバーに渡す必要があります。

>[!IMPORTANT]
>
> 引受け
>
> * パートナーフレームワークは、MVPDを選択するためのユーザーインタラクションをサポートします。
> * パートナーフレームワークは、選択したMVPDを使用して認証を行うためのユーザーインタラクションをサポートしています。
> * パートナーフレームワークは、ユーザー権限およびプロバイダー情報を提供します。

### ワークフロー {#workflow-create-and-retrieve-profile-using-partner-authentication-response}

次の図に示すように、パートナー認証応答を使用してプロファイル取得フローを実装するには、指定の手順を実行します。

![&#x200B; パートナー認証応答を使用したプロファイルの作成と取得 &#x200B;](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*パートナー認証応答を使用した、認証済みプロファイルの作成と取得*

1. **Partner Framework を使用したMVPD認証の完了：** 認証フローが成功すると、MVPDとの Partner Framework のやり取りによって、Partner Framework のステータス情報と共に返される Partner Authentication Response （SAML 応答）が生成されます。

1. **パートナー認証応答を返す：** ストリーミングアプリケーションは、基本条件が満たされていることを確認するために応答データを検証します。
   * ユーザー権限のアクセスステータスが付与されます。
   * ユーザープロバイダーマッピング識別子が存在し、有効です。
   * ユーザープロバイダープロファイルの有効期限（使用可能な場合）は有効です。

1. **パートナー認証応答を使用したプロファイルの作成と取得：** ストリーミングアプリケーションは、プロファイルパートナーエンドポイントを呼び出してプロファイルを作成および取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; パートナー認証応答を使用したプロファイルの作成と取得 &#x200B;](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `partner` 必須 `SAMLResponse` パラメーター
   > * _、_、`Authorization`、`AP-Device-Identifier`、`Content-Type` など、すべての `X-Device-Info` 必須 `AP-Partner-Framework-Status` ヘッダー
   > * すべての _オプション_ ヘッダーとパラメーター
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、パートナーフレームワークのステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。

1. **パートナープロファイルの作成と保存：** Adobe Pass サーバーは、すべての条件が満たされていることを確認した後、パートナープロファイルを作成して保存します。

1. **パートナープロファイルに関する情報を返す：** プロファイルエンドポイント応答には、「appleSSO」に設定されている属性 `type` ど、パートナープロファイルに関する情報が含まれています。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[&#x200B; パートナー認証応答を使用したプロファイルの作成と取得 &#x200B;](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)API ドキュメントを参照してください。
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
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   >
   > プロファイルパートナーエンドポイントは、リクエストデータを検証して、パートナーのシングルサインオン条件が満たされていることを確認します。
   >
   >  * Adobe Pass サーバーのパートナーのシングルサインオン設定が有効であり、有効である必要があります。
   >  * [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ヘッダーを介して受信したパートナーフレームワークステータスペイロードは、有効である必要があります。
   >
   > <br/>
   >
   > パートナーのシングルサインオン検証が失敗した場合、応答はデフォルトで基本プロファイル取得フローに設定されます。

1. **決定フローで続行：** ストリーミングアプリケーションは、後続の決定フローで続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、パートナーフレームワークのステータスに有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AP-Partner-Framework-Status` くは、[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) ドキュメントを参照してください。
