---
title: プログラマーのエンタイトルメントフロー
description: プログラマーのエンタイトルメントフロー
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 0%

---

# プログラマーのエンタイトルメントフロー {#prog-entitlement-flow}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

このドキュメントでは、プログラマーの観点からの基本的なエンタイトルメントフローについて説明します。  ここで説明する基本的な TVE 統合以外の機能と使用例については、 [プログラマーの使用例](/help/authentication/programmer-use-cases.md).

Adobe Pass認証は、両者に対して安全で一貫性のあるインターフェイスを提供することで、プログラマーと MVPD 間のエンタイトルメントフローを仲介します。  プログラマー側では、Adobe Pass Authentication は、次の 2 つの一般的なタイプのエンタイトルメントインターフェイスを提供します。

1. AccessEnabler - Web ページをレンダリングできるデバイス（Web アプリ、スマートフォン/タブレットアプリなど）上のアプリ用の API ライブラリを提供するクライアントコンポーネント。
2. クライアントレス API - Web ページをレンダリングできないデバイス（セットトップボックス、ゲームコンソール、スマートテレビなど）向けの RESTful Web サービス。 Web ページをレンダリングするための要件は、MVPD の Web サイトでユーザーが認証を受ける MVPD の要件に基づいています。

ここで示すプラットフォームに依存しない概要に加えて、クライアントレス API 固有の概要は、クライアントレス API ドキュメントに記載されています。 AccessEnabler は、サポート対象のプラットフォーム (Web 上では AS / JS、iOSでは Objective-C、Android では Java) でネイティブに動作します。 AccessEnabler API は、サポートされるプラットフォーム間で一貫しています。 AccessEnabler をサポートしていないすべてのプラットフォームは、同じクライアントレス API を使用します。

どちらのタイプのインターフェイスでも、Adobe Pass Authentication は、プログラマーのアプリとユーザーの MVPD の間のエンタイトルメントフローを安全に仲介します。

![](assets/prog-entitlement-flow.png)


*図： Adobe Pass Authentication Ecosystem*

>[!IMPORTANT]
>
>上の図では、Adobe Pass認証サーバーを経由しないエンタイトルメントフローの一部として、MVPD ログインが存在することに注意してください。 ユーザーは、MVPD のログインページにログオンする必要があります。 この要件のため、Web ページをレンダリングできないデバイスでは、プログラマーのアプリは、Web 対応デバイスに切り替えて MVPD でログインし、その後、エンタイトルメントフローの残りの部分に戻るように指示する必要があります。

## 権利付与フロー {#entitlement-flow}

基本的なエンタイトルメントフローを構成する次の 4 つの個別のサブフローがあります。

1. [起動フロー](/help/authentication/entitlement-flow.md#startup)
1. [認証フロー](/help/authentication/entitlement-flow.md#authentication)
1. [認証フロー](/help/authentication/entitlement-flow.md#authorization)
1. [ログアウトのフロー](/help/authentication/entitlement-flow.md#logout)

ユーザーがプログラマーのサイトに初めて訪問すると、エンタイトルメントフローは上記の順序で進みます。 ただし、以降の訪問では、認証および承認トークンの有効期限が切れているかどうか、または表示ポリシーによっては、ユーザーがサブフローの 1 つまたは 2 つしか経由しない場合があります。

### 起動フロー {#startup}

プログラマーとデバイスの ID を確立し、初期化タスクを実行します。 これは、以降のすべての使用権限の呼び出しの前提条件です。

**AccessEnabler**

* **`setRequestor()`** - AccessEnalber と、拡張機能によってAdobe Pass Authentication Server の識別を確立します。 この呼び出しは、残りのエンタイトルメントフローの前駆者です。 JavaScript の場合の例：

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

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`**  — プラットフォームによっては、アプリが再コードを呼び出す前に、前提条件のタスクを完了する必要がある場合があります。 詳しくは、 **クライアントレス API ドキュメント** 」を参照してください。 例えば、Xbox プラットフォームでは、regcode を呼び出す前に、所定のセキュリティ手順を完了する必要があります。

### 認証フロー {#authentication}

認証が成功すると、デバイスとリクエスト元に関連付けられた AuthN トークンが生成されます。 認証が成功すると、認証の前提条件となります。

**AccessEnabler**

* `checkAuthentication()`  — ローカルトークンのキャッシュに有効なキャッシュ済み認証トークンが存在するかどうかをチェックします。実際には完全な認証フローがトリガーされません。 このトリガーは、 `setAuthenticationStatus()` コールバック関数。
* `getAuthentication()`  — 完全な認証フローを開始します。 成功した場合、Adobe Pass Authentication は AuthN トークンを生成し、クライアントにキャッシュします。 ユーザーは、選択した MVPD サイトにログインします。このサイトは、プラットフォームに応じて iFrame、ポップアップウィンドウ、Web ビューのいずれかで表示されます。 このトリガーは、displayProviderDialog() です。

**クライアントレス API**

* `<FQDN>/.../checkauthn`  — の Web サービスのバージョン。 `checkAuthentication()` 上記の
* `<FQDN>/.../config` - 2nd-screen アプリに MVPD のリストを返します。
* `<FQDN>/.../authenticate`  — 第 2 画面アプリから認証フローを開始し、選択した MVPD にユーザーをリダイレクトしてログインします。 成功した場合、Adobe Pass認証は AuthN トークンを生成してサーバーに保存し、ユーザーが元のデバイスに戻って使用権限フローを完了します。

次の 2 つの点が真の場合、AuthN トークンは有効と見なされます。

* AuthN トークンが期限切れではありません
* AuthN トークンに関連付けられている MVPD は、現在の要求元 ID に対して許可されている MVPD のリストにあります

#### 汎用 AccessEnabler 初期認証ワークフロー {#generic-ae-initial-authn-flow}

1. アプリが、 `getAuthentication()`：有効なキャッシュ済み認証トークンを確認します。 このメソッドにはオプションの `redirectURL` パラメータ。 `redirectURL`認証が成功すると、ユーザーは認証の初期化に使用した URL に戻ります。
1. AccessEnabler は、現在の認証状態を決定します。 ユーザーが現在認証されている場合、AccessEnabler は、 `setAuthenticationStatus()` コールバック関数で、成功を示す認証ステータスを渡す。
1. ユーザーが認証されていない場合、AccessEnabler は、特定の MVPD でユーザーの最後の認証試行が成功したかどうかを判断することで、認証フローを継続します。 MVPD ID がキャッシュされ、 `canAuthenticate` フラグが true の場合、またはを使用して MVPD が選択された場合 `setSelectedProvider()`を指定した場合、MVPD 選択ダイアログが表示されることはありません。 認証フローは、MVPD のキャッシュされた値（最後の正常な認証時に使用されたのと同じ MVPD）を引き続き使用します。 バックエンドサーバーに対してネットワーク呼び出しがおこなわれ、ユーザーは MVPD ログインページにリダイレクトされます。

1. MVPD ID がキャッシュされておらず、を使用して MVPD が選択されていない場合 `setSelectedProvider()` または `canAuthenticate` フラグが false に設定されている場合、 `displayProviderDialog()` コールバックが呼び出されます。 このコールバックは、選択する MVPD のリストをユーザーに表示する UI を作成するようにアプリに指示します。 MVPD オブジェクトの配列が提供され、MVPD セレクターを構築するために必要な情報が含まれます。 各 MVPD オブジェクトは、MVPD エンティティを表し、MVPD の ID や MVPD ロゴが見つかる URL などの情報を含みます。

1. MVPD を選択したら、ユーザーが選択した内容を AccessEnabler に通知する必要があります。 非Flashクライアントの場合、ユーザが目的の MVPD を選択したら、AccessEnabler に対し、 `setSelectedProvider()` メソッド。 Flashクライアントが代わりに共有をディスパッチする `MVPDEvent` タイプ&quot;`mvpdSelection`&quot;、選択したプロバイダーを渡します。

1. AccessEnabler がユーザの MVPD 選択を通知すると、バックエンドサーバに対してネットワーク呼び出しが行われ、ユーザは MVPD ログインページにリダイレクトされます。

1. 認証ワークフロー内で、AccessEnabler はAdobe Pass Authentication および選択した MVPD と通信し、ユーザーの資格情報（ユーザー ID とパスワード）を要求し、その ID を検証します。 ログイン用に独自のサイトにリダイレクトされる MVPD もありますが、iFrame 内にログインページを表示する必要がある MVPD もあります。 顧客がこれらの MVPD の 1 つを選択した場合に備えて、iFrame を作成するコールバックをページに含める必要があります。<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. ユーザーが正常にログインすると、AccessEnabler は認証トークンを取得し、認証フローが完了したことをアプリに通知します。 AccessEnabler が `setAuthenticationStatus()` ステータスコードが 1 のコールバック。成功を示します。 これらの手順の実行中にエラーが発生した場合、 `setAuthenticationStatus()` コールバックは、ステータスコード 0 でトリガーされ、認証の失敗を示し、対応するエラーコードも示します。

>[!IMPORTANT]
>Comcast は、現時点ではロゴの静的 URL を提供しない MVPD のみです。 プログラマーは、最新のロゴをから引き出す必要があります。 [XFINITY 開発者のポータル](https://developers.xfinity.com/products/tv-everywhere).
>

### 認証フロー {#authorization}

認証は、保護されたコンテンツを表示するための前提条件です。 正常な認証により、AuthZ トークンと、セキュリティ上の理由からプログラマーのアプリに提供される短時間のみ有効なメディアトークンが生成されます。 承認ワークフローをサポートするには、必要なリクエスト元の設定を事前に実行し、 [メディアトークン検証ツール](/help/authentication/media-token-verifier-int.md). これらが完了したら、認証を開始できます。

保護されたリソースへのアクセスをユーザーがリクエストすると、アプリが認証を開始します。 リクエストされたリソース（例えば、チャネル、エピソードなど）を指定するリソース ID を渡します。 アプリは、まず、保存された認証トークンを確認します。 見つからない場合は、認証プロセスを開始します。

**AccessEnabler**

* `checkAuthorization()`  — 完全な認証フローを開始せずに、認証をチェックします。 これは、多くの場合、プログラマーアプリの UI に表示されるステータス情報を更新するために使用されます。

* `getAuthorization()`  — 完全な認証フローを開始します。

認証呼び出しの結果を処理するために、次のコールバック関数を提供します。

* `setToken()`  — 以前に認証が成功し、認証が成功した場合は、AccessEnabler が `setToken()` コールバック関数に渡され、短時間のみ有効なメディアトークンが渡され、Adobe Pass認証の資格付与フローの成功した結果が示されます。 ( ユーザーに保護されたコンテンツの表示を許可する前に、プログラマーのアプリは、メディアトークン検証ツールを使用してメディアトークンの有効性を確認します。

* `tokenRequestFailed()`  — 要求されたリソースに対してユーザーが許可されていない場合（またはクエリが他の理由で失敗した場合）、AccessEnabler はこのコールバック関数（および独自のエラー報告関数）を呼び出し、失敗の詳細を渡します。

**クライアントレス API**

* `\<FQDN\>/.../authorize`  — 完全な認証フローを開始します。

#### 汎用 AccessEnabler 認証ワークフロー {#generic-ae-authr-wf}

1. 割り当てられたプログラマ GUID を Access Enabler に登録するコールバック関数を指定します。 `setReqestor()`. このコールバック関数は、AccessEnabler が正常にダウンロードされたときに呼び出されます。

1. 通話 `getAuthorization()` ユーザーが保護されたリソースへのアクセスを要求したとき。 使用 `getAuthorization()`を呼び出す場合は、リクエストされたリソース（例えば、チャネル、エピソードなど）を指定するリソース ID を渡します。 AccessEnabler は、認証リクエストと共に渡すキャッシュされた認証トークンを探します。 見つからない場合は、認証フローを開始します。
1. 認証の結果を処理するコールバック関数を提供します。

   * `setToken()`  — 認証が成功した場合、またはユーザーが以前に認証を受けている場合、Access Enabler は、 `setToken()` コールバック関数に短時間のみ有効な認証トークンを渡す。

   * `tokenRequestFailed()`  — 要求されたリソースに対してユーザーが許可されていない（またはクエリが他の理由で失敗した）場合、AccessEnabler は、登録したエラー報告関数を呼び出します。 `tokenRequestFailed()` コールバック。失敗に関する詳細を渡します。

### ログアウトフロー {#logout}

現在のユーザーのエンタイトルメントフローに関連付けられたトークンやその他のデータをクリアします。

**AccessEnabler**

* `logout()`

**クライアントレス API**

* `\<FQDN\>/.../logout`

## AccessEnabler の動作の理解 {#ae-behavior}

AccessEnabler API 呼び出しはすべて非同期です（ただし、API リファレンスに記載されている例外は 1 つです）。 API は任意の回数呼び出すことができますが、呼び出しによってトリガーされるアクションが呼び出しがおこなわれたのと同じ順序で完了するという強力な保証はありません。 ( ただし、現在のFlash Playerランタイムは例外です。マルチスレッドではないので、呼び出しを確実におこないます。 *do* 呼び出された順に完了します )。

応答を区別し、応答を呼び出しとペアリングできるように、すべてのコールバックは入力パラメータをエコーバックします。 これには以下が含まれます。 `setToken()` および`tokenRequestFailed()`( 最終的には、 `checkAuthorization()`. ( の `checkAuthorization()` コールバックを呼び出すと、使用したリソースがエコーバックされます )。 この機能を利用すると、どの応答がどの呼び出しに対応しているかを区別できます。 この機能を使用するには、次のようなコードを記述します。

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

### AEM の動作に関する FAQ {#ae-beh-faq}

**質問。 最初の呼び出しが終了する前に 2 回目の AccessEnabler 呼び出しを実行するとどうなりますか。**

2 番目の呼び出しが実行されると（非同期通信）、最初の呼び出しが引き続き実行されます。

**質問。 AccessEnabler がサポートする同時呼び出しの数は最大ですか。**

AccessEnabler コードには制限が明示的に設定されていないので、使用可能なシステムリソースと MVPD の容量によってのみ制限されます。

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
