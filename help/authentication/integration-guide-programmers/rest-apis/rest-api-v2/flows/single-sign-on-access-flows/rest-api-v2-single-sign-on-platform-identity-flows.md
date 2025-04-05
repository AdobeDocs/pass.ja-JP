---
title: シングルサインオン - Platform Id - フロー
description: REST API V2 - シングルサインオン - Platform Id - フロー
exl-id: 5200e851-84e8-4cb4-b068-63b91a2a8945
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# プラットフォーム ID フローを使用したシングルサインオン {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

Platform ID 方式を使用すると、複数のアプリケーションで一意の Platform ID を使用して、Adobe Pass サービスの使用時にデバイスレベルまたはプラットフォームレベルでシングルサインオン（SSO）を実現できます。

アプリケーションは、デバイス固有の ID サービスやAdobe Pass システム外のライブラリを使用して、一意の Platform ID ペイロードを取得する役割を果たします。

アプリケーションは、この一意のプラットフォーム識別子ペイロードを、それを指定するすべてのリクエストの `Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token` ヘッダーの一部として含める責任があります。

`Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` ヘッダーの詳細については、[Adobe Systems-件名-トークン](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-トークン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ドキュメントを参照してください。

>[!MORELIKETHIS]
> 
> * [Amazon SSO クックブック](/help/authentication/integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
> * [Roku SSO クックブック](/help/authentication/integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)

## プラットフォーム ID を使用したシングルサインオンによる認証の実行 {#perform-authentication-through-single-sign-on-using-platform-identity}

### 前提条件 {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

プラットフォーム ID を使用して認証フローシングルサインオンを実行する前に、次の前提条件が満たされていることを確認してください。

* プラットフォームは、同じデバイスまたはプラットフォーム上のすべてのアプリケーション間でペイロード `JWS` または `JWE` として一貫した情報を返す ID サービスまたはライブラリを提供する必要があります。
* 最初のストリーミングアプリケーションは、一意のプラットフォーム識別子を取得し、それを指定するすべての要求の [Adobe Systems-件名-トークン](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-トークン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)ヘッダーの一部として`JWS`または`JWE`ペイロードを含める必要があります。
* 最初のストリーミング アプリケーションでは、MVPD を選択する必要があります。
* 最初のストリーミング アプリケーションは、選択した MVPD でサインインするための認証セッションを開始する必要があります。
* 最初のストリーミング アプリケーションは、ユーザー エージェントで選択した MVPD で認証する必要があります。
* 2 番目のストリーミングアプリケーションは、一意のプラットフォーム ID を取得し、それを指定するすべてのリクエストの [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ヘッダーの一部として `JWS` または `JWE` ペイロードを含める必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * 最初のストリーミングアプリケーションは、MVPDを選択するためのユーザーインタラクションをサポートしています。
> * 最初のストリーミング アプリケーションはユーザーユーザーエージェントで選択した MVPD で認証するための対話をサポートします。

### ワークフロー {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

次の図に示すように、プラットフォーム ID を使用して認証フローシングルサインオン実装するするには、上記の手順を実行します。

![Platform ID を使用したシングルサインオンによる認証の実行 ](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Platform ID を使用したシングルサインオンによる認証の実行*

1. **プラットフォーム ID の取得：** 最初のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスまたはライブラリを呼び出して、一意のプラットフォーム ID に関連付けられた `JWS` または `JWE` ペイロードを取得します。

1. **JWS または JWE として一意のプラットフォーム ID を返します：** 最初のストリーミングアプリケーションでは、応答データを検証して、基本的なセキュリティ条件が満たされていることを確認します。
   * ペイロードが期限切れではありません。
   * ペイロードは署名または暗号化されます。

1. **認証セッションの作成：** 最初のストリーミングアプリケーションは、セッションエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 認証セッションの作成 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   > 
   > * `serviceProvider`、`mvpd`、`domainName`、`redirectUrl` などのすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子の有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` ヘッダーの詳細については、[Adobe Systems-件名-トークン](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-トークン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ドキュメントを参照してください。

1. **次のアクションを示します** Sessions エンドポイントの応答には、次のアクションに関する最初のストリーミング アプリケーションガイドために必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > セッション応答で提供される情報の詳細については [作成認証セッション](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > Sessions エンドポイントは、リクエストデータを検証して、基本的な条件が満たされていることを確認します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **ユーザーエージェントで URL を開く：** セッションエンドポイントの応答には、次のデータが含まれます。
   * MVPDのログインページ内でインタラクティブ認証を開始するために使用できる `url`。
   * `actionName`属性は「認証」に設定されます。
   * `actionType`属性は「インタラクティブ」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、最初のストリーミングアプリケーションがユーザーエージェントを開いて指定された `url` を読み込み、Authenticate エンドポイントにリクエストを送信します。 このフローには、複数のリダイレクトが含まれる場合があり、最終的にユーザーがMVPDのログインページに移動して、有効な資格情報を提供します。

1. **MVPD認証の完了：** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** 最初のストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信してプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   > 
   > * `serviceProvider` などの&#x200B;_必須_&#x200B;パラメーターすべてを選択、`code`
   > * `Authorization` などの&#x200B;_必須_&#x200B;ヘッダーすべてを選択、`AP-Device-Identifier`
   > * _オプションの_&#x200B;パラメーターとヘッダーすべてを選択

   >[!TIP]
   >
   > 提案:ストリーミングアプリケーションは、ユーザーエージェントが指定された `redirectUrl` に到達するのを待って、通常のプロファイルが正常に生成および保存されたかどうかを確認できます。

1. **通常のプロファイル検索文字列:** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **通常のプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた、見つかったプロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > プロファイルエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **意思決定フローの続行:** 最初のストリーミングアプリケーション後続の意思決定フローを続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token` ヘッダーについて詳しくは、[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ドキュメントを参照してください。

1. **プラットフォーム ID の取得：** 2 つ目のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスまたはライブラリを呼び出し、一意のプラットフォーム ID に関連付けられた `JWS` または `JWE` ペイロードを取得します。

1. **JWS または JWE として一意のプラットフォーム ID を返します：** 2 番目のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードは署名または暗号化されます。

1. **プロファイルの取得:** 2 番目のストリーミング アプリケーションは、プロファイル エンドポイントにリクエストを送信して、すべてのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 以下について詳しくは、 [プロファイルの取得](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API ドキュメントを参照してください。
   > 
   > * _必須_&#x200B;パラメーターすべてを選択、`serviceProvider`
   > * `Authorization` などの&#x200B;_必須_&#x200B;ヘッダーすべてを選択、`AP-Device-Identifier`
   > * _オプションの_&#x200B;パラメーターとヘッダーすべてを選択
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子の有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` ヘッダーの詳細については、[Adobe Systems-件名-トークン](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-トークン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ドキュメントを参照してください。

1. **検索文字列 シングルサインオン プロファイル:** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なシングルサインオンプロファイルを識別します。

1. **シングルサインオン プロファイル に関する情報を返します** Profiles エンドポイントの応答には、受信したパラメーターとヘッダーに関連付けられている見つかったプロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報の詳細については、 [プロファイルの取得](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API ドキュメントを参照してください。
   >
   > <br/>
   > 
   > プロファイルエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **意思決定フローの続行:** 2 番目のストリーミングアプリケーションは、後続の意思決定フローを続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子の有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` ヘッダーの詳細については、[Adobe Systems-件名-トークン](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-トークン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ドキュメントを参照してください。

## Platform ID を使用したシングルサインオンを通じた認証決定の取得{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### 前提条件 {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

プラットフォーム ID を使用してシングルサインオン経由で認証フローを実行する前に、次の前提条件が満たされていることを確認してください。

* プラットフォームは、同じデバイスまたはプラットフォーム上のすべてのアプリケーションで、一貫した情報を `JWS` または `JWE` ペイロードとして返す ID サービスまたはライブラリを提供する必要があります。
* 2 番目のストリーミングアプリケーションは、一意のプラットフォーム ID を取得し、それを指定するすべてのリクエストの [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ヘッダーの一部として `JWS` または `JWE` ペイロードを含める必要があります。
* 2 つ目のストリーミングアプリケーションでは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * 最初のストリーミングアプリケーションで認証が実行され、[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) リクエストヘッダーに有効な値が含まれています。

### ワークフロー {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

次の図に示すように、プラットフォーム ID を使用したシングルサインオンを通じて認証フローを実装するには、指定された手順を実行します。

![Platform ID を使用したシングルサインオンを通じた認証決定の取得 ](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*プラットフォーム ID を使用したシングルサインオンによる認証の決定の取得*

1. **プラットフォーム ID の取得：** 2 つ目のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスまたはライブラリを呼び出し、一意のプラットフォーム ID に関連付けられた `JWS` または `JWE` ペイロードを取得します。

1. **JWS または JWE として一意のプラットフォーム ID を返します：** 2 番目のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名済みまたは暗号化されている。

1. **認証決定の取得：** 2 番目のストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd を使用した認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token`/`X-Roku-Reserved-Roku-Connect-Token` ヘッダーの詳細については、[Adobe Systems-件名-トークン](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)/[X-Roku-Reserved-Roku-Connect-トークン](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md) ドキュメントを参照してください。

1. **シングルサインオンプロファイルの検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なシングルサインオンプロファイルを識別します。

1. **リクエストされたリソースのMVPD決定を取得：** Adobe Pass サーバーは、MVPD Authorization エンドポイントを呼び出して、ストリーミングアプリケーションから受信した特定のリソースに関する `Permit` 定または `Deny` 定の決定を取得します。

1. **メディアトークン `Permit` 決定を返す：** 決定の承認エンドポイント応答には、`Permit` 決定とメディアトークンが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > Decision Authorizeエンドポイントは、リクエストデータを検証して、基本的な条件が満たされていることを確認します。
   >
   > * _必須_&#x200B;パラメーターおよびヘッダーは有効である必要があります。
   > * 指定された `serviceProvider` と `mvpd` 間の統合がアクティブになっている必要があります。
   >
   > <br/>
   > 
   > 検証が失敗すると、エラー応答が生成され、 [強化機能エラーコード](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに準拠した追加情報が提供されます。

1. **メディアトークンを使用した開始ストリーム:** 2 番目のストリーミング アプリケーションは、メディア トークンを使用して内容を再生します。

1. **決定`Deny`詳細と共に返す:** Decision Authorize エンドポイントの応答には、[強化機能 エラー コード](../../../../features-standard/error-reporting/enhanced-error-codes.md)ドキュメントに準拠した`Deny`決定とエラー ペイロードが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 検証が失敗すると、エラー応答が生成され、 [強化機能エラーコード](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに準拠した追加情報が提供されます。

1. **決定の詳細 `Deny` 処理：** 2 番目のストリーミングアプリケーションは、応答からのエラー情報を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

>[!NOTE]
>
> 事前認証フローの手順は、認証フローの手順と同じです。ただし、使用されるエンドポイントが、「[ 特定の mvpd を使用した事前認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) ドキュメントで説明されているエンドポイントである点が異なります。
