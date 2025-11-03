---
title: シングルサインオン – サービストークン – フロー
description: REST API V2 - シングルサインオン – サービストークン – フロー
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '1857'
ht-degree: 0%

---

# サービストークンフローを使用したシングルサインオン{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

サービストークンを使用すると、複数のアプリケーションで一意のユーザー ID を使用して、Adobe Pass サービスの使用時に複数のデバイスやプラットフォームにわたってシングルサインオン（SSO）を実現できます。

アプリケーションは、Adobe Pass システムの外部で外部 ID サービスを使用して、一意のユーザー ID ペイロードを取得する役割を果たします。例えば、次のようなものがあります。

* ユーザーが同じ資格情報を使用して各デバイスにログインし、同じユーザー ID またはユーザーアカウント名に関連付けられている、ダイレクト ツーコンシューマー（DTC） サービス。
* ユーザーが同じ資格情報を使用して各デバイスにログインし、同じメールアドレスに関連付けられる、Googleや Facebook などのサードパーティ認証サービス。

アプリケーションは、この一意のユーザー ID ペイロードを指定するすべてのリクエストの `AD-Service-Token` ヘッダーの一部として含める責任があります。

ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

## サービストークンを使用したシングルサインオンによる認証の実行 {#performing-authentication-flow-using-service-token-single-sign-on-method}

### 前提条件 {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

サービストークンを使用してシングルサインオンを通じた認証フローを実行する前に、次の前提条件が満たされていることを確認します。

* 外部 ID サービスは、複数のデバイスとプラットフォームにまたがって、すべてのアプリケーション `JWS` 対してペイロードとして一貫した情報を返す必要があります。
* 最初のストリーミングアプリケーションは、一意のユーザー識別子を取得し、それを指定するすべてのリクエストの `JWS`AD-Service-Token[ ヘッダーの一部として ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ペイロードを含める必要があります。
* 最初のストリーミングアプリケーションでは、MVPDを選択する必要があります。
* 最初のストリーミングアプリケーションでは、選択したMVPDでログインするための認証セッションを開始する必要があります。
* 最初のストリーミングアプリケーションは、ユーザーエージェントで選択されたMVPDを使用して認証される必要があります。
* 2 番目のストリーミングアプリケーションは、一意のユーザー識別子を取得し、それを指定するすべてのリクエストの `JWS`AD-Service-Token[ ヘッダーの一部として ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ペイロードを含める必要があります。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * 最初のストリーミングアプリケーションは、MVPDを選択するためのユーザーインタラクションをサポートしています。
> * 最初のストリーミングアプリケーションは、ユーザーエージェント内の選択されたMVPDを使用して認証を行うユーザーインタラクションをサポートしています。

### ワークフロー {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

次の図に示すように、サービストークンを使用してシングルサインオンを通じて認証フローを実装するには、指定された手順を実行します。

![ サービストークンを使用したシングルサインオンによる認証の実行 ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*サービストークンを使用したシングルサインオンによる認証の実行*

1. **ID サービスによる認証：** 最初のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスを呼び出し、一意のユーザー ID に関連付けられた `JWS` ペイロードを取得します。

1. **一意のユーザー ID を JWS として返す：** 最初のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名されました。

1. **認証セッションの作成：** 最初のストリーミングアプリケーションは、セッションエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 認証セッションの作成 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider`、`mvpd` などのすべての `domainName` 必須 `redirectUrl` パラメーター
   > * _、_ などのすべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のユーザー識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

1. **次のアクションを示す：** セッションエンドポイント応答には、次のアクションに関する最初のストリーミングアプリケーションをガイドするために必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > セッション応答で提供される情報について詳しくは、[ 認証セッションの作成 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   >
   > <br/>
   > 
   > セッション エンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **ユーザーエージェントで URL を開く：** セッションエンドポイントの応答には、次のデータが含まれます。
   * MVPDのログインページ内でインタラクティブ認証を開始するために使用できる `url`。
   * `actionName` 属性は「authenticate」に設定されています。
   * `actionType` 属性は「interactive」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、最初のストリーミングアプリケーションがユーザーエージェントを開いて指定された `url` を読み込み、Authenticate エンドポイントにリクエストを送信します。 このフローには、複数のリダイレクトが含まれる場合があり、最終的にユーザーがMVPDのログインページに移動して、有効な資格情報を提供します。

1. **MVPD認証の完了：** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** 最初のストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信してプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   > 
   > * _、_ などのすべての `serviceProvider` 必須 `code` パラメーター
   > * _、_ などのすべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

   >[!TIP]
   >
   > ストリーミングアプリケーションは、ユーザーエージェントが指定された `redirectUrl` に到達して、通常のプロファイルが正常に生成および保存されたかどうかを確認するまで待つ必要があります。

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

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

1. **決定フローで続行：** 最初のストリーミングアプリケーションは、後続の決定フローで続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のユーザー識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

1. **ID サービスによる認証：** 2 つ目のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスを呼び出し、一意のユーザー ID に関連付けられた `JWS` ペイロードを取得します。

1. **一意のユーザー ID を JWS として返す：** 2 番目のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名されました。

1. **プロファイルの取得：** 2 番目のストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信して、すべてのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ プロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API ドキュメントを参照してください。
   >
   > * _のようなすべての_ 必須 `serviceProvider` パラメーター
   > * _、_ などのすべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のユーザー識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

1. **シングルサインオンプロファイルの検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なシングルサインオンプロファイルを識別します。

1. **シングルサインオンプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた、見つかったプロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[ プロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API ドキュメントを参照してください。
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

1. **決定フローで続行：** 2 番目のストリーミングアプリケーションは、後続の決定フローで続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のユーザー識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

## サービストークンを使用したシングルサインオンを通じた認証決定の取得 {#performing-authorization-flow-using-service-token-single-sign-on-method}

### 前提条件 {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

サービストークンを使用してシングルサインオンを通じて認証フローを実行する前に、次の前提条件が満たされていることを確認します。

* 外部 ID サービスは、複数のデバイスとプラットフォームにまたがって、すべてのアプリケーション `JWS` 対してペイロードとして一貫した情報を返す必要があります。
* 最初のストリーミングアプリケーションは、一意のユーザー識別子を取得し、それを指定するすべてのリクエストの `JWS`AD-Service-Token[ ヘッダーの一部として ](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ペイロードを含める必要があります。
* 2 つ目のストリーミングアプリケーションでは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * 最初のストリーミングアプリケーションで認証が実行され、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) リクエストヘッダーに有効な値が含まれています。

### ワークフロー {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

次の図に示すように、サービストークンを使用してシングルサインオンを通じて認証フローを実装するには、指定された手順を実行します。

![ サービストークンを使用したシングルサインオンを通じた認証決定の取得 ](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*サービストークンを使用したシングルサインオンを通じた認証決定の取得*

1. **ID サービスによる認証：** 2 つ目のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスを呼び出し、一意のユーザー ID に関連付けられた `JWS` ペイロードを取得します。

1. **一意のユーザー ID を JWS として返す：** 2 番目のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名されました。

1. **認証決定の取得：** 2 番目のストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd を使用した認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `mvpd` 必須 `resources` パラメーター
   > * _や_ など、すべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のユーザー識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

1. **シングルサインオンプロファイルの検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なシングルサインオンプロファイルを識別します。

1. **リクエストされたリソースのMVPD決定を取得：** Adobe Pass サーバーは、MVPD Authorization エンドポイントを呼び出して、ストリーミングアプリケーションから受信した特定のリソースに関する `Permit` 定または `Deny` 定の決定を取得します。

1. **メディアトークン `Permit` 決定を返す：** 決定の承認エンドポイント応答には、`Permit` 決定とメディアトークンが含まれています。

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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **メディアトークンでストリームを開始：** 2 番目のストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

1. **詳細を含んだ決定 `Deny` 返す：** 決定の承認承認エンドポイント応答には、`Deny` 拡張エラーコード [ ドキュメントに従った ](../../../../features-standard/error-reporting/enhanced-error-codes.md) 決定とエラーペイロードが含まれています。

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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定の詳細 `Deny` 処理：** 2 番目のストリーミングアプリケーションは、応答からのエラー情報を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

>[!NOTE]
>
> 事前認証フローの手順は、認証フローの手順と同じです。ただし、使用されるエンドポイントが、「[ 特定の mvpd を使用した事前認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) ドキュメントで説明されているエンドポイントである点が異なります。
