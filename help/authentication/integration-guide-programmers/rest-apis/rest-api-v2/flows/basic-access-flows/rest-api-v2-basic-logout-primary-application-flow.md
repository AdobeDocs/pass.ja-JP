---
title: 基本ログアウト - プライマリ適用 – フロー
description: REST API V2 – 基本ログアウト - プライマリアプリケーション – フロー
exl-id: 21dbff4a-0d69-4f81-b04f-e99d743c35b3
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# プライマリアプリケーション内で実行される基本的なログアウトフロー {#basic-logout-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

Adobe Pass認証使用権内の **ログアウトフロー** により、ストリーミングアプリケーションは次の 2 つの主な手順を実行できます。

* Adobe Pass バックエンドで保存された通常のプロファイルを削除します。
* ユーザーエージェント（ブラウザー）を使用してMVPD ログアウトエンドポイントに移動し、MVPD バックエンドでクリーンアップをトリガーします。

基本的なログアウトフローを使用すると、次のシナリオについてクエリを実行できます。

* [ログアウト エンドポイントを使用して特定の mvpd のログアウトを開始します。](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [ログアウトエンドポイントを使用しない特定の mvpd に対するログアウトの開始](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## ログアウトエンドポイントを持つ特定の mvpd に対してログアウトを開始します {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### 前提条件 {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

ログアウトエンドポイントを使用して特定のMVPDのログアウトを開始する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションには、次のいずれかの基本認証フローを使用してMVPD用に正常に作成された有効な標準プロファイルが必要です。
   * [プライマリアプリケーション内での認証の実行](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
* ストリーミングアプリケーションは、MVPDからログアウトする必要がある場合は、ログアウトフローを開始する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * MVPDは、ログアウトフローをサポートし、ログアウトエンドポイントを持っています。

### ワークフロー {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

次の図に示すように、プライマリアプリケーション内で実行されるログアウトエンドポイントを使用して、特定のMVPDの基本的なログアウトフローを実装するには、以下の手順に従います。

![ ログアウト エンドポイントを使用して特定の mvpd のログアウトを開始する ](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*ログアウト エンドポイントを使用して特定の mvpd のログアウトを開始する*

1. **Adobe Pass ログアウトの開始：** ストリーミングアプリケーションは、Adobe Pass ログアウトエンドポイントを呼び出して、ログアウトフローを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd のログアウトの開始 ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `mvpd` 必須 `redirectUrl` パラメーター
   > * _、_ などのすべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **標準プロファイルを削除：** Adobe Pass サーバーは、識別された標準プロファイルをAdobe Pass バックエンドから削除します。

1. **次のアクションを示す：** Adobe Pass ログアウトエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドとして必要なデータが含まれます。
   * MVPDがログアウトフローをサポートするので、`url` 属性が存在します。
   * `actionName` 属性は「logout」に設定されます。
   * `actionType` 属性は「interactive」に設定されます。

   >[!IMPORTANT]
   >
   > ログアウト応答で提供される情報について詳しくは、[ 特定の mvpd のログアウトの開始 ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > Adobe Pass ログアウトエンドポイントは、基本的な条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **MVPD ログアウトの開始：** ストリーミングアプリケーションは `url` を読み取り、ユーザーエージェントを使用してMVPDとのログアウトフローを開始します。 フローには、MVPD システムへの複数のリダイレクトが含まれる場合があります。 それでも、結果として、MVPDは内部クリーンアップを実行し、最終的なログアウト確認をAdobe Pass バックエンドに返します。

1. **ログアウト完了の指定：** ストリーミングアプリケーションは、ユーザーエージェントが指定された `redirectUrl` に到達するまで待機し、それをシグナルとして使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

## ログアウトエンドポイントを使用しない特定の mvpd に対するログアウトの開始 {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### 前提条件 {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

ログアウトエンドポイントを使用せずに特定のMVPDに対してログアウトを開始する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションには、次のいずれかの基本認証フローを使用してMVPD用に正常に作成された有効な標準プロファイルが必要です。
   * [プライマリアプリケーション内での認証の実行](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
* ストリーミングアプリケーションは、MVPDからログアウトする必要がある場合は、ログアウトフローを開始する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * MVPDはログアウトフローをサポートしておらず、ログアウトエンドポイントがありません。

### ワークフロー {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

次の図に示すように、プライマリアプリケーション内でログアウトエンドポイントを使用しない特定のMVPDに対して基本的なログアウトフローを実装するには、以下の手順に従います。

![ ログアウト エンドポイントを使用せずに、特定の mvpd に対してログアウトを開始する ](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*ログアウト エンドポイントを使用せずに、特定の mvpd に対してログアウトを開始する*

1. **Adobe Pass ログアウトの開始：** ストリーミングアプリケーションは、Adobe Pass ログアウトエンドポイントを呼び出して、ログアウトフローを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd のログアウトの開始 ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * _、_、`serviceProvider` など、すべての `mvpd` 必須 `redirectUrl` パラメーター
   > * _、_ などのすべての `Authorization` 必須 `AP-Device-Identifier` ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **標準プロファイルを削除：** Adobe Pass サーバーは、識別された標準プロファイルを削除します。

1. **次のアクションを示す：** Adobe Pass ログアウトエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドとして必要なデータが含まれます。
   * MVPDがログアウトフローをサポートしていないので、`url` 属性がありません。
   * `actionName` 属性は「complete」に設定されます。
   * `actionType` 属性は「none」に設定されます。

   >[!IMPORTANT]
   >
   > ログアウト応答で提供される情報について詳しくは、[ 特定の mvpd のログアウトの開始 ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > Adobe Pass ログアウトエンドポイントは、基本的な条件が満たされていることを確認するためにリクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   > * 指定した `serviceProvider` と `mvpd` の統合はアクティブである必要があります。
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **ログアウト完了の指定：** ストリーミングアプリケーションが応答を処理し、オプションでその応答を使用して、ユーザーインターフェイスに特定のメッセージを表示できます。
