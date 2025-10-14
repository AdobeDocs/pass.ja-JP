---
title: シングル ログアウト – フロー
description: REST API V2 - シングルログアウト – フロー
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# シングルログアウトフロー {#single-logout-flow}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

## 特定の mvpd に対するシングルログアウトの開始 {#initiate-single-logout-for-specific-mvpd}

### 前提条件 {#prerequisites-initiate-single-logout-for-specific-mvpd}

特定のMVPDに対してシングルログアウトを開始する前に、次の前提条件が満たされていることを確認してください。

* 2 つ目のストリーミングアプリケーションには、シングルサインオン認証フローの 1 つを使用してMVPD用に正常に作成された有効なシングルサインオンプロファイルが必要です。
   * [プラットフォーム ID を使用したシングルサインオンによる認証の実行](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [サービストークンを使用したシングルサインオンによる認証の実行](rest-api-v2-single-sign-on-service-token-flows.md)
* 2 つ目のストリーミングアプリケーションは、MVPDからログアウトする必要がある場合、シングルログアウトフローを開始する必要があります。

>[!IMPORTANT]
> 
> 前提
>
> <br/>
> 
> * 第 1 および第 2 のストリーミングアプリケーションは、`JWS` または `JWE` と同じ一意のプラットフォーム識別子ペイロード、または `JWS` と同じ一意のユーザー識別子ペイロードを取得する。

### ワークフロー {#workflow-initiate-single-logout-for-specific-mvpd}

次の図に示すように、特定のMVPDに対してシングルログアウトフローを実装するには、以下の手順を実行します。

![&#x200B; 特定の mvpd に対してシングルログアウトを開始する &#x200B;](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*特定の mvpd に対してシングルログアウトを開始する*

1. **Adobe Pass ログアウトの開始：** ストリーミングアプリケーションは、Adobe Pass ログアウトエンドポイントを呼び出して、ログアウトフローを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[&#x200B; 特定の mvpd のログアウトの開始 &#x200B;](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`redirectUrl` など、すべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子または一意のユーザー識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。
   > 
   > <br/>
   > 
   > ヘッダーについて詳 `AD-Service-Token` くは、[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md) ドキュメントを参照してください。

1. **標準およびシングルサインオンプロファイルの検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて、標準およびシングルサインオンの有効なプロファイルの両方を識別します。

1. **通常のサインオンプロファイルとシングルサインオンプロファイルを削除する：** Adobe Pass サーバーは、識別された通常のサインオンプロファイルとシングルサインオンプロファイルをAdobe Pass バックエンドから削除します。

1. **次のアクションを示す：** Adobe Pass ログアウトエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドに必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > ログアウト応答で提供される情報について詳しくは、[&#x200B; 特定の mvpd のログアウトの開始 &#x200B;](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API ドキュメントを参照してください。
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
   > 検証に失敗した場合は、エラー応答が生成され、[&#x200B; 拡張エラーコード &#x200B;](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **ログアウト完了を示す：** MVPDがログアウトフローをサポートしていない場合、ストリーミングアプリケーションが応答を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

1. **MVPD ログアウトの開始：** MVPDがログアウトフローをサポートしている場合、ストリーミングアプリケーションが応答を処理し、ユーザーエージェントを使用してMVPDとのログアウトフローを開始します。 フローには、MVPD システムへの複数のリダイレクトが含まれる場合があります。 それでも、結果として、MVPDは内部クリーンアップを実行し、最終的なログアウト確認をAdobe Pass バックエンドに返します。

1. **ログアウト完了の指定：** ストリーミングアプリケーションは、ユーザーエージェントが指定された `redirectUrl` に到達するまで待機し、それをシグナルとして使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

>[!NOTE]
>
> 単一ログアウトフローの手順は、最初のストリーミングアプリケーションから開始した場合は前述と同じです。
