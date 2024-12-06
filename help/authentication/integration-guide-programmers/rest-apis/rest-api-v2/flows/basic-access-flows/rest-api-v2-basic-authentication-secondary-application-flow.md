---
title: 基本認証 – セカンダリアプリケーション – フロー
description: REST API V2 – 基本認証 – セカンダリアプリケーション – フロー
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2000'
ht-degree: 0%

---

# セカンダリ・アプリケーション内で実行される基本認証フロー {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

Adobe Pass認証使用権内の **認証フロー** により、ストリーミングアプリケーションはユーザーが有効な MVPD アカウントを持っていることを確認できます。 このプロセスでは、ユーザーがアクティブな MVPD アカウントを持ち、MVPD ログインページに有効なログイン資格情報を入力する必要があります。

次の場合は、認証フローが必要です。

* ユーザーが初めてアプリケーションを開いたとき。
* ユーザーの以前の認証が期限切れになった場合。
* ユーザーが MVPD アカウントからログアウトしたとき。
* ユーザーが別の MVPD で認証を行う場合。

これらの場合はすべて、プロファイルエンドポイントを呼び出すアプリケーションは、空の応答または 1 つ以上のプロファイル（ただし異なる MVPD の場合）を受け取ります。

**認証フロー** は、ユーザーエージェント（ブラウザー）が、アプリケーションからAdobe Pass バックエンド、MVPD ログインページの順に一連の呼び出しを実行し、最後にアプリケーションに戻る必要があります。 このフローには、MVPD システムへのリダイレクトと、各ドメインに保存される Cookie またはセッションの管理が含まれる場合があり、ユーザーエージェントなしでは達成と保護が困難な場合があります。

MVPD を選択し、ユーザーエージェントで選択された MVPD を使用して認証を行うためのユーザーインタラクションをサポートするプライマリアプリケーション（ストリーミングアプリケーション）の機能に基づいて、認証シナリオは次のようになります。

* [プライマリアプリケーション内での認証の実行](rest-api-v2-basic-authentication-primary-application-flow.md)
* [事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## 事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行 {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### 前提条件 {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

プライマリアプリケーション内で認証フローを開始し、セカンダリアプリケーション内のユーザーインタラクションを通じて認証フローを終了する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションは MVPD を選択する必要があります。
* ストリーミングアプリケーションは、選択した MVPD でログインするための認証セッションを開始する必要があります。
* セカンダリ・アプリケーションは、ユーザー・エージェントで選択した MVPD を使用して認証する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * ストリーミングアプリケーションは、MVPD を選択するためのユーザーインタラクションをサポートしています。
> * セカンダリ・アプリケーション（通常はセカンダリ・デバイス上）は、ユーザー・エージェント内の選択された MVPD を使用して認証を行うユーザー・インタラクションをサポートします。

### ワークフロー {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

次の図に示すように、事前に選択された MVPD を使用して、セカンダリ アプリケーション内で実行される基本認証フローを実装するには、次の手順に従います。

![ 事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行する ](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*事前に選択された mvpd を使用して、セカンダリ・アプリケーション内で認証を実行する*

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

   Adobe Pass バックエンドが有効なプロファイルを特定した場合、選択された MVPD でストリーミングアプリケーションを再認証する必要はありません。その後の決定フローで使用できるプロファイルが既に存在するからです。

1. **認証コードを表示：** セッションエンドポイント応答には、次のデータが含まれます。
   * セカンダリ・アプリケーション内で認証セッションを再開するために使用できる `code`。
   * `actionName` 属性は「authenticate」に設定されています。
   * `actionType` 属性は「interactive」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、ストリーミングアプリケーションは、セカンダリアプリケーション内で認証セッションを再開するために使用できる `code` を表示します。

1. **認証コードの検証：** セカンダリアプリケーションは、指定されたユーザー `code` 検証し、ユーザーエージェントの MVPD 認証で続行できることを確認します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 認証セッション情報の取得 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider` や `code` など、すべての _必須_ パラメーター
   > * `Authorization` のようなすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **認証セッションに関する情報を返す：** セッションエンドポイント応答には、次のデータが含まれています。
   * `existing` 属性には、既に指定されている既存のパラメーターが含まれます。
   * `missing` 属性には、認証フローを完了するために指定する必要がある、欠落しているパラメーターが含まれています。

   >[!IMPORTANT]
   >
   > セッション検証応答で提供される情報について詳しくは、[ 認証セッション情報の取得 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) API ドキュメントを参照してください。
   >
   > <br/>
   >
   > セッション エンドポイントは、基本的な条件が満たされていることを確認するために、リクエストデータを検証します。
   >
   > * _required_ パラメーターおよびヘッダーは有効である必要があります。
   >
   > <br/>
   >
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

   >[!TIP]
   >
   > 提案：セカンダリ・アプリケーションは、認証セッションが欠落していることを示すエラー応答が発生した場合に使用された `code` が無効であることをユーザーに通知し、新しいセッションを使用して再試行するようにユーザーに通知できます。

1. **ユーザーエージェントで URL を開く：** セカンダリアプリケーションがユーザーエージェントを開いて、自己計算された `url` を読み込み、認証エンドポイントにリクエストを送信します。 このフローには、複数のリダイレクトが含まれ、最終的にユーザーが MVPD ログインページに移動して、有効な資格情報を提供する場合があります。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ ユーザーエージェントでの認証の実行 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)API ドキュメントを参照してください。
   >
   > * `serviceProvider` や `code` など、すべての _必須_ パラメーター
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **Complete MVPD authentication:** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、プロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   > 
   > * `serviceProvider` や `code` など、すべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

   >[!TIP]
   >
   > 提案：ストリーミングアプリケーションでは、`code` を使用してポーリングメカニズムを実装し、通常のプロファイルが正常に生成および保存されたかどうかを確認できます。

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

## 事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行 {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### 前提条件 {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

プライマリアプリケーション内で認証フローを開始し、セカンダリアプリケーション内のユーザーインタラクションを通じて認証フローを終了する前に、次の前提条件が満たされていることを確認します。

* ストリーミングアプリケーションは、ログインする必要がある場合に認証セッションを開始する必要があります。
* セカンダリ アプリケーションは MVPD を選択する必要があります。
* セカンダリ・アプリケーションは、ユーザー・エージェントで選択した MVPD を使用して認証する必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * セカンダリ アプリケーション（通常はセカンダリ デバイス）は、MVPD を選択するためのユーザ インタラクションをサポートします。
> * セカンダリ・アプリケーション（通常はセカンダリ・デバイス上）は、ユーザー・エージェント内の選択された MVPD を使用して認証を行うユーザー・インタラクションをサポートします。

### ワークフロー {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

次の図に示すように、事前に選択された MVPD を使用せずに、セカンダリ アプリケーション内で実行される基本認証フローを実装するには、次の手順に従います。

![ 事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行 ](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*事前に選択された mvpd を使用せずに、セカンダリ・アプリケーション内で認証を実行*

1. **認証セッションの作成：** ストリーミングアプリケーションは、セッションエンドポイントを呼び出して認証セッションを開始するために必要なデータの一部を収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 認証セッションの作成 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider` のようなすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > 認証セッションの作成時、ストリーミングアプリケーションは 1 回の呼び出しで必要なすべてのパラメーターを提供することはできません。

1. **次のアクションを示す：** セッションエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドとして必要なデータが含まれます。
   * セカンダリ・アプリケーション内で認証セッションを再開するために使用できる `code`。
   * `actionName` 属性は「resume」に設定されます。
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
   >
   > <br/>
   > 
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../../features-standard/error-reporting/enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **認証コードを表示：** ストリーミングアプリケーションは、セカンダリアプリケーション内で認証セッションを再開するために使用できる `code` ードを表示します。

1. **認証セッションの不足しているパラメーターを指定する：** セカンダリ アプリケーションは、認証セッションの再開に必要なすべての不足しているデータを収集し、セッション エンドポイントを呼び出します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 認証セッションの再開 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`domainName`、`redirectUrl` などのすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

1. **次のアクションを示す：** セッションエンドポイント応答には、次のアクションに関するストリーミングアプリケーションのガイドに必要なデータが含まれています。

   >[!IMPORTANT]
   >
   > セッション応答で提供される情報について詳しくは、[ 認証セッションの再開 ](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API ドキュメントを参照してください。
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

   >[!TIP]
   >
   > 提案：セカンダリ・アプリケーションは、認証セッションが欠落していることを示すエラー応答が発生した場合に使用された `code` が無効であることをユーザーに通知し、新しいセッションを使用して再試行するようにユーザーに通知できます。

1. **既存のプロファイルを示す：** セッションエンドポイントの応答には、次のデータが含まれます。
   * `actionName` 属性は「authorize」に設定されます。
   * `actionType` 属性は「direct」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを特定した場合、選択された MVPD でストリーミングアプリケーションを再認証する必要はありません。その後の決定フローで使用できるプロファイルが既に存在するからです。

1. **ユーザーエージェントで URL を開く：** セッションエンドポイントの応答には、次のデータが含まれます。
   * MVPD ログインページ内でインタラクティブ認証を開始するために使用できる `url`。
   * `actionName` 属性は「authenticate」に設定されています。
   * `actionType` 属性は「interactive」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、セカンダリアプリケーションは User Agent を開いて指定されたプロファイ `url` を読み込み、Authenticate エンドポイントにリクエストを行います。 このフローには、複数のリダイレクトが含まれ、最終的にユーザーが MVPD ログインページに移動して、有効な資格情報を提供する場合があります。

1. **Complete MVPD authentication:** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** ストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信することで、プロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ 特定のコードのプロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)API ドキュメントを参照してください。
   >
   > * `serviceProvider` や `code` など、すべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー

   >[!TIP]
   >
   > 提案：ストリーミングアプリケーションでは、`code` を使用してポーリングメカニズムを実装し、通常のプロファイルが正常に生成および保存されたかどうかを確認できます。

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
