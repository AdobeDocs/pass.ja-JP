---
title: 基本認証 – プライマリアプリケーション – フロー
description: REST API V2 – 基本認証 – プライマリアプリケーション – フロー
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---


# プライマリアプリケーション内で実行される基本認証フロー {#basic-authentication-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) のドキュメントで制限されています。

Adobe Pass認証使用権内の **認証フロー** により、ストリーミングアプリケーションはユーザーが有効な MVPD アカウントを持っていることを確認できます。 このプロセスでは、ユーザーがアクティブな MVPD アカウントを持ち、MVPD ログインページに有効なログイン資格情報を入力する必要があります。

次の場合は、認証フローが必要です。

* ユーザーが初めてアプリケーションを開いたとき。
* ユーザーの以前の認証が期限切れになった場合。
* ユーザーが MVPD アカウントからログアウトしたとき。
* ユーザーが別の MVPD で認証を行う場合。

これらの場合はすべて、プロファイルエンドポイントを呼び出すアプリケーションは、空の応答または 1 つ以上のプロファイル（ただし異なる MVPD の場合）を受け取ります。

**認証フロー** は、ユーザーエージェント（ブラウザー）が、アプリケーションからAdobe Pass バックエンド、MVPD ログインページの順に一連の呼び出しを実行し、最後にアプリケーションに戻る必要があります。 このフローには、MVPD システムへのリダイレクトと、各ドメインに保存される Cookie またはセッションの管理が含まれる場合があり、ユーザーエージェントなしでは達成と保護が困難な場合があります。

MVPD を選択し、ユーザーエージェントで選択された MVPD を使用して認証を行うためのユーザーインタラクションをサポートするプライマリアプリケーション（ストリーミングアプリケーション）の機能に基づいて、認証シナリオは次のようになります。

* [プライマリアプリケーション内での認証の実行](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## プライマリアプリケーション内での認証の実行 {#perform-authentication-within-primary-application}

### 前提条件 {#prerequisites-perform-authentication-within-primary-application}

プライマリアプリケーション内でユーザーインタラクションを通じて認証を実行する前に、次の前提条件が満たされていることを確認してください。

* ストリーミングアプリケーションは MVPD を選択する必要があります。
* ストリーミングアプリケーションは、選択した MVPD でログインするための認証セッションを開始する必要があります。
* ストリーミングアプリケーションは、ユーザーエージェント内の選択された MVPD で認証する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * ストリーミングアプリケーションは、MVPD を選択するためのユーザーインタラクションをサポートしています。
> * ストリーミングアプリケーションは、ユーザーエージェント内の選択された MVPD を使用して認証を行うユーザーインタラクションをサポートしています。

### ワークフロー {#workflow-perform-authentication-completed-on-primary-application}

次の図に示すように、プライマリ・アプリケーション内で実行される基本認証フローを実装するには、次の手順に従います。

![ プライマリアプリケーション内での認証の実行 ](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定フローで続行：** セッションエンドポイント応答には、次のデータが含まれます。
   * `actionName` 属性は「authorize」に設定されます。
   * `actionType` 属性は「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを特定した場合、選択された MVPD でストリーミングアプリケーションを再認証する必要はありません。その後の決定フローで使用できるプロファイルが既に存在するからです。

1. **ユーザーエージェントで URL を開く：** セッションエンドポイントの応答には、次のデータが含まれます。
   * MVPD ログインページ内でインタラクティブ認証を開始するために使用できる `url`。
   * `actionName` 属性は「authenticate」に設定されています。
   * `actionType` 属性は「interactive」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、ストリーミングアプリケーションはユーザーエージェントを開いて指定されたプロファイ `url` を読み込み、Authenticate エンドポイントにリクエストを送信します。 このフローには、複数のリダイレクトが含まれ、最終的にユーザーが MVPD ログインページに移動して、有効な資格情報を提供する場合があります。

1. **Complete MVPD authentication:** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、プロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`code` などのすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

   >[!NOTE]
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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。
