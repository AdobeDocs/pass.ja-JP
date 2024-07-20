---
title: プログラマーの使用権限フロー
description: プログラマーの使用権限フロー
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# プログラマーの使用権限フロー {#prog-entitlement-flow}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

このドキュメントでは、プログラマーの観点からの基本的な使用権限のフローを説明します。  ここで説明する基本的な TVE 統合を超える機能とユースケースについては、[ プログラマーのユースケース ](/help/authentication/programmer-use-cases.md) を参照してください。

Adobe Pass認証は、プログラマーと MVPD の間の使用権限フローを、両方の関係者に安全で一貫性のあるインターフェイスを提供することで仲介します。  プログラマー側では、Adobe Pass認証には、2 つの一般的なタイプの使用権限インターフェイスが用意されています。

1. AccessEnabler - Web ページをレンダリングできるデバイス（Web アプリ、スマートフォン/タブレットアプリなど）上のアプリに対して API のライブラリを提供するクライアントコンポーネント。
2. クライアントレス API - Web ページをレンダリングできないデバイス（セットトップボックス、ゲーム機、スマート TV など）用の RESTful Web サービス。 Web ページのレンダリング要件は、MVPD の web サイトでユーザーが認証を行うという MVPD の要件に基づいています。

ここに示すプラットフォームに依存しない概要に加えて、ここにクライアントレス API 固有の概要があります。クライアントレス API ドキュメント。 AccessEnabler は、サポートされているプラットフォーム（Web では AS/JS、iOSでは Objective-C、Androidでは Java）でネイティブに実行されます。 AccessEnabler API は、サポートされているプラットフォーム間で一貫性があります。 AccessEnabler をサポートしていないプラットフォームでは、同じ Clientless API を使用します。

どちらのタイプのインターフェイスでも、Adobe Pass認証は、プログラマーのアプリとユーザーの MVPD の間のエンタイトルメントフローを安全に仲介します。

![](assets/prog-entitlement-flow.png)


*図：Adobe Pass認証エコシステム*

>[!IMPORTANT]
>
>上の図では、Adobe Pass認証サーバーを経由しない使用権限フローの一部として MVPD ログインがあることに注意してください。 ユーザーは、MVPD のログインページにログオンする必要があります。 この要件により、Web ページをレンダリングできないデバイスでは、プログラマーのアプリは、MVPD を使用してログインするために Web 対応デバイスに切り替えるようにユーザーに指示する必要があります。その後、ユーザーは元のデバイスに戻り、残りの使用権フローを確保します。

## 使用権限フロー {#entitlement-flow}

基本権限を構成する 4 つの異なるサブフローがあります
フロー：

1. [起動フロー](/help/authentication/entitlement-flow.md#startup)
1. [認証フロー](/help/authentication/entitlement-flow.md#authentication)
1. [認証フロー](/help/authentication/entitlement-flow.md#authorization)
1. [ログアウト](/help/authentication/entitlement-flow.md#logout)

ユーザーがプログラマーのサイトに初めて訪問すると、使用権限フローが上記の順序で続行されます。 ただし、その後の訪問では、認証トークンと承認トークンの有効期限が切れているかどうかに応じて、または表示ポリシーに応じて、ユーザーは 1 つまたは 2 つのサブフローのみを通過する場合があります。

### 起動フロー {#startup}

プログラマとデバイスの ID を確立し、初期化タスクを実行します。 これは、以降のすべての資格コールの前提条件です。

**AccessEnabler**

* **`setRequestor()`** - AccessEnalber および拡張によってAdobe Pass Authentication Server との ID を確立します。 この呼び出しは、残りの使用権限フローの前段階です。 例えば、JavaScriptでは次のようになります。

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**クライアントレス API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - プラットフォームによっては、アプリが regcode を呼び出す前に完了する必要があるタスクが存在する場合があります。 詳しくは、**クライアントレス API ドキュメント** を参照してください。 例えば、Xbox プラットフォームでは、regcode を呼び出す前に所定のセキュリティ手順を完了する必要があります。

### 認証フロー {#authentication}

認証に成功すると、デバイスとリクエスターに関連付けられた AuthN トークンが生成されます。 認証に成功することは、認証を行うための前提条件です。

**AccessEnabler**

* `checkAuthentication()` – 実際に完全な認証フローをトリガーせずに、有効なキャッシュ済み認証トークンがローカルトークンキャッシュに存在するかどうかを確認します。 これにより、`setAuthenticationStatus()` コールバック関数がトリガーされます。
* `getAuthentication()` – 完全な認証フローを開始します。 成功した場合、Adobe Pass認証は AuthN トークンを生成し、クライアントにキャッシュします。 ユーザーは、プラットフォームに応じて、iFrame、ポップアップウィンドウ、または Web ビューで表示される、選択した MVPD サイトにログインします。 displayProviderDialog （）をトリガーにします。

**クライアントレス API**

* `<FQDN>/.../checkauthn` – 上記 `checkAuthentication()` の Web サービスバージョン。
* `<FQDN>/.../config` - 2 番目の画面のアプリに MVPD のリストを返します。
* `<FQDN>/.../authenticate` - 2 番目の画面のアプリから認証フローを開始し、ログイン用に選択した MVPD にユーザーをリダイレクトします。 リクエストが成功した場合、Adobe Pass認証によって AuthN トークンが生成されてサーバーに保存され、ユーザーは元のデバイスに戻って使用権フローを完了します。

AuthN トークンは、次の 2 つのポイントが true の場合に有効と見なされます。

* AuthN トークンが期限切れではありません
* AuthN トークンに関連付けられている MVPD が、現在の要求者 ID で許可されている MVPD の一覧に含まれています

#### Generic AccessEnabler 初期認証ワークフロー {#generic-ae-initial-authn-flow}

1. アプリは、`getAuthentication()` への呼び出しで認証ワークフローを開始し、有効なキャッシュされた認証トークンを確認します。 このメソッドにはオプションの `redirectURL` パラメーターがあります。`redirectURL` の値を指定しない場合、認証が成功すると、認証が初期化された URL にユーザーが返されます。
1. AccessEnabler は、現在の認証ステータスを決定します。 ユーザーが現在認証されている場合、AccessEnabler は `setAuthenticationStatus()` コールバック関数を呼び出し、成功を示す認証ステータスを渡します。
1. ユーザーが認証されていない場合、AccessEnabler は、ユーザーの最後の認証試行が特定の MVPD で成功したかどうかを確認することにより、認証フローを続行します。 MVPD ID がキャッシュされていて、`canAuthenticate` フラグが true であるか、または `setSelectedProvider()` を使用して MVPD が選択されている場合、MVPD 選択ダイアログが表示されません。 認証フローは、MVPD のキャッシュされた値（最後に認証が成功したときに使用されたのと同じ MVPD）を使用して続行されます。 バックエンドサーバーに対してネットワーク呼び出しが実行され、ユーザーは MVPD ログインページにリダイレクトされます。

1. MVPD ID がキャッシュされておらず、MVPD が `setSelectedProvider()` を使用して選択されていない場合、または `canAuthenticate` フラグが false に設定されている場合、`displayProviderDialog()` コールバックが呼び出されます。 このコールバックは、選択する MVPD のリストをユーザーに提示する UI を作成するようにアプリに指示します。 MVPD セレクターを構築するために必要な情報を含む、MVPD オブジェクトの配列が提供されます。 各 MVPD オブジェクトは、MVPD エンティティを記述し、MVPD の ID や MVPD ロゴが見つかる URL などの情報を含みます。

1. MVPD が選択されたら、アプリケーションはユーザーが選択した AccessEnabler に通知する必要があります。 Flash以外のクライアントの場合、ユーザーが目的の MVPD を選択すると、`setSelectedProvider()` メソッドを呼び出して AccessEnabler に通知します。 代わりに、Flashクライアントは「`mvpdSelection`」タイプの共有 `MVPDEvent` をディスパッチし、選択したプロバイダーを渡します。

1. AccessEnabler がユーザーの MVPD 選択を通知されると、バックエンド・サーバにネットワーク・コールが行われ、ユーザーは MVPD ログイン・ページにリダイレクトされます。

1. 認証ワークフロー内で、AccessEnabler はAdobe Pass Authentication および選択された MVPD と通信し、ユーザーの認証情報（ユーザー ID とパスワード）を要求し、ユーザーの ID を確認します。 MVPD の中には、ログインのために自分のサイトにリダイレクトするものもあれば、iFrame 内で自分のログインページを表示するように要求するものもあります。 顧客が MVPD.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)--> のいずれかを選択する場合は、ページに iFrame を作成するコールバックを含める必要があります。

1. ユーザーが正常にログインすると、AccessEnabler は認証トークンを取得し、認証フローが完了したことをアプリケーションに通知します。 AccessEnabler は、ステータス コードが 1 の `setAuthenticationStatus()` コールバックを呼び出し、成功を示します。 これらの手順の実行中にエラーが発生した場合、`setAuthenticationStatus()` コールバックは、認証失敗を示すステータスコード 0 と対応するエラーコードでトリガーされます。

>[!IMPORTANT]
>現時点で、ロゴの静的 URL を提供しない MVPD は Comcast のみです。 プログラマーは、[XFINITY 開発者ポータル ](https://developers.xfinity.com/products/tv-everywhere) から最新のロゴを取り込む必要があります。
>

### 認証フロー {#authorization}

承認は、保護されたコンテンツを表示するための前提条件です。 認証が成功すると、AuthZ トークンが、セキュリティ目的でプログラマーのアプリに提供される短期間有効のメディアトークンと共に生成されます。 承認ワークフローをサポートするには、必要なリクエスター設定を事前に実行し、[ メディアトークンベリファイア ](/help/authentication/media-token-verifier-int.md) を統合しておく必要があります。 これらを完了したら、認証を開始できます。

保護されたリソースへのアクセスをユーザーがリクエストすると、アプリが認証を開始します。 リクエストされたリソース（チャネル、エピソードなど）を指定するリソース ID を渡します。 アプリでは最初に、保存された認証トークンを確認します。 見つからない場合は、認証プロセスを開始します。

**AccessEnabler**

* `checkAuthorization()` – 完全な認証フローを開始せずに認証を確認します。 これは、プログラマーアプリの UI に表示されるステータス情報を更新する場合によく使用されます。

* `getAuthorization()` – 完全な認証フローを開始します。

次のコールバック関数を使用して、の結果を処理できます。
認証呼び出しは、次のようになります。

* `setToken()`：以前に認証が成功し、認証が成功した場合、AccessEnabler は `setToken()` コールバック関数を呼び出し、短時間のみ有効なメディア・トークンを渡して、Adobe Pass認証資格フローが正常に終了したことを示します。 （ユーザーが保護されたコンテンツを表示できるようにする前に、プログラマーのアプリはメディアトークン検証子を使用してメディアトークンの有効性を確認します。

* `tokenRequestFailed()` - ユーザーが要求されたリソースに対して承認されていない場合（または他の理由でクエリが失敗した場合）、AccessEnabler はこのコールバック関数（および独自のエラー報告関数）を呼び出し、失敗の詳細を渡します。

**クライアントレス API**

* `\<FQDN\>/.../authorize` – 完全な認証フローを開始します。

#### Generic AccessEnabler 認証ワークフロー {#generic-ae-authr-wf}

1. `setReqestor()` を使用して、割り当てられたプログラマ GUID をアクセス イネーブラに登録するコールバック関数を指定します。 このコールバック関数は、AccessEnabler が
正常にダウンロードされました。

1. 保護されたリソースへのアクセスをユーザーがリクエストした場合は、`getAuthorization()` を呼び出します。 `getAuthorization()` を使用して、リクエストされたリソース（チャネル、エピソードなど）を指定するリソース ID を渡します。 AccessEnabler は、認証リクエストで渡すキャッシュされた認証トークンを探します。 見つからない場合は、認証フローが開始されます。
1. 認証結果を処理するコールバック関数を指定します。

   * `setToken()`：認証に成功した場合、またはユーザーが以前に認証されている場合、アクセス イネーブラは `setToken()` コールバック関数を呼び出して短期間有効な認証トークンを渡すことにより、認証プロセスを続行します。

   * `tokenRequestFailed()` - ユーザーが要求されたリソースに対して許可されていない場合（または他の理由でクエリが失敗した場合）、AccessEnabler は、登録したエラー報告関数に加えて `tokenRequestFailed()` コールバックを呼び出し、失敗の詳細を渡します。

### ログアウトフロー {#logout}

現在のユーザーの使用権限フローに関連付けられたトークンと他のデータをクリアします。

**AccessEnabler**

* `logout()`

**クライアントレス API**

* `\<FQDN\>/.../logout`

## AccessEnabler の動作について {#ae-behavior}

すべての AccessEnabler API 呼び出しは非同期です（ただし、API リファレンスに記載されている例外があります）。 API は任意の回数呼び出すことができますが、アクションがトリガーされたという強力な保証はありません
呼び出しによって、呼び出しが行われたのと同じ順序で完了します。 （ただし、現在のFlash Playerランタイムは例外です。マルチスレッドでない場合は、呼び出しが *実行* 順に完了します
呼び出されます）。

応答を区別し、応答と呼び出しをペアにできるように、すべてのコールバックで入力パラメーターがエコーバックされます。 これには、`checkAuthorization()` によって最終的にトリガーされる `setToken()` および `tokenRequestFailed()` が含まれます。 （`checkAuthorization()` コールバックの場合、使用されたリソースがエコーバックされます。） この機能を利用すると、どの応答がどの呼び出しに対応するかを区別できます。 この機能を使用するには、次のようなコードを作成します。

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### AE の動作に関する FAQ {#ae-beh-faq}

**質問。 最初の呼び出しが終了する前に 2 回目の AccessEnabler 呼び出しを行うと、どうなりますか。**

2 回目の呼び出し（非同期通信）の実行に応じて、最初の呼び出しは引き続き実行されます。

**質問。 AccessEnabler がサポートできる同時呼び出しの最大数はありますか。**

AccessEnabler コードでは制限は明示的に設定されていないため、使用可能なシステム リソースと MVPD の容量によってのみ制限されます。

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
