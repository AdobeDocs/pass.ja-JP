---
title: 基本的な事前認証 – プライマリ申請 – フロー
description: REST API V2 – 基本的な事前認証 – プライマリアプリケーション – フロー
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---


# プライマリアプリケーション内で実行される基本的な事前認証フロー {#basic-preauthorization-flow-performed-within-primary-application}

Adobe Pass認証使用権内の **事前認証フロー** により、ストリーミングアプリケーションは、MVPD がリソースのリストへのユーザーのアクセスを許可または拒否できるかどうかを判断できます。 この検証により、アプリケーションが、閲覧資格のあるコンテンツに関する正確な情報をユーザーに提示できるようになります。

## 特定の mvpd を使用した事前認証決定の取得 {#retrieve-preauthorization-decisions-using-specific-mvpd}

### 前提条件 {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

特定の MVPD を使用して事前認証の決定を取得する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションには、次のいずれかの基本認証フローを使用して MVPD に対して正常に作成された有効な標準プロファイルが必要です。
   * [プライマリアプリケーション内での認証の実行](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* ストリーミングアプリケーションは、リソースのリストを表示する事前認証の決定と、関連するステータスを取得したいと考えています。

### ワークフロー {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

次の図に示すように、プライマリ アプリケーション内で実行される特定の MVPD を使用した基本的な事前認証フローを実装するには、次の手順に従います。

![ 特定の mvpd を使用した事前認証決定の取得 ](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*特定の mvpd を使用した事前認証決定の取得*

1. **事前認証決定の取得：** ストリーミングアプリケーションは、決定の事前認証エンドポイントを呼び出すことにより、リソースのリストに対する事前認証決定を取得するために必要なすべてのデータを収集します。

   次の項目について詳しくは、[ 特定の mvpd を使用した事前認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   * すべての _オプション_ パラメーターおよびヘッダー

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **リクエストされたリソースの MVPD 決定を取得：** Adobe Pass サーバーは MVPD 事前認証エンドポイントを呼び出して、ストリーミングアプリケーションから受信した各リソースの `Permit` 定または `Deny` 定を取得します。

1. **再来訪の事前認証の決定：** 決定の事前認証エンドポイント応答には、各リソースの `Permit` または `Deny` の決定が含まれています。
   * `Permit` の決定は、リソースが再生可能であることを意味します。 事前認証フローはリソースの再生に使用できないので、応答にはメディアトークンが含まれていません。
   * `Deny` の決定は、リソースが再生可能でないことを意味します。 応答には、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従ったエラーペイロードが含まれています。

   決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した事前認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。

   >[!IMPORTANT]
   >
   > 決定の事前認証エンドポイントは、基本条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **事前認証の決定を処理：** ストリーミングアプリケーションは応答を処理し、それを使用して、オプションでユーザーインターフェイス上の各リソースの適切なステータスを表示できます。
