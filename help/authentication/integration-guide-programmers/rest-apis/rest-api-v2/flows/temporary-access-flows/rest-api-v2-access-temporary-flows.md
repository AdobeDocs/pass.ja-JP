---
title: 一時的なアクセスフロー
description: REST API V2 – 一時的なアクセスフロー
exl-id: 387fcdb0-3a42-4893-ba83-e809426f92be
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '3259'
ht-degree: 0%

---

# 一時的なアクセスフロー {#temporary-access-flows}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

TempPass を使用すると、プログラマーは、有効なMVPD アカウントで認証するようにユーザーに求めることなく、保護されたコンテンツへの一時的なアクセスを提供できます。

TempPass 機能の詳細については、[TempPass](../../../../features-premium/temporary-access/temp-pass-feature.md) ドキュメントを参照してください。

一時的なアクセスフローを使用すると、次のようなシナリオでクエリを実行できます。

* [基本的な TempPass を使用した認証決定の取得](#retrieve-authorization-decisions-using-basic-temppass)
* [プロモーション TempPass を使用した承認決定の取得](#retrieve-authorization-decisions-using-promotional-temppass)
* [プロモーション TempPass を使用して最大数のリソースを使用](#consume-maximum-number-of-resources-using-promotional-temppass)
* [基本またはプロモーションの TempPass の有効期限が切れた場合の承認決定の取得](#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires)
* [基本 TempPass のプロファイルの取得](#retrieve-profile-for-basic-temppass)
* [プロモーション TempPass のプロファイルを取得](#retrieve-profile-for-promotional-temppass)

## 基本的な TempPass を使用した認証決定の取得 {#retrieve-authorization-decisions-using-basic-temppass}

### 前提条件 {#prerequisites-retrieve-authorization-decisions-using-basic-temppass}

基本的な TempPass を使用して認証決定を取得する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションは、ユーザーに認証を求めずにコンテンツを再生するための一時的なアクセスを提供したいと考えています。
* ストリーミングアプリケーションは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * 指定された `serviceProvider` と `mvpd` の統合には、基本 TempPass の有効な設定が適用されている必要があります。
> * 基本的な TempPass 用に設定された有効期間（TTL）の有効期限が切れていません。

### ワークフロー {#workflow-retrieve-authorization-decisions-using-basic-temppass}

次の図に示すように、基本的な TempPass を使用して認証フローを実装するには、以下の手順に従います。

![&#x200B; 基本的な TempPass を使用した認証決定の取得 &#x200B;](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-basic-temppass-flow.png)

*基本的な TempPass を使用した認証決定の取得*

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   > 
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **基本的な TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された基本的な TempPass の有効な設定があるかどうかを確認します。

1. **メディアトークン `Permit` 決定を返す：** 決定の承認エンドポイント応答には、`Permit` 決定とメディアトークンが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd を使用した認証の決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   > 
   > 決定を承認エンドポイントは、リクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * 基本的な TempPass 用に設定された有効期間（TTL）の有効期限を切ってはいけません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **メディアトークンでストリームを開始：** ストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

## プロモーション TempPass を使用した承認決定の取得 {#retrieve-authorization-decisions-using-promotional-temppass}

### 前提条件 {#prerequisites-retrieve-authorization-decisions-using-promotional-temppass}

プロモーションの TempPass を使用して承認の決定を取得する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションは、ユーザーに認証を求めずに、最大リソース数を再生するための一時的なアクセスを提供したいと考えています。
* ストリーミングアプリケーションは、認証決定を取得する際に、ユーザーの ID に関する一意の情報を含める必要があります。
* ストリーミングアプリケーションは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * 指定された `serviceProvider` と `mvpd` の統合には、プロモーション TempPass の有効な設定設定が適用されている必要があります。
> * プロモーション TempPass 用に設定された有効期間（TTL）が期限切れではありません。
> * プロモーション TempPass 用に設定された最大リソース数が消費されていません。

### ワークフロー {#workflow-retrieve-authorization-decisions-using-promotional-temppass}

次の図に示すように、プロモーション用の TempPass を使用して認証フローを実装するには、次の手順に従います。

![&#x200B; プロモーション TempPass を使用した承認決定の取得 &#x200B;](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-promotional-temppass-flow.png)

*プロモーション TempPass を使用した承認決定の取得*

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   >
   > プロモーション TempPass を使用する場合、決定の承認エンドポイント `AP-TempPass-Identity` はヘッダーが存在する必要があります。 ヘッダーには、コンテンツにアクセスするユーザーの ID に関する一意の情報が含まれます。
   > 
   > <br/>
   > 
   > ヘッダーについて詳 `AP-TempPass-Identity` くは、[AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) ドキュメントを参照してください。

1. **プロモーションの TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用されたプロモーションの TempPass の有効な設定があるかどうかを確認します。

1. **メディアトークン `Permit` 決定を返す：** 決定の承認エンドポイント応答には、`Permit` 決定とメディアトークンが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd を使用した認証の決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   > 
   > 決定を承認エンドポイントは、リクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * プロモーション TempPass 用に設定された Time-To-Live （TTL）の有効期限を切ることはできません。
   > * プロモーション TempPass に設定されているリソースの最大数は使用できません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **メディアトークンでストリームを開始：** ストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

## プロモーション TempPass を使用して最大数のリソースを使用 {#consume-maximum-number-of-resources-using-promotional-temppass}

### 前提条件 {#prerequisites-consume-maximum-number-of-resources-using-promotional-temppass}

プロモーションの TempPass を使用して最大数のリソースを使用する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションは、ユーザーに認証を求めずに、最大リソース数を再生するための一時的なアクセスを提供したいと考えています。
* ストリーミングアプリケーションは、認証決定を取得する際に、ユーザーの ID に関する一意の情報を含める必要があります。
* ストリーミングアプリケーションは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * 指定された `serviceProvider` と `mvpd` の統合には、プロモーション TempPass の有効な設定設定が適用されている必要があります。
> * プロモーション TempPass 用に設定された有効期間（TTL）が期限切れではありません。
> * プロモーション TempPass に設定できるリソースの最大数は 1 です。

### ワークフロー {#workflow-consume-maximum-number-of-resources-using-promotional-temppass}

次の図に示すように、プロモーション用の TempPass を使用して最大数のリソースを使用する際に承認フローを実装するには、以下の手順に従います。

![&#x200B; プロモーションの TempPass を使用して最大数のリソースを使用 &#x200B;](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-consume-maximum-number-of-resources-using-promotional-temppass-flow.png)

*プロモーションの TempPass を使用して最大数のリソースを使用*

1. **プロモーション TempPass のプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、プロモーション TempPass のプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; 特定の mvpd のプロファイルを取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > プロファイルエンドポイントのクエリはオプションで、プロモーションの TempPass で引き続き再生できるリソースの数を決定するために使用できます。

1. **プロモーションの TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用されたプロモーションの TempPass の有効な設定があるかどうかを確認します。

1. **一時プロファイルに関する情報を返す：** プロファイルエンドポイント応答には、属性 `type` 「一時」に設定されているなど、一時プロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd のプロファイルを取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > プロファイルエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
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
   > プロファイルエンドポイントはリクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * プロモーション TempPass 用に設定された Time-To-Live （TTL）の有効期限を切ることはできません。
   > * プロモーション TempPass に設定されているリソースの最大数は使用できません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定フローで続行：** プロファイルエンドポイント応答にプロファイルが含まれている場合、ストリーミングアプリケーションは一時的なプロファイル情報を使用して、後続の決定フローを続行します。

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   > 
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > プロモーション TempPass を使用する場合、決定の承認エンドポイント `AP-TempPass-Identity` はヘッダーが存在する必要があります。 ヘッダーには、コンテンツにアクセスするユーザーの ID に関する一意の情報が含まれます。
   > 
   > <br/>
   > 
   > ヘッダーについて詳 `AP-TempPass-Identity` くは、[AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) ドキュメントを参照してください。

1. **プロモーションの TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用されたプロモーションの TempPass の有効な設定があるかどうかを確認します。

1. **メディアトークン `Permit` 決定を返す：** 決定の承認エンドポイント応答には、`Permit` 決定とメディアトークンが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd を使用した認証の決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   > 
   > <br/>
   > 
   > 決定を承認エンドポイントは、リクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * プロモーション TempPass 用に設定された Time-To-Live （TTL）の有効期限を切ることはできません。
   > * プロモーション TempPass に設定されているリソースの最大数は使用できません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > プロモーション TempPass を使用する場合、決定の承認エンドポイント `AP-TempPass-Identity` はヘッダーが存在する必要があります。 ヘッダーには、コンテンツにアクセスするユーザーの ID に関する一意の情報が含まれます。
   >
   > <br/>
   > 
   > ヘッダーについて詳 `AP-TempPass-Identity` くは、[AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) ドキュメントを参照してください。

1. **プロモーションの TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用されたプロモーションの TempPass の有効な設定があるかどうかを確認します。

1. **詳細を含んだ決定 `Deny` 返す：** 決定の承認承認エンドポイント応答には、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った `Deny` 決定とエラーペイロードが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd を使用した認証の決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   > 
   > 決定を承認エンドポイントは、リクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * プロモーション TempPass 用に設定された Time-To-Live （TTL）の有効期限を切ることはできません。
   > * プロモーション TempPass に設定されているリソースの最大数は使用できません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定の詳細 `Deny` 処理：** ストリーミングアプリケーションは、応答からのエラー情報を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

   >[!TIP]
   >
   > ストリーミングアプリケーションは、リソースの最大数を超えたことをユーザーに通知し、通常のMVPDを使用して基本認証フローを開始して視聴を続行するようにユーザーに助言できます。

## 基本またはプロモーションの TempPass の有効期限が切れた場合の承認決定の取得 {#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

### 前提条件 {#prerequisites-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

基本またはプロモーションの TempPass の有効期限が切れた場合に認証決定を取得する前に、次の前提条件が満たされていることを確認します。

* [&#x200B; 基本的な TempPass を使用して認証決定を取得するための前提条件 &#x200B;](#prerequisites-retrieve-authorization-decisions-using-basic-temppass)
* [&#x200B; プロモーション TempPass を使用して承認の決定を取得するための前提条件 &#x200B;](#prerequisites-retrieve-authorization-decisions-using-promotional-temppass)。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * 指定された `serviceProvider` と `mvpd` の統合には、基本またはプロモーションの TempPass の有効な設定設定が適用されている必要があります。
> * 基本またはプロモーション用に設定された有効期間（TTL）一時アクセス期間の制限を超えています。

### ワークフロー {#workflow-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

次の図に示すように、基本またはプロモーションの TempPass の有効期限が切れた場合に認証フローを実装するには、指定の手順に従います。

![&#x200B; 基本またはプロモーションの TempPass の有効期限が切れた場合の承認決定の取得 &#x200B;](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires-flow.png)

*基本またはプロモーションの TempPass の有効期限が切れた場合の承認決定の取得*

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   > 
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > プロモーション TempPass を使用する場合、決定の承認エンドポイント `AP-TempPass-Identity` はヘッダーが存在する必要があります。 ヘッダーには、コンテンツにアクセスするユーザーの ID に関する一意の情報が含まれます。
   > 
   > <br/>
   > 
   > ヘッダーについて詳 `AP-TempPass-Identity` くは、[AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md) ドキュメントを参照してください。

1. **基本またはプロモーションの TempPass の検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された基本またはプロモーションの TempPass の有効な設定があるかどうかを確認します。

1. **詳細を含んだ決定 `Deny` 返す：** 決定の承認承認エンドポイント応答には、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った `Deny` 決定とエラーペイロードが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd を使用した認証の決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   > 
   > 決定を承認エンドポイントは、リクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * 基本またはプロモーションの TempPass 用に設定された有効期間（TTL）の有効期限を切ってはいけません。
   > * プロモーション TempPass に設定されているリソースの最大数は使用できません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定の詳細 `Deny` 処理：** ストリーミングアプリケーションは、応答からのエラー情報を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

   >[!TIP]
   >
   > ストリーミングアプリケーションは、一時的なアクセス権の有効期限が切れたことをユーザーに通知し、通常のMVPDを使用して基本認証フローを開始して視聴を続行するようにユーザーに助言できます。

## 基本 TempPass のプロファイルの取得 {#retrieve-profile-for-basic-temppass}

>[!IMPORTANT]
>
> 基本的な TempPass のプロファイルエンドポイントクエリはオプションです。

### 前提条件 {#prerequisites-retrieve-profile-for-basic-temppass}

基本的な TempPass のプロファイルを取得する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションは、一時アクセスが期限切れでないことを確認するために、一時プロファイルを取得します。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * 指定された `serviceProvider` と `mvpd` の統合には、基本 TempPass の有効な設定が適用されている必要があります。
> * 基本的な TempPass 用に設定された有効期間（TTL）の有効期限を切ってはいけません。

### ワークフロー {#workflow-retrieve-profile-information-for-basic-temppass}

次の図に示すように、基本的な TempPass のプロファイル取得フローを実装するには、次の手順に従います。

![&#x200B; 基本 TempPass のプロファイルを取得 &#x200B;](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-basic-temppass-flow.png)

*基本 TempPass のプロファイルを取得*

1. **基本 TempPass のプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、基本 TempPass のプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; 特定の mvpd のプロファイルを取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   > 
   > * `serviceProvider`、`mvpd` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **基本的な TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された基本的な TempPass の有効な設定があるかどうかを確認します。

1. **一時プロファイルに関する情報を返す：** プロファイルエンドポイント応答には、属性 `type` 「一時」に設定されているなど、一時プロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd のプロファイルを取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > プロファイルエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
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
   > プロファイルエンドポイントはリクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * 基本的な TempPass 用に設定された有効期間（TTL）の有効期限を切ってはいけません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定フローで続行：** プロファイルエンドポイント応答にプロファイルが含まれている場合、ストリーミングアプリケーションは一時的なプロファイル情報を使用して、後続の決定フローを続行します。

## プロモーション TempPass のプロファイルを取得 {#retrieve-profile-for-promotional-temppass}

>[!IMPORTANT]
>
> プロモーション TempPass の場合、プロファイルエンドポイントクエリはオプションです。

### 前提条件 {#prerequisites-retrieve-profile-for-promotional-temppass}

プロモーション TempPass のプロファイルを取得する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションは、一時プロファイルを取得して、一時アクセスが期限切れになっていないことを確認したり、まだ再生できるリソースの数を判断したりします。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * 指定された `serviceProvider` と `mvpd` の統合には、プロモーション TempPass の有効な設定設定が適用されている必要があります。
> * プロモーション TempPass 用に設定された有効期間（TTL）が期限切れではありません。
> * プロモーション TempPass 用に設定された最大リソース数が消費されていません。

### ワークフロー {#workflow-retrieve-profile-information-for-promotional-temppass}

次の図に示すように、プロモーション用 TempPass のプロファイル取得フローを実装するには、次の手順に従います。

![&#x200B; プロモーション TempPass のプロファイルを取得 &#x200B;](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-promotional-temppass-flow.png)

*プロモーション TempPass のプロファイルを取得*

1. **プロモーション TempPass のプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、プロモーション TempPass のプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; 特定の mvpd のプロファイルを取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   > 
   > * `serviceProvider`、`mvpd` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **プロモーションの TempPass を検証：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用されたプロモーションの TempPass の有効な設定があるかどうかを確認します。

1. **一時プロファイルに関する情報を返す：** プロファイルエンドポイント応答には、属性 `type` 「一時」に設定されているなど、一時プロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd のプロファイルを取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > プロファイルエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
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
   > プロファイルエンドポイントはリクエストデータを使用して、一時的なアクセス条件を満たしているかどうかを確認します。
   >
   > * プロモーション TempPass 用に設定された Time-To-Live （TTL）の有効期限を切ることはできません。
   > * プロモーション TempPass に設定されているリソースの最大数は使用できません。
   >
   > <br/>
   > 
   > 一時的なアクセスの検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定フローで続行：** プロファイルエンドポイント応答にプロファイルが含まれている場合、ストリーミングアプリケーションは一時的なプロファイル情報を使用して、後続の決定フローを続行します。
