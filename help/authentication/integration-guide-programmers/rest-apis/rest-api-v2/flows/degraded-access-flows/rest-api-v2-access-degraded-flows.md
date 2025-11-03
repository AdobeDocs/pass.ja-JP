---
title: アクセス フローの低下
description: REST API V2 – 低下したアクセスフロー
exl-id: 9276f5d9-8b1a-4282-8458-0c1e1e06bcf5
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '1615'
ht-degree: 0%

---

# アクセスフローの低下 {#degraded-access-flows}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

パフォーマンスが低下すると、特定のMVPD認証および承認エンドポイントが一時的にバイパスされます。 通常、プログラマはこのアクションを開始しますが、誰がデグレード イベントをトリガーしているかに関係なく、このアクションは影響を受ける MVPD との事前の取り決めに依存します。

最適化機能について詳しくは、[ 最適化 ](/help/premium-workflow/degraded-access/degradation-feature.md) ドキュメントを参照してください。

縮退アクセスフローを使用すると、次のシナリオについてクエリを実行できます。

* [パフォーマンス低下が適用されている間に認証を実行](#perform-authentication-while-degradation-is-applied)
* [劣化適用中の認証決定の取得](#retrieve-authorization-decisions-while-degradation-is-applied)
* [劣化適用中の事前認証決定の取得](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [最適化適用中にプロファイルを取得](#retrieve-profile-while-degradation-is-applied)

## パフォーマンス低下が適用されている間に認証を実行 {#perform-authentication-while-degradation-is-applied}

### 前提条件 {#prerequisites-perform-authentication-while-degradation-is-applied}

低下が適用されている場合の認証フローを実行する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションは、MVPDでログインする必要がある場合、認証セッションを開始する必要があります。

>[!IMPORTANT]
> 
> 前提
> 
> <br/>
> 
> * ストリーミングアプリケーションには、Adobe Pass バックエンドに保存されたその特定のMVPDに対する有効なプロファイルがありません。
> * 指定された `serviceProvider` と `mvpd` の間の統合に適用される AuthNAll 低下ルールがあります。

### ワークフロー {#workflow-perform-authentication-while-degradation-is-applied}

次の図に示すように、低下が適用されている間に認証フローを実装するには、次の手順に従います。

![ パフォーマンス低下が適用されている間に認証を実行 ](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*パフォーマンス低下が適用されている間に認証を実行*

1. **認証セッションの作成：** ストリーミングアプリケーションは、セッションエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 認証セッションの作成 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   > 
   > * _、_、`serviceProvider`、`mvpd` などのすべての `domainName` 必須 `redirectUrl` パラメーター
   > * _や_ など、すべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **最適化規則の確認：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された AuthNAll 最適化規則があるかどうかを確認します。

1. **次のアクションを示す：** セッションエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドとして必要なデータが含まれます。
   * `actionName` 属性は「authorize」に設定されます。
   * `actionType` 属性は「direct」に設定されます。

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
   > 基本検証が失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   > 
   > セッション エンドポイントは要求データを使用して、縮退したアクセス条件が満たされているかどうかを確認します。
   >
   > * 指定された `serviceProvider` と `mvpd` の統合には、AuthNAll 低下ルールが適用されている必要があります。
   >
   > <br/>
   > 
   > 縮退アクセスの検証に失敗した場合、応答はデフォルトで基本認証フローに設定されます。

1. **決定フローで続行：** ストリーミングアプリケーションは、後続の決定フローで続行できます。

## 劣化適用中の認証決定の取得 {#retrieve-authorization-decisions-while-degradation-is-applied}

### 前提条件 {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

劣化適用中の認証決定を取得する前に、次の前提条件を満たしていることを確認してください。

* ストリーミングアプリケーションは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * ストリーミングアプリケーションには、その特定のMVPDに対する有効なプロファイルがありません。
> * 指定された `serviceProvider` と `mvpd` の間の統合に、AuthZAll または AuthNAll の最適化規則が適用されています。

### ワークフロー {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

次の図に示すように、低下が適用されている間に認証フローを実装するには、次の手順に従います。

![ 最適化適用中に認証決定を取得 ](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*最適化適用中に認証決定を取得*

1. **認証決定の取得：** ストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   > 
   > 次の項目について詳しくは、[ 特定の mvpd を使用した認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `mvpd` 必須 `resources` パラメーター
   > * _や_ など、すべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **最適化規則の確認：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された AuthZAll または AuthNAll の最適化規則があるかどうかを確認します。

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
   > 基本検証が失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   >
   > 決定の認証エンドポイントは、リクエストデータを使用して、低下したアクセス条件が満たされているかどうかを確認します。
   >
   > * 指定された `serviceProvider` と `mvpd` の間の統合には、AuthZAll または AuthNAll 低下ルールが適用されている必要があります。
   >
   > <br/>
   > 
   > 縮退アクセスの検証に失敗した場合、応答はデフォルトで基本認証フローに設定されます。

1. **メディアトークンでストリームを開始：** ストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

## 劣化適用中の事前認証決定の取得 {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### 前提条件 {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

劣化適用中に事前認証の決定を取得する前に、次の前提条件を満たしていることを確認してください。

* ストリーミングアプリケーションは、リソースのリストを表示する事前認証の決定と、関連するステータスを取得したいと考えています。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * ストリーミングアプリケーションには、その特定のMVPDに対する有効なプロファイルがありません。
> * 指定された `serviceProvider` と `mvpd` の間の統合に、AuthZAll または AuthNAll の最適化規則が適用されています。

### ワークフロー {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

次の図に示すように、事前認証フローを実装し、パフォーマンス低下が適用されている間は、次の手順に従います。

![ 最適化適用中に事前認証の決定を取得 ](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*最適化適用中に事前認証の決定を取得*

1. **事前認証決定の取得：** ストリーミングアプリケーションは、決定の事前認証エンドポイントを呼び出すことにより、リソースのリストに対する事前認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd を使用した事前認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `mvpd` 必須 `resources` パラメーター
   > * _や_ など、すべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **最適化規則の確認：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された AuthZAll または AuthNAll の最適化規則があるかどうかを確認します。

1. **再来訪の事前認証の決定：** 決定の事前認証エンドポイント応答には、各リソースの `Permit` の決定が含まれています。

   >[!IMPORTANT]
   >
   > 決定応答で提供される情報について詳しくは、[ 特定の mvpd を使用した事前認証の決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   >
   > 決定の事前認証エンドポイントは、リクエストデータを使用して、劣化したアクセス条件が満たされているかどうかを確認します。
   >
   > * 指定された `serviceProvider` と `mvpd` の間の統合には、AuthZAll または AuthNAll 低下ルールが適用されている必要があります。
   >
   > <br/>
   > 
   > 縮退アクセスの検証に失敗した場合、応答はデフォルトで基本的な事前認証フローに設定されます。

1. **事前認証の決定を処理：** ストリーミングアプリケーションは応答を処理し、それを使用して、オプションでユーザーインターフェイス上の各リソースの適切なステータスを表示できます。

## 最適化適用中にプロファイルを取得 {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> パフォーマンス低下が適用されている場合、プロファイルエンドポイントクエリはオプションです。
>
> <br/>
> 
> セッションエンドポイント応答は、低下が適用されている間、決定フローを続行するようにアプリケーションに指示します。 詳しくは、[ パフォーマンス低下が適用されている間に認証を実行する ](#perform-authentication-while-degradation-is-applied) の節を参照してください。

### 前提条件 {#prerequisites-retrieve-profile-while-degradation-is-applied}

低下が適用されている場合に特定のMVPDのプロファイルを取得する前に、次の前提条件が満たされていることを確認してください。

* 選択されたまたはキャッシュされた `mvpd` ーザー ID を持つストリーミングアプリケーションが、特定のMVPDのプロファイルを取得したいと考えています。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * ストリーミングアプリケーションには、その特定のMVPDに対する有効なプロファイルがありません。
> * 指定された `serviceProvider` と `mvpd` の間の統合に適用される AuthNAll 低下ルールがあります。

### ワークフロー {#workflow-retrieve-profile-while-degradation-is-applied}

次の図に示すように、パフォーマンス低下が適用されている場合に特定のMVPDのプロファイル取得フローを実装するには、次の手順に従います。

![ 最適化適用中にプロファイルを取得 ](/help/authentication/assets/rest-api-v2/flows/degraded-access-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*最適化適用中にプロファイルを取得*

1. **特定の mvpd のプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信して、その特定のMVPDのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定の mvpd のプロファイルを取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
   >
   > * _、_ など、すべての `serviceProvider` 必須 `mvpd` パラメーター
   > * _や_ など、すべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **最適化規則の確認：** Adobe Pass サーバーは、指定された `serviceProvider` と `mvpd` の間の統合に適用された AuthNAll 最適化規則があるかどうかを確認します。

1. **機能縮退プロファイルに関する情報を返す：** プロファイルエンドポイント応答には、属性 `type` 「機能縮退」に設定されているなど、機能縮退プロファイルに関する情報が含まれています。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[ 特定の mvpd のプロファイルを取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)API ドキュメントを参照してください。
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
   > 基本検証が失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
   >
   > <br/>
   > 
   > プロファイルエンドポイントはリクエストデータを使用して、低下したアクセス条件が満たされているかどうかを確認します。
   >
   > * 指定された `serviceProvider` と `mvpd` の統合には、AuthNAll 低下ルールが適用されている必要があります。
   >
   > <br/>
   > 
   > 縮退したアクセス検証が失敗した場合、応答はデフォルトで基本プロファイル取得フローに設定されます。

1. **決定フローで続行：** プロファイルエンドポイント応答にプロファイルが含まれている場合、ストリーミングアプリケーションは劣化したプロファイル情報を使用して、後続の決定フローを続行します。

1. **新しい基本認証フローを指定：** プロファイルエンドポイント応答にプロファイルが含まれていない場合、ストリーミングアプリケーションは新しい基本認証フローを開始するようにユーザーに指示します。

>[!NOTE]
>
> 特定の認証コードのプロファイル取得フローの手順は前述と同じですが、使用するエンドポイントが「[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) ドキュメントに記載されているエンドポイントである点が異なります。
