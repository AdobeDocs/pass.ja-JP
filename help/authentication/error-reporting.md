---
title: エラーレポート
description: エラーレポート
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2961'
ht-degree: 1%

---

# エラーレポート {#error-reporting}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


## 概要 {#overview}

Adobe Pass認証のエラーレポートは、現在、次の 2 つの方法で実装されています。

* **高度なエラーレポート** 実装者は、 [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) または、「 」という名前の Interface メソッドを実装します。`status`」 [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) および [AccessEnabler Android SDK](#accessenabler-android-sdk)を使用して、高度なエラーレポートを受け取ることができます。 エラーは次の分類に分類されます。 **情報**, **警告**、および **エラー** タイプ。 このレポートシステムは、 **非同期**&#x200B;で、 **複数のエラーがトリガーされる順序は保証されません**.  高度なエラー報告システムの詳細については、 [高度なエラーレポート](#advanced-error-reporting) 」セクションに入力します。

* **オリジナルエラーレポート —** 特定のリクエストが失敗した場合に、エラーメッセージが特定のコールバック関数に渡される静的レポートシステム。 エラーは、汎用、認証、および認証の各タイプに分類されます。 元のシステムで報告されたエラーのリストについては、 [元のエラーレポート](#original-error-reporting) 」セクションに入力します。


## 高度なエラーレポート {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>古い [元のエラーレポート](#original-error-reporting) API は引き続き以前と同様に機能し、高度なエラーレポートでは機能が壊れませんが、元のエラーレポートは更新を受け取らなくなります。 すべての新しいエラーおよび更新は、高度なエラーレポートシステムで発生します。

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

新しいエラーレポートシステムはオプションのままなので、実装者はエラーハンドラーコールバックを明示的に登録して、高度なエラーレポートを受け取ることができます。 システムには、複数のエラーコールバックを動的に登録および登録解除する機能が含まれます。 また、AccessEnabler JavaScript SDK の読み込み後すぐに新しいエラーコールバックを登録できます。他の初期化を実行する必要はありません（を呼び出す前に）。 `setRequestor()`) を使用することで、初期化エラーと設定エラーに関する高度なレポートを受け取ることができます。


#### 実装 {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


エラーハンドラーのコールバック関数は、次の構造を持つ 1 つのオブジェクト（マップ）を受け取ります。

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1.バインド {#bind}

**`.bind(eventType:String, handlerName:String):void`**

イベントのハンドラをアタッチします。

**`eventType`** - 「`errorEvent`「 」の値を指定すると、AccessEnabler JavaScript SDK が高度なエラーレポートのコールバックをトリガーします。

**`handlerName`** - errors ハンドラ関数の名前を指定する文字列。


両方のバインドパラメータは、次のセットの文字のみを使用する必要があります。 `[0-9a-zA-Z][-._a-zA-Z0-9]`つまり、パラメーターは数字または文字で始まる必要があり、ハイフン、ピリオド、アンダースコア、英数字のみを含めることができます。  また、パラメーターは 1024 文字を超えることはできません。

**例** バインディングエラーハンドラーの数：

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

技術的な制限により、クロージャや匿名関数をバインドすることはできません。 2 番目のパラメーターでメソッドの名前を指定する必要があります。


### 2.バインド解除 {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

以前にアタッチされたイベントハンドラを削除します。

**`eventType`** - &#39;のみ`errorEvent`&#39;の値を指定すると、AccessEnabler JavaScript SDK が高度なエラーレポートコールバックをトリガーします。

**`handlerName`**  — が null の場合、または指定した `eventType` が削除されます。

両方のバインドパラメータは、次のセットの文字のみを使用する必要があります。 `[0-9a-zA-Z][-._a-zA-Z0-9]`つまり、パラメーターは数字または文字で始まる必要があり、ハイフン、ピリオド、アンダースコア、英数字のみを含めることができます。  また、パラメーターは 1024 文字を超えることはできません。

**例** 単一のエラーハンドラーを削除する方法：

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**例** すべてのエラーハンドラーを削除しています：

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

新しいエラー報告システムは必須なので、実装者は新しい Objective C &quot;EntitlementStatus&quot;プロトコルに明示的に準拠する必要があります。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 実装 {#accessenab-ios-tvossdk-imp}

実装者は、次の条件に従う必要があります **EntitlementStatus** プロトコル：

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

お使いの **ステータス** 関数は、単一のオブジェクト ( `NSDictionary`) を次の構造で置き換えます。

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. 宣言**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. 実装**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### AccessEnabler Android SDK {#accessenabler-android-sdk}

実装者は明示的に `IAccessEnablerDelegate` インターフェイスで定義されたプロトコル。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 実装 {#access-enablr-androidsdk-imp}

実装者は、新しい `status` インターフェイスからのメソッド`IAccessEnablerDelegate`. The **`status`** 関数は、 **`AdvancedStatus`** 次のモデルを持つオブジェクト：

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**サンプル**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### AccessEnabler FireOS SDK {#accessenabler-fireos-sdk}


実装者は明示的に `IAccessEnablerDelegate` インターフェイスで定義されたプロトコル。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 実装 {#access-enab-fireos-sdk-}

実装者は、新しい `status`インターフェイスからのメソッド`IAccessEnablerDelegate`. The **`status`** 関数は、 **`AdvancedStatus`** 次のモデルを持つオブジェクト：

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**サンプル**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## 高度なエラーコードリファレンス {#advanced-error-codes-reference}

次の表に、新しいエラー API によって公開されるエラーコードと、それらを修正するために推奨されるアクションを示します。

| ID | レベル | 説明 | 開発者アクション | ユーザーアクション | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp;AAPL_ERROR | エラー | 汎用Apple SSO エラー | エラーには、元の VSA エラーを含む詳細フィールドが含まれます。 | 該当なし | 該当なし | はい | 該当なし |
| VSA203 | 情報 | プラットフォーム SSO を介したログインの結果、認証が発生した間に、ユーザーはアプリケーションからログアウトすることにしました。 | tvOS の設定/アカウント/TV Provider から明示的にログアウトするようにユーザーに指示またはプロンプトを表示します。 <br><br> iOS/iPadOS の設定/TV Provider から明示的にサインアウトするようにユーザーに指示/プロンプトを表示します。 | tvOS の [ 設定 ] -> [ アカウント ] -> [ テレビプロバイダ ] から明示的にサインアウトします。 <br> <br> iOS/iPadOS の設定/TV Provider から明示的にログアウトする | 該当なし | はい | 該当なし |
| VSA404 | 情報 | アプリケーションビデオサブスクライバーのアカウント権限が未確定です。 | シングルサインオン (SSO) のユーザーエクスペリエンスの利点を説明することで、サブスクリプション情報へのアクセス権の付与を拒否するユーザーに対してインセンティブ化をおこないます。 | ユーザーは、アプリケーション設定（TV Provider アクセス）、またはiOS/iPadOS の設定/TV Provider、または設定/アカウント/tvOS の TV Provider からセクションに移動して、決定を変更できます。 | 該当なし | はい | 該当なし |
| VSA503 | 情報 | アプリケーションビデオサブスクライバーアカウントメタデータリクエストが失敗しました。 | MVPD エンドポイントが応答していません。 アプリケーションは、通常の認証フローにフォールバックする可能性があります。 | 該当なし | 該当なし | はい | 該当なし |
| 500 | エラー | 内部エラー | AccessEnablerDebug を使用し、何が問題になったかを判断するためにデバッグログ（console.log 出力）を調べます。 | 該当なし | はい | はい | 該当なし |
| SEC403 | エラー | ドメインセキュリティエラー。 要求元が無効なドメインを使用しています。 特定の要求者 ID で使用されるすべてのドメインは、Adobeでホワイトリストに登録する必要があります。 | - AccessEnabler を読み込むのは、許可されたドメインのリストからのみ <br> <br>  — 使用する要求者 ID のドメインホワイトリストを管理するためにAdobeに連絡してください <br> <br> - iOS：正しい証明書を使用していること、および署名が正しく作成されていることを確認します | 該当なし | 該当なし | はい | 該当なし |
| SEC412 | 警告 | [リリース 2.5 で利用可能] デバイス ID が一致しません。 この問題は、基になるプラットフォームがデバイス ID を変更した場合に発生する可能性があります。 この場合、既存のトークンはクリアされ、ユーザーは認証されなくなります。 これは、ユーザーが JS SDK を使用していて、ローミング中の場合に有効に発生することに注意してください（JS では、クライアント IP はデバイス ID の一部です）。 そうでないと、不正の試みを示す可能性があり、別のデバイスからトークンをコピーしようとする試みです。 |  — 警告数を監視します。 不正な試行の指標となる明らかな理由（最近のブラウザーの更新や新しいオペレーティングシステムの更新はなし）でスパイクが発生した場合。  <br> <br> — オプションで、ユーザーに再度ログインする必要があることを通知します。 | 再度ログインします。 | はい | はい | はい（3.2 以降） |
| SEC420 | エラー | Adobe Pass Authentication Server との通信時の HTTP セキュリティエラー。 このエラーは、通常、スプーフィング/プロキシが配置されている場合に発生します。 |  — 読み込み `[https://]{SP_FQDN\}` ブラウザーで、SSL 証明書を手動で受け入れる（例： ） **https://api.auth.adobe.com** または **https://api.auth-staging.adobe.com** <br> <br> — プロキシ証明書を信頼済みとしてマーク | これが通常のユーザーに対して起こる場合は、中間者攻撃の可能性を示しています。 | はい | はい | はい（3.2 以降） |
| CFG100 | 警告 | クライアントマシンの日付/時刻/タイムゾーンが正しく設定されていません。 認証/認証エラーが発生する可能性があります。 |  — 正しい時刻を設定するようにユーザーに通知します。 <br> <br> エンタイトルメントフローが失敗する可能性が高いので、エンタイトルメントフローを防ぐためのアクションを実行します。 | 正しい日付/時刻を設定します。 | はい | はい | はい（3.2 以降） |
| CFG400 | エラー | 無効な要求元 ID が提供されました。 | 開発者は有効な要求者 ID を指定する必要があります。 | 該当なし | はい | はい | はい（3.2 以降） |
| CFG404 | エラー | Adobe Pass認証サーバーが見つかりませんでした。 これは、次の 3 つのインスタンスで発生する可能性があります。 <br><br>  — 開発者は無効なスプーフィングを適用しています。 <br><br>  — ユーザーはネットワークに問題が発生し、Adobe Pass認証ドメインに到達できません。 <br><br> - Adobe Pass認証サーバーの設定が誤っています。 <br><br>  **注意：** Firefox では、CFG404（ブラウザーの制限）の代わりに CFG400 が表示されます |  — スプーフィングをチェックします。 <br><br>  — ネットワーク/DNS 設定を確認します。 <br><br>  — 通知Adobe。 | ネットワーク/DNS 設定を確認します。 | はい | はい | はい（3.2 以降） |
| CFG410 | エラー | AccessEnabler が古すぎます。 | キャッシュをクリアするようにユーザーに通知します。 | ブラウザーのキャッシュをクリアします。 | はい | 該当なし | はい（3.2 以降） |
| CFG5xx | エラー | Adobe Pass認証サーバーで内部エラーが発生しています。 xx には任意の数値を指定できます。 | - Adobe Pass Authentication が使用できなくなったことをユーザーに通知します。 <br><br> - Adobe Pass認証をバイパスします。 <br> <br>  — 通知Adobe | 後でお試しください。 | はい | はい | はい（3.2 以降） |
| N000 | 情報 | ユーザーが認証されていません。 | 該当なし | ログイン。 | はい | はい | はい（3.2 以降） |
| N001 | 情報 | パッシブ認証の試行がバックグラウンドで開始されました。 これは、「要求元ごとの認証」で設定された MVPD に対して発生します。 ユーザーが自動的に認証されると考えられますが、初期化時にパフォーマンスが低下する可能性があります。 | 必要に応じて、「作業中」という警告をユーザーに通知する UI をユーザーに表示するか、または表示します。 | 待て。 | はい | はい | はい（3.2 以降） |
| N003 | 情報 | ユーザーは、Apple MVPD ピッカーから「その他の TV プロバイダー」オプションを選択します。 | The *displayProviderDialog* コールバックが呼び出され、アプリケーションは通常の認証フローにフォールバックできます。 | 通常の MVPD を選択し、ログイン画面を表示します。 | 該当なし | はい | 該当なし |
| N004 | 情報 | 現在の要求元でサポートされていない TV プロバイダを選択します。 | The *displayProviderDialog* コールバックが呼び出され、アプリケーションは通常の認証フローにフォールバックできます。 | 通常の MVPD を選択し、ログイン画面を表示します。 | 該当なし | はい | 該当なし |
| N005 | 情報 | MVPD ピッカーがキャンセルされました。 | 該当なし | 該当なし | はい | はい | はい（3.2 以降） |
| N010 | 警告 | 選択した MVPD に対して authenticate-all devaldation ルールが適用されている間に、ユーザーが認証されました。 | オプションで、MVPD の問題が原因で「無料」のアクセスを受け取っていることをユーザーに通知します。 | 該当なし | はい | はい | はい（3.2 以降） |
| N011 | 情報 | ユーザーは TempPass を使用して認証されました。 |  — ユーザーに通知します。 <br> <br>  — 通常の MVPD のリストを任意で提示します。 | オプションで、通常の MVPD を使用してログインします。 | はい | はい | はい（3.2 以降） |
| N111 | 警告 | 期限切れの TempPass。 |  — ユーザーに通知します。 <br> <br>  — 通常の MVPD のリストを表示します。 <br> <br> - TempPass オプションを非表示にします。 | 通常の MVPD でログインします。 | はい | はい | はい（3.2 以降） |
| N130 | エラー | **認証トークンがセッションで見つかりませんでした。**  これは、次のいずれかが原因である可能性があります。 <br> <br> 1. ブラウザーに（サードパーティの）cookie が無効になっている（AccessEnabler JavaScript SDK バージョン 4.x には適用されない） <br> <br> 2. ブラウザーでクロスサイトトラッキングを防ぐ機能が有効になっている（Safari 11 以降） <br> <br> 3. セッションが期限切れです <br> <br> 4. プログラマーが誤った連続で認証 API を呼び出す <br> <br> 注意：このエラーコードは、フルページのリダイレクト認証フローでは使用できません。 | 1.ユーザーに対し、（サードパーティの）cookie の有効化を促すメッセージを表示する <br> <br> 2. クロスサイトトラッキングを無効にするようユーザーに促す <br> <br> 3. ユーザーに再認証を求める <br> <br> 4. 正しい順序で API を呼び出す | 1. （サードパーティ）Cookie を有効にする <br> <br> 2. クロスサイトトラッキングの無効化 <br> <br> 3. 再認証 <br> <br> 4. 該当なし | はい | はい | はい（3.2 以降） |
| N500 | エラー | 内部エラー。 <br> <br> 注意：これは、元のエラーシステムの「汎用認証エラー」と「内部認証エラー」です。 このエラーは最終的に廃止されます。 | AccessEnablerDebug を使用し、何が問題になったかを判断するためにデバッグログ（console.log 出力）を調べます。 | 該当なし | はい | はい | 該当なし |
| R401 | エラー | アクセストークンの取得中にエラーが発生しました。 <br> <br> 注意：これは回復不能なエラーです。 アプリケーションが使用できないことをユーザーに通知します。 | - iOS：アプリケーションのソフトウェア文とカスタムスキームを確認します。 <br> <br> - JavaScript: Web サイトアプリケーションで software 文を確認します。 <br> <br> Zendesk を使用してチケットを開き、システムが一時的に利用できないことをユーザーに知らせます。 | 該当なし | v4.0 からはい | はい v3.0 以降 | はい（3.2 以降） |
| R400 | エラー | アプリケーションが登録されていません。 ソフトウェアステートメントが無効か、取り消されました。 <br> <br> 注意：これは回復不能なエラーです。 アプリケーションが使用できないことをユーザーに通知します。 | - iOS：アプリケーションのソフトウェア文とカスタムスキームを確認します。 <br> <br> - JavaScript: Web サイトアプリケーションで software 文を確認します。 <br> <br> Zendesk を使用してチケットを開き、システムが一時的に利用できないことをユーザーに知らせます。 | 該当なし | v4.0 からはい | はい v3.0 以降 | はい（3.2 以降） |
| REG500 | エラー | 登録コードをサーバーから取得できませんでした。 <br> <br> 注意：これは回復不能なエラーです。 アプリケーションが使用できないことをユーザーに通知します。 | Zendesk を使用してチケットを開き、システムが一時的に使用できないことをユーザーに通知します。 | 該当なし | v4.0 からはい | はい v3.0 以降 | はい（3.2 以降） |
| REGCODE | 成功 | tvOS プラットフォームでの setSelectedProvider API というアプリケーション。 | 提供された登録コードを使用してログインするために、第 2 のデバイス（画面）を使用するようにユーザーに指示/プロンプトを表示します。 | 第 2 のデバイス（画面）で regcode を使用して認証を開始します。 | 該当なし | tvOS の場合のみ可 | 該当なし |
| Z010 | 警告 | 選択した MVPD に対して authenticate-all または authorize-all 分解ルールが適用されている間、ユーザーは承認されました。 | オプションで、MVPD の問題が原因で「無料」のアクセスを受け取っていることをユーザーに通知します。 | 該当なし | はい | はい | はい（3.2 以降） |
| Z011 | 情報 | ユーザーは TempPass を使用して認証されました | オプションでユーザーに通知します | 該当なし | はい | はい | はい（3.2 以降） |
| Z100 | エラー | 要求されたリソースのサブスクリプションがユーザーにないか、MVPD に起因するその他の理由（例：ビデオがユーザーアカウントの保護者による制御設定と一致しない）が原因で、認証に失敗しました |  — 再生を許可しません。 <br> <br>  — ユーザーに通知します。 <br> <br>  — エラーメッセージ MAY 内の&#39;message&#39;キーには、MVPD が提供するより詳細なメッセージが含まれています。 | 該当なし | はい | はい | はい（3.2 以降） |
| Z110 | エラー | MVPD 拒否が繰り返し行われたため、承認が拒否されました。 不正の試みまたは DOS が発生する可能性があります。 |  — 再生を許可しません。 <br> <br>  — ユーザーに通知します。 | 該当なし | はい | はい | はい（3.2 以降） |
| Z120 | エラー | MVPD との通信に技術的な理由により、承認が拒否されました。 ネットワークエラーが発生した可能性があります。 |  — 再生を許可しません。 <br> <br> - MVPD が困難を経験し、後でやり直す必要があることをユーザーに通知します。 | 後でお試しください。 | はい | はい | はい（3.2 以降） |
| Z130 | エラー | 無効な/形式の正しくないリソースが使用されたので、認証が拒否されました。 | リソース文字列を確認し、修正します。 一般に、このエラーは、MRSS の形式が正しくないか、MRSS の代わりにプレーン文字列を使用していることが原因です。 | 該当なし | はい | はい | はい（3.2 以降） |
| Z169 | エラー | authzNone 劣化ルールが指定されたリソースに適用されたので、承認が拒否されました。 | ユーザーに通知 | 該当なし | はい | はい | はい（3.2 以降） |
| Z500 | エラー | 内部エラー。 <br> <br>  注意：これは、従来の「汎用認証エラー」と「内部認証エラー」です。 このエラーは最終的に廃止されます。 | AccessEnablerDebug を使用し、何が問題になったかを判断するためにデバッグログ（console.log 出力）を調べます。 | 該当なし | はい | はい | はい（3.2 以降） |
| P100 | エラー | 事前認証に失敗しました。 多くの場合、これは、リソースに対する認証をリクエストしすぎることが原因である可能性があります。 |  — 使用できるリソースの最大数を超えてはなりません。 <br> <br> - Adobe Pass認証サポートに連絡して、許可されるリソースの最大数を調べたり、設定したりします。 | 該当なし | はい v3.0 以降 | はい | はい（3.2 以降） |
| IS2XX | エラー | これらのエラーコードは、個別化サーバーのエンドポイント応答データの形式が無効か、必要な個別化情報が見つからない場合に返されます。 | Zendesk を使用してチケットを開き、システムが一時的に利用できないことをユーザーに知らせます。 | 該当なし | はい v3.0 以降 | 該当なし | 該当なし |
| IS4XX | エラー | これらのエラーコードは、個別化サーバーエンドポイントが失敗した場合に返されます。4XX は、応答の HTTP ステータスコードです。 | Zendesk を使用してチケットを開き、システムが一時的に利用できないことをユーザーに知らせます。 | 該当なし | はい v3.0 以降 | 該当なし | 該当なし |
| IS5XX | エラー | これらのエラーコードは、個別化サーバーエンドポイントが失敗した場合に返されます。5XX は、応答の HTTP ステータスコードです。 | Zendesk を使用してチケットを開き、システムが一時的に利用できないことをユーザーに知らせます。 | 該当なし | はい v3.0 以降 | 該当なし | 該当なし |
| IS0 | エラー | このコードが返されるのは、個別化サーバーエンドポイントが応答しなかったため、接続がタイムアウトした場合です | Zendesk を使用してチケットを開き、システムが一時的に利用できないことをユーザーに知らせます。 | 該当なし | はい v3.0 以降 | 該当なし | 該当なし |
| LS011 | 警告 | LSO/LocalStorage の問題と WebStorage の問題（または利用不可）が原因で、AccessEnabler は揮発性ストレージを使用しています。 <br> <br> 認証/認証は、現在のページの後は保持されません。 各ページが読み込まれるたびに、ユーザーは認証を必要とします。 設定された TTL は、ページのリロード時には適用されません。 |  — 制限事項をユーザーに通知します。 <br> <br>  — 使用可能なストレージ容量を増やす方法をユーザーに通知します。 <br> <br>  — または、ストレージを消去するためにログアウトします。 |  — ストレージを増やします。 <br> <br>  — ストレージをクリアするにはログアウトします。 | はい | 該当なし | 該当なし |

<br>

## 元のエラーレポート {#original-error-reporting}

この項では、元のエラー・レポート・システムと元のエラー・コードについて説明します。 元のエラー・レポート・システムでは、AccessEnabler は次の 2 つのコールバック関数にエラーを渡します。 `setAuthenticationStatus()` ～に電話した後で `checkAuthentication()`; `tokenRequestFailed()`（への呼び出しが失敗した後） `checkAuthorization()` または `getAuthorization()`.

元のエラーレポートとステータス API は、引き続き以前と同じように機能します。 ただし、今後、元のエラーレポート API は更新されません。 古いエラーに関する新しいエラーのレポートと更新は、新しいエラーにのみ反映されます [高度なエラー報告システム](#advanced-error-reporting).


元のエラー報告システムの使用例については、 [JavaScript API リファレンス](/help/authentication/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) および [tokenRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#token-request-failed-error-msg) 関数 [iOS/tvOS API リファレンス](/help/authentication/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#setAuthNStatus)および [tokentRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#tokenReqFailed), [Android API リファレンス](/help/authentication/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus) および [tokenRequestFailed()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### 元のコールバックエラーコード {#original-callback-error-codes}

| **一般的なエラー** | |
|---|---|
| 内部エラー | 要求を処理しようとした際にシステムエラーが発生しました。 |
| プロバイダーが選択されていませんエラー | 顧客がプロバイダー選択ダイアログでキャンセルしたときに発生します。 |
| プロバイダを利用できませんエラー | 使用可能なプロバイダーがない場合に発生します。 |
|  |  |
| **認証エラー** | |
| 汎用認証エラー | 理由が不明な場合や公開できない場合に返されます。 |
| 内部認証エラー | 認証を試みた際にシステムエラーが発生しました。 |
| ユーザー未認証エラー | ユーザーが認証されていません。 |
|  |  |
| **認証エラー** |  |
| 汎用認証エラー | 理由が不明な場合や公開できない場合に返されます。 |
| 内部認証エラー | 認証を試みた際にシステムエラーが発生しました。 |
| ユーザーが認証されていませんエラー | お客様は、リクエストされたコンテンツの表示を許可されていません。 |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
