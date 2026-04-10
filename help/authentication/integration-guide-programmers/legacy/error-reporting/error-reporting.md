---
title: エラー報告
description: エラー報告
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '3034'
ht-degree: 1%

---

# （レガシー） エラー報告 {#error-reporting}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#overview}

Adobe Pass認証のエラーレポートは、現在、次の2つの方法で実装されています。

* **高度なエラー報告**&#x200B;実装者は、[AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)の場合はエラーコールバックを登録し、[AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)および[AccessEnabler Android SDK](#accessenabler-android-sdk)の場合は「`status`」という名前のインターフェイス メソッドを実装して、高度なエラー報告を受け取ります。 エラーは、**情報**、**警告**、**エラー**&#x200B;の種類に分類されます。 このレポートシステムは&#x200B;**非同期**&#x200B;です。その&#x200B;**では、複数のエラーがトリガーされる順序の保証はありません**。  高度なエラー報告システムについて詳しくは、[高度なエラー報告](#advanced-error-reporting)の節を参照してください。

* **元のエラー報告 –**&#x200B;特定のリクエストが失敗した場合にエラーメッセージが特定のコールバック関数に渡される静的レポート システム。 エラーは、汎用、認証、および認証タイプにグループ化されます。 元のシステムで報告されたエラーの一覧については、[元のエラー報告](#original-error-reporting)の節を参照してください。


## 高度なエラーレポート {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>古い[元のエラーレポート ](#original-error-reporting) APIは、以前と同様に引き続き機能します。高度なエラーレポートでは、機能は壊れませんが、元のエラーレポートでは更新は受け取られません。 すべての新しいエラーと更新は、高度なエラーレポートシステムに対して行われます。

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

新しいエラーレポートシステムはオプションのままであるため、実装者はエラーハンドラーコールバックを明示的に登録して、高度なエラーレポートを受信できます。 このシステムには、複数のエラーコールバックを動的に登録および登録解除する機能が含まれています。 さらに、AccessEnabler JavaScript SDKが読み込まれるとすぐに、新しいエラーコールバックを登録できます。他の初期化を実行する必要はありません（`setRequestor()`を呼び出す前）。つまり、初期化エラーと設定エラーに関する高度なレポートを受け取ることができます。


#### 導入 {#access-enab-js-imp}

yourErrorHandler （errorData:Object）


エラーハンドラーのコールバック関数は、次の構造を持つ1つのオブジェクト（マップ）を受け取ります。

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

### &#x200B;1. バインド {#bind}

**`.bind(eventType:String, handlerName:String):void`**

イベントのハンドラーをアタッチします。

**`eventType`** - AccessEnabler JavaScript SDKで高度なエラーレポート コールバックがトリガーされるのは、「`errorEvent`」値のみです。

**`handlerName`** - エラーハンドラー関数の名前を指定する文字列。


両方のバインド パラメーターは、次のセットの文字のみを使用する必要があります：`[0-9a-zA-Z][-._a-zA-Z0-9]`。つまり、パラメーターは数字または文字で始まる必要があり、ハイフン、ピリオド、アンダースコア、英数字のみを含めることができます。  また、パラメーターは1024文字を超えることはできません。

バインド エラーハンドラーの&#x200B;**例**:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

技術的な制限により、クロージャや匿名の関数をバインドすることはできません。 2番目のパラメーターでメソッドの名前を指定する必要があります。


### &#x200B;2. バインド解除 {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

以前にアタッチされたイベントハンドラーを削除します。

**`eventType`** - AccessEnabler JavaScript SDKで高度なエラーレポート コールバックがトリガーされるのは、&#39;`errorEvent`&#39;値のみです。

**`handlerName`** – 指定された`eventType`に対して、エラーハンドラー関数の名前を指定する文字列（がnullの場合または添付されたすべてのハンドラーが見つからない場合）が削除されます。

両方のバインド パラメーターは、次のセットの文字のみを使用する必要があります：`[0-9a-zA-Z][-._a-zA-Z0-9]`。つまり、パラメーターは数字または文字で始まる必要があり、ハイフン、ピリオド、アンダースコア、英数字のみを含めることができます。  また、パラメーターは1024文字を超えることはできません。

単一のエラーハンドラーを削除する&#x200B;**例**:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

すべてのエラーハンドラーを削除する&#x200B;**例**:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

新しいエラー報告システムは必須であるため、実装者は新しいObjective C &quot;EntitlementStatus&quot; プロトコルに明示的に準拠する必要があります。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 導入 {#accessenab-ios-tvossdk-imp}

実装者は、次の&#x200B;**EntitlementStatus** プロトコルに準拠する必要があります。

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

**status**&#x200B;関数は、次の構造を持つ単一のオブジェクト（`NSDictionary`）を受け取ります。

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

実装者は明示的に`IAccessEnablerDelegate` インターフェイス定義プロトコルに準拠する必要があるため、新しいエラーレポートシステムは必須です。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 導入 {#access-enablr-androidsdk-imp}

実装者は、インターフェイス `IAccessEnablerDelegate`から新しい`status` メソッドを処理する必要があります。 **`status`**&#x200B;関数は、次のモデルを持つ1つの&#x200B;**`AdvancedStatus`** オブジェクトを受け取ります。

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


実装者は明示的に`IAccessEnablerDelegate` インターフェイス定義プロトコルに準拠する必要があるため、新しいエラーレポートシステムは必須です。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 導入 {#access-enab-fireos-sdk-}

実装者は、インターフェイス `IAccessEnablerDelegate`から新しい`status` メソッドを処理する必要があります。 **`status`**&#x200B;関数は、次のモデルを持つ1つの&#x200B;**`AdvancedStatus`** オブジェクトを受け取ります。

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

## 詳細エラーコード リファレンス {#advanced-error-codes-reference}

次の表に、新しいエラーAPIによって公開されるエラーコードと、その修正用に提案されたアクションを示します。

| ID | レベル | 説明 | 開発者の操作 | ユーザーアクション | JavaScrip | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | エラー | 汎用Apple SSO エラー | このエラーには、元のVSA エラーを含む詳細フィールドが含まれています。 | なし | なし | はい | なし |
| VSA203 | 情報 | プラットフォーム SSOを介したログインの結果、認証が発生した間、ユーザーはアプリケーションからログアウトすることにしました。 | tvOSの設定/アカウント/TV プロバイダーから明示的にログアウトするようにユーザーに指示またはプロンプトを表示します。<br><br> IOS/iPadOSの設定/テレビプロバイダーから明示的にログアウトするように指示またはプロンプトを表示します。 | tvOSの設定/アカウント/TV プロバイダーから明示的にログアウトします。<br> <br>iOS/iPadOSの設定/TV プロバイダーから明示的にログアウトする | なし | はい | なし |
| VSA404 | 情報 | アプリケーションビデオの購読者アカウントの権限が不明です。 | サブスクリプション情報へのアクセス許可を拒否したユーザーに対して、シングルサインオン（SSO）ユーザーエクスペリエンスのメリットを説明することで、インセンティブを提供します。 | お客様は、アプリケーションの設定（TV Provider access）に移動するか、iOS/iPadOSのSettings -> TV ProviderまたはSettings -> Accounts -> TV Provider on tvOSのセクションに移動することで、その判断を変更できます。 | なし | はい | なし |
| VSA503 | 情報 | Application Video Subscriber Account metadata requestが失敗しました。 | MVPD エンドポイントが応答しません。 通常の認証フローにフォールバックする可能性があります。 | なし | なし | はい | なし |
| 500 | エラー | 内部エラー | AccessEnablerDebugを使用し、デバッグログ（console.log出力）を調べて、何が問題だったのかを判断します。 | なし | はい | はい | なし |
| SEC403 | エラー | Domain Security エラー。 依頼者が無効なドメインを使用しています。 特定の依頼者IDで使用されるすべてのドメインは、Adobeでホワイトリストに登録する必要があります。 |  – 許可されたドメイン <br>のリストからのみAccessEnablerを読み込みます <br> – 使用されている依頼者IDのドメイン ホワイトリストを管理するには、Adobeにお問い合わせください<br> <br> - iOS：正しい証明書を使用していること、および署名が正しく作成されていることを確認してください | なし | なし | はい | なし |
| SEC412 | 警告 | リリース 2.5]で使用可能な[ デバイス IDが一致しません。 これは、基盤となるプラットフォームがデバイス IDを変更するたびに発生する可能性があります。 この場合、既存のトークンはクリアされ、ユーザーは認証されなくなります。 これは、ユーザーがJS SDKを使用しており、ローミングしている場合（JSでは、クライアント IPはデバイス IDの一部です）に正当に発生することに注意してください。 そうでない場合、これは詐欺行為の兆候である可能性があります – 別のデバイスからトークンをコピーしようとする試み。 |  – 警告の数を監視します。 明らかな理由なくスパイクした場合（最近のブラウザー更新なし、新しいオペレーティングシステム）、不正行為の指標になる可能性があります。 <br> <br> – 必要に応じて、再度ログインする必要があることをユーザーに通知します。 | 再度ログインします。 | はい | はい | 3.2から可能 |
| SEC420 | エラー | ADOBE PASS認証サーバーと通信する際のHTTP セキュリティエラー。 このエラーは、通常、スプーフィング/プロキシが配置されている場合に発生します。 | - `[https://]{SP_FQDN\}`をブラウザーに読み込み、SSL証明書を手動で受け入れます（例：**https://api.auth.adobe.com**&#x200B;または&#x200B;**https://api.auth-staging.adobe.com** <br>） <br>- プロキシ証明書を信頼済みとしてマーク | これが通常のユーザーに対して発生した場合は、中間者攻撃の可能性を示しています。 | はい | はい | 3.2から可能 |
| CFG100 | 警告 | クライアントマシンの日付/時刻/タイムゾーンが正しく設定されていません。 これは認証/認証エラーにつながる可能性があります。 |  – 正しい時間を設定するようにユーザーに通知します。<br> <br>使用権限フローは失敗する可能性が高いため、防止するためのアクションを実行します。 | 正しい日付/時刻を設定します。 | はい | はい | 3.2から可能 |
| CFG400 | エラー | 無効な依頼者IDが指定されました。 | 開発者は、有効な依頼者IDを指定する必要があります。 | なし | はい | はい | 3.2から可能 |
| CFG404 | エラー | Adobe Pass認証サーバーが見つかりませんでした。 これは3つのインスタンスで発生する可能性があります：<br><br> – 開発者は無効なスプーフィングを使用しています。<br><br> – このユーザーにはネットワークの問題があり、Adobe Pass認証ドメインにアクセスできません。<br><br> - Adobe Pass認証サーバーが正しく設定されていません。<br><br>  **メモ：** Firefoxでは、CFG404ではなくCFG400が表示されます（ブラウザーの制限） | - スプーフィングを確認します。<br><br> - ネットワーク / DNS設定を確認します。<br><br> -Adobeに情報を提供します。 | ネットワーク / DNS設定を確認します。 | はい | はい | 3.2から可能 |
| CFG410 | エラー | AccessEnablerが古すぎます。 | キャッシュをクリアするようにユーザーに通知します。 | ブラウザーのキャッシュをクリアします。 | はい | なし | 3.2から可能 |
| CFG5xx | エラー | Adobe Pass認証サーバーで内部エラーが発生しています。 xxは任意の数値にできます。 | - Adobe Pass認証が利用できないことをユーザーに通知します。<br><br> - Adobe Pass認証をバイパスします。<br> <br> - Adobeに通知します。 | 後で試してください。 | はい | はい | 3.2から可能 |
| N000 | 情報 | ユーザーは認証されていません。 | なし | ログイン。 | はい | はい | 3.2から可能 |
| N001 | 情報 | パッシブ認証の試行がバックグラウンドで開始されました。 これは、「リクエスト者ごとの認証」で設定されたMVPDに対して発生します。 ユーザーは自動的に認証されることを望んでいますが、初期化するとパフォーマンスにペナルティが課されます。 | オプションで、「作業中」をユーザーに通知するか、ユーザーに警告するUIを表示します。 | 待ってください。 | はい | はい | 3.2から可能 |
| N003 | 情報 | Apple MVPD ピッカーから「その他のテレビプロバイダー」オプションを選択します。 | *displayProviderDialog* コールバックが呼び出され、アプリケーションは通常の認証フローにフォールバックする可能性があります。 | 通常のMVPDを選択し、ログイン画面に進みます。 | なし | はい | なし |
| N004 | 情報 | ユーザーは、現在の依頼者がサポートしていないTV プロバイダーを選択します。 | *displayProviderDialog* コールバックが呼び出され、アプリケーションは通常の認証フローにフォールバックする可能性があります。 | 通常のMVPDを選択し、ログイン画面に進みます。 | なし | はい | なし |
| N005 | 情報 | MVPD ピッカーはキャンセルされました。 | なし | なし | はい | はい | 3.2から可能 |
| N010 | 警告 | 選択したMVPDに対する認証済みのすべての劣化ルールが有効になっている間に、ユーザーが認証されました。 | オプションで、MVPDの問題が原因で「無料」の無料アクセス権を取得していることをユーザーに通知します。 | なし | はい | はい | 3.2から可能 |
| N011 | 情報 | ユーザーはTempPassを使用して認証されました。 | - ユーザーに通知します。<br> <br> – 必要に応じて、通常のMVPDのリストを表示します。 | オプションで、通常のMVPDにログインします。 | はい | はい | 3.2から可能 |
| N111 | 警告 | 期限切れのTempPass。 | - ユーザーに通知します。<br> <br> – 通常のMVPDのリストを表示します。<br> <br> - TempPass オプションを非表示にします。 | 通常のMVPDでログインします。 | はい | はい | 3.2から可能 |
| N130 | エラー | **認証トークンがセッションで見つかりません。**  これは、次のいずれかによるものである可能性があります：<br> <br> 1. ブラウザーの（サードパーティの） Cookieが無効になっています（AccessEnabler JavaScript SDK バージョン 4.xには適用されません） <br> <br> 2. ブラウザーでクロスサイトトラッキング防止が有効になっています（Safari 11以降） <br> <br> 3. セッションの有効期限が切れています：<br> <br> 4. プログラマーが認証APIを誤った順序で呼び出しています<br> <br>注：このエラーコードは、ページ全体のリダイレクト認証フローでは使用できません。 | &#x200B;1. （3 rd パーティ） Cookie <br>を有効にするようユーザーに促す <br> 2. クロスサイトトラッキング <br>を無効にするようユーザーに促す <br> 3. ユーザーに<br>の再認証を促す <br> 4. APIを正しい順序で呼び出す | &#x200B;1. （3 rd パーティ） Cookie <br>を有効にする <br> 2. クロスサイトトラッキングを無効にする<br> <br> 3. <br>の再認証 <br> 4. 該当なし | はい | はい | 3.2から可能 |
| N500 | エラー | 内部エラー。<br> <br>注：これは、元のエラーシステムの「汎用認証エラー」と「内部認証エラー」です。 このエラーは最終的に段階的に削除されます。 | AccessEnablerDebugを使用し、デバッグログ（console.log出力）を調べて、何が問題だったのかを判断します。 | なし | はい | はい | なし |
| R401 | エラー | アクセストークンの取得中にエラーが発生しました。<br> <br>注：これは回復不能なエラーです。 アプリケーションが使用できないことをユーザーに通知します。 | - iOS: アプリケーションのソフトウェアステートメントとカスタムスキームを確認します。<br> <br> - JavaScript: web サイト アプリケーションのソフトウェアに関する記述を確認してください。<br> <br> Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します | なし | v4.0からはい | v3.0からはい | 3.2から可能 |
| R400 | エラー | アプリケーションが登録されていません。 ソフトウェアのステートメントが無効か、取り消されました。<br> <br>注：これは回復不能なエラーです。 アプリケーションが使用できないことをユーザーに通知します。 | - iOS: アプリケーションのソフトウェアステートメントとカスタムスキームを確認します。<br> <br> - JavaScript: web サイト アプリケーションのソフトウェアに関する記述を確認してください。<br> <br> Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します | なし | v4.0からはい | v3.0からはい | 3.2から可能 |
| REG500 | エラー | 登録コードをサーバーから取得できませんでした。<br> <br>注：これは回復不能なエラーです。 アプリケーションが使用できないことをユーザーに通知します。 | Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します。 | なし | v4.0からはい | v3.0からはい | 3.2から可能 |
| REGCODE | 成功 | tvOS プラットフォームのsetSelectedProvider APIというアプリケーション。 | 提供された登録コードを使用して、2番目のデバイス（画面）を使用してログインするようにユーザーに指示またはプロンプトを表示します。 | 2台目のデバイス（画面）でレジコードを使用して、認証を開始します。 | なし | はい、tvOSのみ | なし |
| Z010 | 警告 | 選択したMVPDに対する認証済みデプロイメント規則または認証済みデプロイメント規則が有効な間、ユーザーは承認されました。 | オプションで、MVPDの問題が原因で「無料」の無料アクセス権を取得していることをユーザーに通知します。 | なし | はい | はい | 3.2から可能 |
| Z011 | 情報 | ユーザーはTempPassを使用して承認されました | オプションでユーザーに通知 | なし | はい | はい | 3.2から可能 |
| Z100 | エラー | 承認に失敗しました。要求されたリソースのサブスクリプションがないか、MVPDから送信されたその他の理由（例：ビデオがユーザーアカウントのペアレンタルコントロール設定と一致しない）が原因です |  – 再生を許可しないでください。<br> <br> - ユーザーに通知します。<br> <br> - エラーメッセージの「message」キーに、MVPDから提供される詳細なメッセージが含まれている可能性があります。 | なし | はい | はい | 3.2から可能 |
| Z110 | エラー | MVPDが繰り返し拒否したため、認証が拒否されました。 不正行為またはDOSの可能性がある： |  – 再生を許可しないでください。<br> <br> - ユーザーに通知します。 | なし | はい | はい | 3.2から可能 |
| Z120 | エラー | MVPDとの通信に技術的な理由があるため、承認が拒否されました。 ネットワークエラーの可能性があります。 |  – 再生を許可しないでください。<br> <br> - MVPDで問題が発生したため、後で試す必要があることをユーザーに通知します。 | 後で試してください。 | はい | はい | 3.2から可能 |
| Z130 | エラー | 不正または形式のリソースが使用されたため、認証が拒否されました。 | リソース文字列を確認して修正します。 通常、このエラーは、MRSSの形式が正しくないか、MRSSの代わりにプレーン文字列を使用していることが原因です。 | なし | はい | はい | 3.2から可能 |
| Z169 | エラー | 指定されたリソースに対してauthzNone劣化ルールが適用されているため、承認が拒否されました。 | ユーザーに通知 | なし | はい | はい | 3.2から可能 |
| Z500 | エラー | 内部エラー。<br> <br>注：これは従来の「汎用認証エラー」および「内部認証エラー」です。 このエラーは最終的に段階的に削除されます。 | AccessEnablerDebugを使用し、デバッグログ（console.log出力）を調べて、何が問題だったのかを判断します。 | なし | はい | はい | 3.2から可能 |
| P100 | エラー | 事前認証に失敗しました。 これは、多くのリソースの認証をリクエストすることが原因である可能性が高いです。 |  – 許可されるリソースの最大数を超えて使用しないでください。<br> <br> – 許可されているリソースの最大数を検索または設定するには、Adobe Pass認証サポートにお問い合わせください。 | なし | v3.0からはい | はい | 3.2から可能 |
| IS2XX | エラー | これらのエラーコードは、個人化サーバーエンドポイントの応答データの形式が無効であるか、必要な個人化情報が欠落している場合に返されます。 | Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します | なし | v3.0からはい | なし | なし |
| IS4XX | エラー | これらのエラーコードは、個別化サーバーエンドポイントのエラー4XXの場合に返されます。これは、応答のHTTP ステータスコードです。 | Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します | なし | v3.0からはい | なし | なし |
| IS5XX | エラー | これらのエラーコードは、個人化サーバーエンドポイントのエラー5XXの場合に返されます。これは、応答のHTTP ステータスコードです。 | Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します | なし | v3.0からはい | なし | なし |
| IS0 | エラー | このコードは、個人化サーバーエンドポイントが全く応答しなかったため、接続がタイムアウトした場合に返されます | Zendeskを使用してチケットを開き、システムが一時的に利用できないことをユーザーに通知します | なし | v3.0からはい | なし | なし |
| LS011 | 警告 | AccessEnablerは、LSO/LocalStorageの問題およびWebStorageの問題（または使用不可）が原因で、揮発性ストレージを使用しています。<br> <br>認証/認証は、現在のページを超えて保持されません！ ページが読み込まれるたびに、ユーザーは認証を必要とします。 設定されたTTLは、ページのリロード時に適用されません。 |  – 制限をユーザーに通知します。<br> <br> – 使用可能なストレージ容量を増やす方法をユーザーに通知します。<br> <br> – または、ストレージをクリアするためにログアウトします。 | - ストレージを増やす。<br> <br> - ストレージをクリアするためにログアウトします。 | はい | なし | なし |

<br>

## 元のエラーレポート {#original-error-reporting}

この節では、元のエラーレポートシステムと、元のエラーコードについて説明します。 元のエラー報告システムでは、AccessEnablerは`checkAuthentication()`への呼び出し後`setAuthenticationStatus()`、`tokenRequestFailed()`または`checkAuthorization()`または`getAuthorization()`への呼び出しの失敗後に、これらの2つのコールバック関数にエラーを渡します。

元のエラーレポートとステータス APIは、以前とまったく同じように機能し続けます。 ただし、今後、元のエラーレポート APIは更新されません。 古いエラーに関するすべての新しいエラー報告と更新は、新しい[高度なエラー報告システム ](#advanced-error-reporting)にのみ反映されます。


元のエラーレポートシステムの使用例については、[JavaScript API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[setAuthenticationStatus （） ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error)および[tokenRequestFailed （） ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg)関数、[iOS/tvOS API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md):[setAuthenticationStatus （） ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus)および[tokentRequestFailed （） ](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed)、[Android API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md):[setAuthenticationStatus （） ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus)および[tokenRequestFailed （） ](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed)。

### 元のコールバックエラーコード {#original-callback-error-codes}

| **汎用エラー** | |
|---|---|
| 内部エラー | リクエストの処理中にシステムエラーが発生しました。 |
| Provider not Selected エラー | お客様がプロバイダー選択ダイアログでキャンセルしたときに発生します。 |
| Provider not Available エラー | プロバイダーが利用できない場合に発生します。 |
|  |  |
| **認証エラー** | |
| 汎用認証エラー | 理由が不明な場合、または公開できない場合に返されます。 |
| 内部認証エラー | 認証時にシステムエラーが発生しました。 |
| User Not Authenticated Error | ユーザーは認証されていません。 |
|  |  |
| **認証エラー** |  |
| 汎用承認エラー | 理由が不明な場合、または公開できない場合に返されます。 |
| 内部認証エラー | 承認しようとしたときにシステムエラーが発生しました。 |
| User not Authorized Error | お客様は、リクエストされたコンテンツを表示する権限がありません。 |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
