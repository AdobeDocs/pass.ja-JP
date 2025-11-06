---
title: 基本認証 – プライマリ申請 – フロー
description: REST API V2 – 基本認証 – プライマリアプリケーション – フロー
exl-id: 46bc9326-966e-44fc-8546-2f58be01b7bc
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# プライマリアプリケーション内で実行される基本認証フロー {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

Adobe Pass認証使用権内の **認証フロー** により、ストリーミングアプリケーションは、MVPDがコンテンツのストリーミングリクエストを許可するか拒否するかを決定できます。 決定が `Permit` の場合、応答にはメディアトークンが含まれます。 Adobe Pass サーバーがメディアトークンに署名すると、ストリーミングアプリケーションでメディアトークン検証用ライブラリを使用して、ストリームがリリースされる前に信頼性を確認できます。

メディアトークン検証ライブラリを使用した検証は、CDN からストリームを解放するための権限チェーンでリンクされているストリーミングアプリケーションバックエンドサービスで行う必要があります。

## 特定の mvpd を使用した認証決定の取得 {#retrieve-authorization-decisions-using-specific-mvpd}

### 前提条件 {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

特定のMVPDを使用して認証決定を取得する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションには、次のいずれかの基本認証フローを使用してMVPD用に正常に作成された有効な標準プロファイルが必要です。
   * [プライマリアプリケーション内での認証の実行](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
* ストリーミングアプリケーションは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

### ワークフロー {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

次の図に示すように、プライマリアプリケーション内で実行される特定のMVPDを使用した基本的な認証フローを実装するには、以下の手順に従います。

![&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*特定の mvpd を使用した認証決定の取得*

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd を使用した認証決定の取得 &#x200B;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `mvpd` 必須 `resources` パラメーター
   > * _や_ など、すべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **リクエストされたリソースのMVPD決定を取得：** Adobe Pass サーバーは、MVPD Authorization エンドポイントを呼び出して、ストリーミングアプリケーションから受信した特定のリソースに関する `Permit` 定または `Deny` 定の決定を取得します。

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
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **メディアトークンでストリームを開始：** ストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

1. **詳細を含んだ決定 `Deny` 返す：** 決定の承認承認エンドポイント応答には、`Deny` 拡張エラーコード [&#x200B; ドキュメントに従った &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) 決定とエラーペイロードが含まれています。

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
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定の詳細 `Deny` 処理：** ストリーミングアプリケーションは、応答からのエラー情報を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。
