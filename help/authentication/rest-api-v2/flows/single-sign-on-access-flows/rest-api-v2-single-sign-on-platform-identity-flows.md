---
title: シングルサインオン - Platform Id - フロー
description: REST API V2 - シングルサインオン - Platform Id - フロー
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# プラットフォーム ID フローを使用したシングルサインオン {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) のドキュメントで制限されています。

Platform ID 方式を使用すると、複数のアプリケーションで一意の Platform ID を使用して、Adobe Pass サービスの使用時にデバイスレベルまたはプラットフォームレベルでシングルサインオン（SSO）を実現できます。

アプリケーションは、デバイス固有の ID サービスやAdobe Pass システム外のライブラリを使用して、一意の Platform ID ペイロードを取得する役割を果たします。

アプリケーションは、この一意のプラットフォーム識別子ペイロードを、それを指定するすべてのリクエストの `Adobe-Subject-Token` ヘッダーの一部として含める責任があります。

`Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe – 件名 – トークン ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

## プラットフォーム ID を使用したシングルサインオンによる認証の実行 {#perform-authentication-through-single-sign-on-using-platform-identity}

### 前提条件 {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

プラットフォーム ID を使用したシングルサインオンを通じた認証フローを実行する前に、次の前提条件が満たされていることを確認してください。

* プラットフォームは、同じデバイスまたはプラットフォーム上のすべてのアプリケーションで、一貫した情報を `JWS` または `JWE` ペイロードとして返す ID サービスまたはライブラリを提供する必要があります。
* 最初のストリーミングアプリケーションは、一意のプラットフォーム ID を取得し、それを指定するすべてのリクエストの [Payload-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) Adobeの一部として `JWS` または `JWE` ペイロードを含める必要があります。
* 最初のストリーミングアプリケーションは MVPD を選択する必要があります。
* 最初のストリーミングアプリケーションは、選択した MVPD でログインするための認証セッションを開始する必要があります。
* 最初のストリーミングアプリケーションは、ユーザーエージェントで選択された MVPD を使用して認証する必要があります。
* 2 つ目のストリーミングアプリケーションは、一意のプラットフォーム ID を取得し、それを指定するすべてのリクエストの [Payload-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) Adobeの一部として `JWS` または `JWE` ペイロードを含める必要があります。

>[!IMPORTANT]
>
> 前提
>
> <br/>
> 
> * 最初のストリーミングアプリケーションは、MVPD を選択するためのユーザーインタラクションをサポートしています。
> * 最初のストリーミングアプリケーションは、ユーザーエージェント内の選択された MVPD で認証するユーザーインタラクションをサポートしています。

### ワークフロー {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

次の図に示すように、プラットフォーム ID を使用したシングルサインオンを通じた認証フローを実装するには、指定された手順を実行します。

![Platform ID を使用したシングルサインオンによる認証の実行 ](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Platform ID を使用したシングルサインオンによる認証の実行*

1. **プラットフォーム ID の取得：** 最初のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスまたはライブラリを呼び出して、一意のプラットフォーム ID に関連付けられた `JWS` または `JWE` ペイロードを取得します。

1. **JWS または JWE として一意のプラットフォーム ID を返します：** 最初のストリーミングアプリケーションでは、応答データを検証して、基本的なセキュリティ条件が満たされていることを確認します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名済みまたは暗号化されている。

1. **認証セッションの作成：** 最初のストリーミングアプリケーションは、セッションエンドポイントを呼び出して認証セッションを開始するために必要なすべてのデータを収集します。

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
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe – 件名 – トークン ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

1. **次のアクションを示す：** セッションエンドポイント応答には、次のアクションに関する最初のストリーミングアプリケーションをガイドするために必要なデータが含まれています。

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

1. **ユーザーエージェントで URL を開く：** セッションエンドポイントの応答には、次のデータが含まれます。
   * MVPD ログインページ内でインタラクティブ認証を開始するために使用できる `url`。
   * `actionName` 属性は「authenticate」に設定されています。
   * `actionType` 属性は「interactive」に設定されます。

   Adobe Pass バックエンドが有効なプロファイルを識別できない場合、最初のストリーミングアプリケーションがユーザーエージェントを開いて指定された `url` を読み込み、Authenticate エンドポイントにリクエストを送信します。 このフローには、複数のリダイレクトが含まれ、最終的にユーザーが MVPD ログインページに移動して、有効な資格情報を提供する場合があります。

1. **Complete MVPD authentication:** 認証フローが正常に完了すると、ユーザーエージェントインタラクションは通常のプロファイルをAdobe Pass バックエンドに保存し、指定された `redirectUrl` に到達します。

1. **特定のコードのプロファイルを取得：** 最初のストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信してプロファイル情報を取得するために必要なすべてのデータを収集します。

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

1. **標準プロファイルを検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なプロファイルを識別します。

1. **通常のプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた、見つかったプロファイルに関する情報が含まれます。

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

1. **決定フローで続行：** 最初のストリーミングアプリケーションは、後続の決定フローで続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe – 件名 – トークン ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

1. **プラットフォーム ID の取得：** 2 つ目のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスまたはライブラリを呼び出し、一意のプラットフォーム ID に関連付けられた `JWS` または `JWE` ペイロードを取得します。

1. **JWS または JWE として一意のプラットフォーム ID を返します：** 2 番目のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名済みまたは暗号化されている。

1. **プロファイルの取得：** 2 番目のストリーミングアプリケーションは、プロファイルエンドポイントにリクエストを送信して、すべてのプロファイル情報を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次について詳しくは、[ プロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API ドキュメントを参照してください。
   > 
   > * `serviceProvider` のようなすべての _必須_ パラメーター
   > * `Authorization`、`AP-Device-Identifier` などのすべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe – 件名 – トークン ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

1. **シングルサインオンプロファイルの検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なシングルサインオンプロファイルを識別します。

1. **シングルサインオンプロファイルに関する情報を返す：** プロファイルエンドポイント応答には、受信したパラメーターとヘッダーに関連付けられた、見つかったプロファイルに関する情報が含まれます。

   >[!IMPORTANT]
   >
   > プロファイル応答で提供される情報について詳しくは、[ プロファイルの取得 ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API ドキュメントを参照してください。
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

1. **決定フローで続行：** 2 番目のストリーミングアプリケーションは、後続の決定フローで続行できます。

   >[!IMPORTANT]
   >
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe – 件名 – トークン ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

## Platform ID を使用したシングルサインオンを通じた認証決定の取得{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### 前提条件 {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

プラットフォーム ID を使用してシングルサインオン経由で認証フローを実行する前に、次の前提条件が満たされていることを確認してください。

* プラットフォームは、同じデバイスまたはプラットフォーム上のすべてのアプリケーションで、一貫した情報を `JWS` または `JWE` ペイロードとして返す ID サービスまたはライブラリを提供する必要があります。
* 2 つ目のストリーミングアプリケーションは、一意のプラットフォーム ID を取得し、それを指定するすべてのリクエストの [Payload-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) Adobeの一部として `JWS` または `JWE` ペイロードを含める必要があります。
* 2 つ目のストリーミングアプリケーションでは、ユーザーが選択したリソースを再生する前に、認証決定を取得する必要があります。

>[!IMPORTANT]
>
> 前提
> 
> <br/>
> 
> * 最初のストリーミング アプリケーションは認証を実行し、[Authentication-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) リクエスト Adobeーに有効な値が含まれています。

### ワークフロー {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

次の図に示すように、プラットフォーム ID を使用したシングルサインオンを通じて認証フローを実装するには、指定された手順を実行します。

![Platform ID を使用したシングルサインオンを通じた認証決定の取得 ](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Platform ID を使用したシングルサインオンを通じた認証決定の取得*

1. **プラットフォーム ID の取得：** 2 つ目のストリーミングアプリケーションは、Adobe Pass システムの外部で ID サービスまたはライブラリを呼び出し、一意のプラットフォーム ID に関連付けられた `JWS` または `JWE` ペイロードを取得します。

1. **JWS または JWE として一意のプラットフォーム ID を返します：** 2 番目のストリーミングアプリケーションは、基本的なセキュリティ条件が満たされていることを確認するために応答データを検証します。
   * ペイロードが期限切れではありません。
   * ペイロードが署名済みまたは暗号化されている。

1. **認証決定の取得：** 2 番目のストリーミングアプリケーションは、決定の承認エンドポイントを呼び出して、特定のリソースの認証決定を取得するために必要なすべてのデータを収集します。

   >[!IMPORTANT]
   >
   > 次の項目について詳しくは、[ 特定の mvpd を使用した認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API ドキュメントを参照してください。
   >
   > * `serviceProvider`、`mvpd`、`resources` など、すべての _必須_ パラメーター
   > * `Authorization` や `AP-Device-Identifier` など、すべての _必須_ ヘッダー
   > * すべての _オプション_ パラメーターおよびヘッダー
   >
   > <br/>
   > 
   > ストリーミングアプリケーションは、リクエストを行う前に、一意のプラットフォーム識別子に有効な値が含まれていることを確認する必要があります。
   >
   > <br/>
   > 
   > `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe – 件名 – トークン ](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

1. **シングルサインオンプロファイルの検索：** Adobe Pass サーバーは、受信したパラメーターとヘッダーに基づいて有効なシングルサインオンプロファイルを識別します。

1. **リクエストされたリソースの MVPD 決定を取得：** Adobe Pass サーバーは MVPD 認証エンドポイントを呼び出して、ストリーミングアプリケーションから受信した特定のリソースに関する `Permit` または `Deny` の決定を取得します。

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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **メディアトークンでストリームを開始：** 2 番目のストリーミングアプリケーションは、メディアトークンを使用してコンテンツを再生します。

1. **詳細を含んだ決定 `Deny` 返す：** 決定の承認承認エンドポイント応答には、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った `Deny` 決定とエラーペイロードが含まれています。

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
   > 検証に失敗した場合は、エラー応答が生成され、[ 拡張エラーコード ](../../../enhanced-error-codes.md) ドキュメントに従った追加情報が提供されます。

1. **決定の詳細 `Deny` 処理：** 2 番目のストリーミングアプリケーションは、応答からのエラー情報を処理し、それを使用して、オプションで特定のメッセージをユーザーインターフェイスに表示できます。

>[!NOTE]
>
> 事前認証フローの手順は、認証フローの手順と同じです。ただし、使用されるエンドポイントが、「[ 特定の mvpd を使用した事前認証決定の取得 ](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) ドキュメントで説明されているエンドポイントである点が異なります。
