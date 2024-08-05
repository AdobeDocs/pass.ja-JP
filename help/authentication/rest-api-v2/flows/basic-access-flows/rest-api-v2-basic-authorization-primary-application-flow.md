---
title: 基本権限 - プライマリアプリケーション - 流量
description: REST API V2 – 基本認証 – プライマリアプリケーション – フロー
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---


# プライマリアプリケーション内で実行される基本認証フロー {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 実装は、 [スロットリング メカニズム](/help/authentication/throttling-mechanism.md) のドキュメントによって制限されます。

エンタイトルメント内の **承認フロー** Adobe Pass Authentication ストリーミング アプリケーションは、MVPD がユーザーの内容のストリーミングリクエストを許可するか拒否するかを判断できます。 決定が `Permit`場合、応答にはメディアトークンが含まれます。 Adobe Passサーバーはメディアトークンに署名し、ストリーミングアプリケーションがメディアトークン検証ツールライブラリを使用して、ストリームが解放される前にその信頼性を確認できるようにします。

メディア トークン検証ツール ライブラリによる検証は、CDN からストリームを解放するためのアクセス許可のチェーンにリンクされているストリーミング アプリケーション バックエンド サービスで行う必要があります。

## 特定の MVPD を使用して認証判断を取得する {#retrieve-authorization-decisions-using-specific-mvpd}

### 前提 条件 {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

特定の MVPD を使用して認証の決定を取得する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションには、次のいずれかの基本認証フローを使用して MVPD に対して正常に作成された有効な標準プロファイルが必要です。
   * [プライマリアプリケーション内での認証の実行](./rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](./rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* ストリーミングアプリケーションは、選択したリソースユーザー再生する前に、認証の決定を取得する必要があります。

### ワークフロー {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

次の図に示すように、プライマリ アプリケーション内で実行される特定の MVPD を使用して基本的な認証フロー実装するには、上記の手順に従います。

![特定の MVPD を使用して認証判断を取得する](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*特定の MVPD を使用して認証判断を取得する*

1. **認証決定の取得:** ストリーミング アプリケーションは、Decision Authorize エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 詳細については、 [特定の mvpd を使用して認証の判断を取得する](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **要求されたリソースの MVPD 決定を取得する:** Adobe Pass サーバーは MVPD 認証 エンドポイントを呼び出して、ストリーミング アプリケーションから受信した特定のリソースの `Permit` または `Deny` 決定を取得します。

1. **メディアトークンを使用して決定 `Permit` を返します。** Decision Authorize エンドポイント応答には、 `Permit` 決定とメディアトークンが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報の詳細については、 [特定の mvpd を使用して認証決定を取得する](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **メディアトークンでストリームを開始：** ストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

1. **詳細を含んだ決定 `Deny` 返す：** 決定の承認承認エンドポイント応答には、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った `Deny` 決定とエラーペイロードが含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > 決定の認証エンドポイントは、基本条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _必須_&#x200B;パラメーターおよびヘッダーは有効である必要があります。
   > * 指定された `serviceProvider` と `mvpd` 間の統合がアクティブになっている必要があります。
   >
   > <br/>
   > 
   > 検証が失敗すると、エラー応答が生成され、 [強化機能 エラーコード](../../../enhanced-error-codes.md) ドキュメントに準拠した追加情報が提供されます。

1. **決定の詳細 `Deny` 処理する:** ストリーミング アプリケーションは、応答からのエラー情報を処理し、オプションでそれを使用して、ユーザーインターフェイスに特定のメッセージを表示できます。
