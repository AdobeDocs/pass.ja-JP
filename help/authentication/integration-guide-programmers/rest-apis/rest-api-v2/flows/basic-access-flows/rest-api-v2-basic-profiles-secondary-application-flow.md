---
title: 基本プロファイル -セカンダリアプリケーション – フロー
description: REST API V2 – 基本プロファイル -セカンダリアプリケーション – フロー
exl-id: 1fcefcfa-7534-4b85-b3b5-df513685d66b
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# セカンダリアプリケーション内で実行される基本プロファイルフロー {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

Adobe Pass認証使用権内の **プロファイルフロー** により、セカンダリアプリケーションはアクティブなユーザーログインに関する情報にアクセスできます。

基本プロファイルフローを使用すると、次のシナリオについてクエリを実行できます。

* [特定のコードのプロファイルの取得](#retrieve-profile-for-specific-code)

## 特定のコードのプロファイルの取得 {#retrieve-profile-for-specific-code}

### 前提条件 {#prerequisites-retrieve-profile-for-specific-code}

特定の認証コードのプロファイルを取得する前に、次の前提条件が満たされていることを確認します。

* MVPDでインタラクティブ認証を実行するために使用される `code` ールを持つセカンダリアプリケーションは、特定の認証コードのプロファイルを取得したいと考えています。

### ワークフロー {#workflow-retrieve-profile-for-specific-code}

次の図に示すように、セカンダリ・アプリケーション内で実行される特定の認証コードに対して基本プロファイル取得フローを実装するには、次の手順に従います。

![&#x200B; 特定のコードのプロファイルを取得 &#x200B;](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*特定のコードのプロファイルを取得*

1. **特定のコードのプロファイルを取得：** セカンダリアプリケーションは、プロファイルエンドポイントにリクエストを送信して、その特定の認証コードのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; 特定のコードのプロファイルの取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   >
   > * _、_ など、すべての `serviceProvider` 必須 `code` パラメーター
   > * _のようなすべての_ 必須 `Authorization` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **通常のプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた、見つかったプロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[&#x200B; 特定のコードのプロファイルの取得 &#x200B;](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > プロファイルエンドポイントは、基本条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **成功したため、認証フローが完了したことを示します。** プロファイルエンドポイント応答にプロファイルが含まれている場合、セカンダリアプリケーションは応答を処理し、オプションでユーザーインターフェイスに特定のメッセージを表示できます。

1. **認証フローで問題が発生したことを示す：** プロファイルエンドポイントの応答にプロファイルが含まれていない場合、セカンダリアプリケーションが応答を処理し、オプションで特定のメッセージをユーザーインターフェイスに表示できます。
