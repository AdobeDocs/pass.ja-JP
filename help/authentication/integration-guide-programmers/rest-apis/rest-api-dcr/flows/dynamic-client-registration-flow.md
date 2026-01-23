---
title: 動的なクライアント登録フロー
description: 動的なクライアント登録フロー
exl-id: d881cf0a-de09-4b1d-a094-d5490f944796
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# 動的なクライアント登録フロー {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> 動的なクライアント登録 API の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) に関するドキュメントに限られています。

## Adobe Passで保護された API へのアクセス {#access-adobe-pass-protected-apis}

### 前提条件 {#prerequisites-access-adobe-pass-protected-apis}

Adobe Passで保護された API にアクセスする前に、次の前提条件が満たされていることを確認してください。

* クライアント担当者は、[&#x200B; 登録アプリケーションの管理 &#x200B;](../dynamic-client-registration-overview.md#manage-registered-applications) の節で説明しているように、登録アプリケーションを作成する必要があります。
* クライアント担当者は、「ソフトウェア明細書の管理 [&#x200B; の節で説明しているように、ソフトウェア明細書をダウンロードして埋め込む必要が &#x200B;](../dynamic-client-registration-overview.md#manage-software-statements) ります。

>[!IMPORTANT]
>
> Adobe Pass Authentication SDK は、クライアントアプリケーションに代わって、クライアント資格情報とアクセストークンを取得および更新する役割を果たします。
> 
> その他のすべてのAdobe Pass保護 API の場合、クライアントアプリケーションは次のワークフローに従う必要があります。

### ワークフロー {#workflow-access-adobe-pass-protected-apis}

次の図に示すように、Adobe Passで保護された API にアクセスするには、次の手順に従います。

![Adobe Pass保護 API へのアクセス &#x200B;](../../../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Adobe Pass保護 API へのアクセス*

1. **クライアント資格情報の取得：** クライアントアプリケーションは、クライアント登録エンドポイントを呼び出して、クライアント資格情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; クライアント資格情報の取得 &#x200B;](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API ドキュメントを参照してください。
   >
   > * `software_statement` のようなすべての _必須_ パラメーター
   > * `Content-Type`、`X-Device-Info` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **クライアント資格情報を返す：** クライアント登録エンドポイント応答には、受信したパラメーターおよびヘッダーに関連付けられたクライアント資格情報に関する情報が含まれます。

   >[!IMPORTANT]
   >
   > クライアント資格情報応答で提供される情報について詳しくは、[&#x200B; クライアント資格情報の取得 &#x200B;](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > クライアントレジスタがリクエストデータを検証し、基本的な条件が満たされていることを確認します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; クライアント資格情報の取得 &#x200B;](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API ドキュメントに従った追加情報が提供されます。

   >[!TIP]
   >
   > クライアント資格情報はキャッシュし、無期限に使用する必要があります。

1. **アクセストークンの取得：** クライアントアプリケーションは、クライアントトークンエンドポイントを呼び出してアクセストークンを取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[&#x200B; アクセストークンの取得 &#x200B;](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API ドキュメントを参照してください。
   >
   > * `client_id`、`client_secret`、`grant_type` など、すべての _必須_ パラメーター
   > * `Content-Type`、`X-Device-Info` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **アクセストークンを返す：** クライアントトークンエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられたアクセストークンに関する情報が含まれています。

   >[!IMPORTANT]
   >
   > アクセストークン応答で提供される情報について詳しくは、[&#x200B; アクセストークンの取得 &#x200B;](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > クライアントトークンは、リクエストデータを検証して、基本的な条件が満たされていることを確認します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; アクセストークンの取得 &#x200B;](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API ドキュメントに準拠する追加情報が提供されます。

   >[!TIP]
   >
   > アクセストークンは、指定された時間内（例：24 時間の有効期間）にのみキャッシュおよび使用する必要があります。 有効期限が切れた後、クライアントアプリケーションは新しいアクセストークンをリクエストする必要があります。

1. **保護された API へのアクセスを続行：** クライアントアプリケーションはアクセストークンを使用して、他のAdobe Passで保護された API にアクセスします。 クライアントアプリケーションは、`Bearer` 認証スキーム（`Authorization: Bearer <access_token>` など）を使用して、`Authorization` リクエストヘッダーにアクセストークンを含める必要があります。

   >[!IMPORTANT]
   >
   > Adobe Passで保護された API は、アクセストークンを検証して、次の基本的な条件が満たされていることを確認します。
   >
   > * _access_ token_ は有効である必要があります。
   > * _access_ token _は、有効な_ client _id_ および _client_secret_ に関連付ける必要があります。
   > * _access_ token _は、有効な_ software_statement_ に関連付ける必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
