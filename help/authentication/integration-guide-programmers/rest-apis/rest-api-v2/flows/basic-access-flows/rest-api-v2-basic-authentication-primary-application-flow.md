---
title: 基本認証 – プライマリアプリケーション – フロー
description: REST API V2 – 基本認証 – プライマリアプリケーション – フロー
exl-id: 8122108d-e9da-43c5-9abb-ab177cb21eb6
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 0%

---

# プライマリアプリケーション内で実行される基本認証フロー {#basic-authentication-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

Adobe Pass認証使用権内の **認証フロー** により、ストリーミングアプリケーションはユーザーが有効なMVPD アカウントを持っていることを確認できます。 このプロセスを実行するには、MVPDのアクティブなアカウントを持ち、MVPDのログインページに有効なログイン資格情報を入力する必要があります。

次の場合は、認証フローが必要です。

* ユーザーが初めてアプリケーションを開いたとき。
* ユーザーの以前の認証が期限切れになった場合。
* ユーザーがMVPD アカウントからログアウトしたとき。
* ユーザーが別のMVPDで認証を行う場合。

これらの場合はすべて、プロファイルエンドポイントを呼び出すアプリケーションは、空の応答または 1 つ以上のプロファイル（ただし異なる MVPD の場合）を受け取ります。

**認証フロー** では、ユーザーエージェント（ブラウザー）が、アプリケーションからAdobe Pass バックエンド、次にMVPD ログインページ、最後にアプリケーションに一連の呼び出しを実行する必要があります。 このフローには、MVPD システムへのリダイレクトと、ドメインごとに保存される Cookie やセッションの管理が含まれる場合があります。これらは、ユーザーエージェントを使用せずに達成し、保護することが困難な場合があります。

ユーザーインタラクションをサポートしてMVPDを選択し、ユーザーエージェントで選択されたMVPDを使用して認証を行うプライマリアプリケーション（ストリーミングアプリケーション）の機能に基づいて、認証シナリオは次のようになります。

* [プライマリアプリケーション内での認証の実行](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)
* [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](rest-api-v2-basic-authentication-secondary-application-flow.md)

## プライマリアプリケーション内での認証の実行 {#perform-authentication-within-primary-application}

### 前提条件 {#prerequisites-perform-authentication-within-primary-application}

プライマリアプリケーション内でユーザーインタラクションを通じて認証を実行する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションは、MVPDを選択する必要があります。
* ストリーミングアプリケーションは、選択したMVPDでログインするための認証セッションを開始する必要があります。
* ストリーミングアプリケーションは、ユーザーエージェントで選択したMVPDを使用して認証する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * ストリーミングアプリケーションは、MVPDを選択するためのユーザーインタラクションをサポートしています。
> * ストリーミングアプリケーションは、ユーザーエージェント内の選択されたMVPDを使用して認証を行うユーザーインタラクションをサポートしています。

### ワークフロー {#workflow-perform-authentication-completed-on-primary-application}

次の図に示すように、プライマリ・アプリケーション内で実行される基本認証フローを実装するには、次の手順に従います。

![ プライマリアプリケーション内での認証の実行 ](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*プライマリアプリケーション内での認証の実行*

1. **認証セッションの作成：** ストリーミングアプリケーションは、セッションエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 認証セッションの作成 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   > 
   > * `serviceProvider`、`mvpd`、`domainName`、`redirectUrl` などのすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   > 
   > <br/>
   > 
   > ストリーミングアプリケーションは、認証セッションを作成する際に、1 回の呼び出しで必要なすべてのパラメーターを指定する必要があります。

1. **次のアクションを示す：** セッションエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドに必要なデータが含まれています。

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

1. **決定フローで続行：** セッションエンドポイント応答には、次のデータが含まれます。
   * `actionName` 属性は「authorize」に設定されます。
   * `actionType` 属性は「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを特定した場合、その後の決定フローに使用できるプロファイルが既に存在するので、ストリーミングアプリケーションは選択したMVPDで再認証する必要はありません。

1. **ユーザーエージェントで URL を開く：** セッションエンドポイントの応答には、次のデータが含まれます。
   * MVPDのログインページ内でインタラクティブ認証を開始するために使用できる `url`。
   * `actionName` 属性は「authenticate」に設定されています。
   * `actionType` 属性は「interactive」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、ストリーミングアプリケーションはユーザーエージェントを開いて指定されたプロファイ `url` を読み込み、Authenticate エンドポイントにリクエストを送信します。 このフローには、複数のリダイレクトが含まれる場合があり、最終的にユーザーがMVPDのログインページに移動して、有効な資格情報を提供します。

1. **MVPD認証の完了：** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、プロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`code` などのすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

   >[!TIP]
   >
   > 提案：ストリーミングアプリケーションは、ユーザーエージェントが指定された `redirectUrl` に到達するのを待って、通常のプロファイルが正常に生成および保存されたかどうかを確認できます。

1. **通常のプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた通常のプロファイルに関する情報が含まれます。

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
