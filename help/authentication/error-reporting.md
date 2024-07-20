---
title: エラーレポート
description: エラーレポート
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2989'
ht-degree: 0%

---

# エラーレポート {#error-reporting}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。


## 概要 {#overview}

Adobe Pass認証のエラーレポートは、現在、次の 2 つの方法で実装されています。

* **高度なエラーレポート** 実装者は、高度なエラーレポートを受け取るために、[AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) の場合はエラーコールバックを登録し、[AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) および [AccessEnabler Android SDK](#accessenabler-android-sdk) の場合は「`status`」という名前のインターフェイスメソッドを実装します。 エラーは、「**情報**」、「**警告**」、「**エラー** タイプに分類されます。 このレポートシステムは **非同期** なので、**複数のエラーがトリガーされる順序は保証されません**。  高度なエラーレポートシステムについて詳しくは、「[ 高度なエラーレポート ](#advanced-error-reporting)」の節を参照してください。

* **元のエラーレポート -** 特定のリクエストが失敗した場合に、エラーメッセージが特定のコールバック関数に渡される静的なレポートシステム。 エラーは、汎用、認証、承認の各タイプにグループ化されます。 元のシステムで報告されたエラーのリストについては、[ 元のエラーレポート ](#original-error-reporting) の節を参照してください。


## 高度なエラーレポート {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>古い [ 元のエラーレポート ](#original-error-reporting) API は引き続き機能し、高度なエラーレポートによって機能が損なわれることはありませんが、元のエラーレポートは更新されなくなります。 新しいエラーと更新はすべて、高度なエラーレポートシステムに対して行われます。

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

新しいエラーレポートシステムはオプションのままなので、実装者はエラーハンドラーコールバックを明示的に登録して、高度なエラーレポートを受け取ることができます。 システムには、複数のエラーコールバックを動的に登録および登録解除する機能が含まれています。 さらに、AccessEnabler JavaScript SDK がロードされるとすぐに新しいエラーコールバックを登録できます。他の初期化を行う必要はありません（`setRequestor()` を呼び出す前に）。つまり、初期化および設定エラーに関する高度なレポートを受け取ることができます。


#### 実装 {#access-enab-js-imp}

yourErrorHandler （errorData:Object）


エラーハンドラーコールバック関数は、次の構造を持つ単一のオブジェクト（マップ）を受け取ります。

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

### 1. バインド {#bind}

**`.bind(eventType:String, handlerName:String):void`**

イベントのハンドラーを追加します。

**`eventType`** - AccessEnabler JavaScript SDK で高度なエラーレポート コールバックがトリガーされるのは、「`errorEvent`」値のみです。

**`handlerName`** - エラーハンドラー関数の名前を指定する文字列。


両方のバインド・パラメータで使用できるのは、次のセットの文字のみです。`[0-9a-zA-Z][-._a-zA-Z0-9]`。つまり、パラメータは数字または文字で始まる必要があり、その後で使用できるのは、ハイフン、ピリオド、アンダースコアおよび英数字のみです。  また、パラメーターは 1024 文字を超えることはできません。

**例** バインディングエラーハンドラー：

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

技術的な制限により、クローズまたは匿名関数をバインドすることはできません。 2 番目のパラメーターには、メソッドの名前を指定する必要があります。


### 2. バインド解除 {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

以前にアタッチされたイベント ハンドラを削除します。

**`eventType`** - AccessEnabler JavaScript SDK で高度なエラーレポート コールバックがトリガーされるのは、&#39;`errorEvent`&#39;値のみです。

**`handlerName`** - エラーハンドラー関数の名前を指定する文字列。が null の場合は、指定された `eventType` に添付されているすべてのハンドラーが欠落している場合は、削除されます。

両方のバインド・パラメータで使用できるのは、次のセットの文字のみです。`[0-9a-zA-Z][-._a-zA-Z0-9]`。つまり、パラメータは数字または文字で始まる必要があり、その後で使用できるのは、ハイフン、ピリオド、アンダースコアおよび英数字のみです。  また、パラメーターは 1024 文字を超えることはできません。

**例** 単一のエラーハンドラーを削除する場合：

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**例** すべてのエラーハンドラーを削除する

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

新しいエラーレポートシステムは必須なので、実装者は新しい目的 C 「EntitlementStatus」プロトコルに明示的に準拠する必要があります。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 実装 {#accessenab-ios-tvossdk-imp}

実装者は、次の **EntitlementStatus** プロトコルに準拠する必要があります。

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

**status** 関数は、次の構造を持つ単一のオブジェクト（`NSDictionary`）を受け取ります。

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

**1.宣言**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2。 実装**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### AccessEnabler Android SDK {#accessenabler-android-sdk}

新しいエラーレポートシステムは必須です。実装者は、`IAccessEnablerDelegate` インターフェイスで定義されたプロトコルに明示的に準拠する必要があるからです。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 実装 {#access-enablr-androidsdk-imp}

実装者は、インターフェイス `IAccessEnablerDelegate` から新しい `status` メソッドを処理する必要があります。 **`status`** 関数は、次のモデルを持つ単一の **`AdvancedStatus`** オブジェクトを受け取ります。

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


新しいエラーレポートシステムは必須です。実装者は、`IAccessEnablerDelegate` インターフェイスで定義されたプロトコルに明示的に準拠する必要があるからです。 この新しいアプローチにより、プログラマーは高度なエラーレポートを受け取ることができます。

#### 実装 {#access-enab-fireos-sdk-}

実装者は、新しい `status` インターフェイスからのメソッド `IAccessEnablerDelegate` を処理する必要があります。 **`status`** 関数は、次のモデルを持つ単一の **`AdvancedStatus`** オブジェクトを受け取ります。

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

## 高度なエラーコードのリファレンス {#advanced-error-codes-reference}

次の表に、新しいエラー API で公開されたエラーコードと、それらを修正するために実行する必要がある推奨アクションを示します。

| ID | レベル | 説明 | 開発者アクション | ユーザーアクション | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | エラー | 一般的なApple SSO エラー | エラーには、元の VSA エラーを含む詳細フィールドが含まれています。 | 該当なし | 該当なし | はい | 該当なし |
| VSA203 | 情報 | 認証がプラットフォーム SSO を介したログインの結果として発生している間、ユーザーはアプリケーションからログアウトすることにしました。 | tvOS で、設定/アカウント/TV プロバイダから明示的にサインアウトするようにユーザーに指示またはプロンプトを表示します。 iOS/iPadOS で <br><br>[ 設定 ] > [TV プロバイダー ] から明示的にサインアウトするように指示またはプロンプトを表示します。 | tvOS の [ 設定 ] > [ アカウント ] > [ テレビ プロバイダ ] から明示的にサインアウトします。<br> <br> iOS/iPadOS で、「設定」から明示的にサインアウトする – /「TV プロバイダー」 | 該当なし | はい | 該当なし |
| VSA404 | 情報 | アプリケーション ビデオ サブスクライバ アカウントのアクセス許可が不明です。 | シングルサインオン（SSO）ユーザーエクスペリエンスの利点を説明することで、購読情報へのアクセス権を与えることを拒否するユーザーにインセンティブを与えます。 | お客様は、アプリケーション設定（TV プロバイダーのアクセス）または [ 設定 ] -> [iOS/iPadOS の TV プロバイダー ] または [ 設定 ] -> [ アカウント ] -> [tvOS の TV プロバイダー ] のセクションに移動して設定を変更できます。 | 該当なし | はい | 該当なし |
| VSA503 | 情報 | アプリケーション ビデオ サブスクライバ アカウント メタデータの要求に失敗しました。 | MVPD エンドポイントが応答していません。 アプリケーションは、通常の認証フローにフォールバックする可能性があります。 | 該当なし | 該当なし | はい | 該当なし |
| 500 | エラー | 内部エラー | AccessEnablerDebug を使用し、デバッグ ログ（console.log 出力）を調べて、問題の原因を特定します。 | 該当なし | はい | はい | 該当なし |
| SEC403 | エラー | ドメイン セキュリティ エラーです。 リクエスターが無効なドメインを使用しています。 特定の要求者 ID で使用されるすべてのドメインは、Adobeで許可リストに登録する必要があります。 |  – 許可されたドメインのリストからのみ AccessEnabler を読み込みます <br> <br> - Adobeに問い合わせて、<br> で使用するリクエスター ID のドメインホワイトリストを管理してください <br> - iOS：正しい証明書を使用していること、および署名が正しく作成されていることを確認します | 該当なし | 該当なし | はい | 該当なし |
| SEC412 | 警告 | [ リリース 2.5 で使用可能 ] デバイス ID が一致しません。 これは、基になるプラットフォームがデバイス ID を変更する場合に常に発生する可能性があります。 この場合、既存のトークンがクリアされ、ユーザーは認証されなくなります。 これは、ユーザーが JS SDK を使用していて、ローミングを行っている場合に有効です（JS では、クライアント IP がデバイス ID の一部になります）。 そうでない場合、これは不正な試み（別のデバイスからトークンをコピーしようとする試み）を示している可能性があります。 |  – 警告数を監視します。 明らかな理由（最近のブラウザーのアップデートがない、新しいオペレーティングシステム）がなく、不正行為の試みの指標となる可能性がある場合。 <br> <br>- （オプション）再度ログインする必要があることをユーザーに通知します。 | もう一度ログインします。 | はい | はい | はい（3.2 から） |
| SEC420 | エラー | Adobe Pass認証サーバーとの通信時に HTTP セキュリティエラーが発生する。 このエラーは、通常、スプーフィング/プロキシが設定されている場合に発生します。 | - ブラウザーに `[https://]{SP_FQDN\}` を読み込み、SSL 証明書を手動で受け入れます（例：**https://api.auth.adobe.com** または **https://api.auth-staging.adobe.com** <br> <br> - プロキシ証明書を信頼済みとしてマークする | これが通常のユーザーで発生した場合は、中間者攻撃の可能性を示しています。 | はい | はい | はい（3.2 から） |
| CFG100 | 警告 | クライアントマシンの日付/時刻/タイムゾーンが正しく設定されていません。 これにより、認証/承認エラーが発生する可能性があります。 |  – 正しい時刻を設定するようにユーザーに通知します。<br> <br> 使用権限フローは失敗する可能性が高いので、これを防ぐためのアクションを実行します。 | 正しい日付/時刻を設定します。 | はい | はい | はい（3.2 から） |
| CFG400 | エラー | 無効なリクエスター ID が指定されました。 | 開発者は、有効なリクエスター ID を指定する必要があります。 | 該当なし | はい | はい | はい（3.2 から） |
| CFG404 | エラー | Adobe Pass認証サーバーが見つかりませんでした。 これは 3 つのインスタンスで発生する可能性があります。<br><br> – 開発者に無効なスプーフィングが設定されています。 <br><br> - ユーザーのネットワークに問題があり、Adobe Pass Authentication ドメインに到達できない。 <br><br> -Adobe Pass認証サーバーの設定が正しくありません。<br><br>  **注意：** Firefox では、CFG404 （ブラウザー制限）ではなく CFG400 が表示されます | - スプーフィングをチェックします。 <br><br> - ネットワーク / DNS 設定を確認します。 <br><br> -Adobeに通知。 | ネットワーク / DNS 設定を確認します。 | はい | はい | はい（3.2 から） |
| CFG410 | エラー | AccessEnabler が古すぎます。 | キャッシュをクリアするようにユーザーに通知します。 | ブラウザーのキャッシュをクリアします。 | はい | 該当なし | はい（3.2 から） |
| CFG5xx | エラー | Adobe Pass認証サーバーで内部エラーが発生しています。 xx は任意の数を指定できます。 | - Adobe Pass認証が使用できないことをユーザーに通知します。 <br><br> - Adobe Pass認証をバイパスします。<br> <br> -Adobeに通知します。 | 後で試してください。 | はい | はい | はい（3.2 から） |
| N000 | 情報 | ユーザーが認証されていません。 | 該当なし | ログイン。 | はい | はい | はい（3.2 から） |
| N001 | 情報 | バックグラウンドでパッシブ認証の試行が開始されました。 これは、「Authentication Per Requestor」が設定された MVPD で発生します。 ユーザーの認証は自動的に行われることを想定していますが、この場合、初期化時にパフォーマンスが低下します。 | オプションで、ユーザーまたはユーザーに通知する UI に、「作業が進行中です」と通知します。 | ちょっと待って。 | はい | はい | はい（3.2 から） |
| N003 | 情報 | Appleの MVPD ピッカーから「その他の TV プロバイダー」オプションを選択します。 | *displayProviderDialog* コールバックが呼び出され、アプリケーションは通常の認証フローにフォールバックできます。 | 通常の MVPD を選択し、ログイン画面を続行します。 | 該当なし | はい | 該当なし |
| N004 | 情報 | 現在の要求者がサポートしていないテレビ プロバイダを選択します。 | *displayProviderDialog* コールバックが呼び出され、アプリケーションは通常の認証フローにフォールバックできます。 | 通常の MVPD を選択し、ログイン画面を続行します。 | 該当なし | はい | 該当なし |
| N005 | 情報 | MVPD ピッカーが取り消されました。 | 該当なし | 該当なし | はい | はい | はい（3.2 から） |
| N010 | 警告 | 選択した MVPD に対して authenticate-all の低下ルールが設定されている間にユーザーが認証されました。 | 必要に応じて、MVPD の問題が原因で「無料」の無料アクセス権が取得されていることをユーザーに通知します。 | 該当なし | はい | はい | はい（3.2 から） |
| N011 | 情報 | TempPass を使用してユーザーが認証されました。 | - ユーザーに通知します。<br> <br> – 通常の MVPD のリストをオプションで表示します。 | オプションで、通常の MVPD でログインします。 | はい | はい | はい（3.2 から） |
| N111 | 警告 | TempPass の有効期限が切れました。 | - ユーザーに通知します。<br> <br> – 通常の MVPD のリストを表示します。<br> <br> - 「TempPass」オプションを非表示にします。 | 通常の MVPD でログインします。 | はい | はい | はい（3.2 から） |
| N130 | エラー | **セッションに認証トークンが見つかりません。** 次のいずれかが原因となっている可能性があります。<br> <br>1. ブラウザーで（サードパーティ）の Cookie が無効になっている（AccessEnabler JavaScript SDK Version 4.x は適用不可） <br> <br>2. ブラウザーで「クロスサイトトラッキングを防ぐ」が有効になっている（Safari 11 以降） <br> <br>3. セッションの有効期限が切れました <br> <br>4. プログラマーが認証 API を誤った連続で呼び出す <br> <br> 注意：このエラーコードは、フルページリダイレクト認証フローでは使用できません。 | 1. （サードパーティ）の Cookie を有効にするようユーザーに促す <br> <br>2. クロスサイトトラッキング <br> ールを無効にするようユーザーに促す <br>3. <br> の再認証をユーザーに促す <br>4. API を正しい順序で呼び出す | 1. （サードパーティ）の Cookie を有効にする <br> <br>2. クロスサイトトラッキング <br> ールを無効にする <br>3. <br> の再認証 <br>4. 該当なし | はい | はい | はい（3.2 から） |
| N500 | エラー | 内部エラー。<br> <br> メモ：これは、元のエラーシステムの「汎用認証エラー」と「内部認証エラー」です。 このエラーは最終的には廃止されます。 | AccessEnablerDebug を使用し、デバッグ ログ（console.log 出力）を調べて、問題の原因を特定します。 | 該当なし | はい | はい | 該当なし |
| R401 | エラー | アクセストークンの取得中にエラーが発生しました。<br> <br> メモ：これは回復不可能なエラーです。 アプリケーションが使用不可であることをユーザーに通知します。 | - iOS：お使いのアプリケーションのソフトウェア ステートメントおよびカスタム スキームを確認してください。<br> <br> - JavaScript:Web サイトアプリケーションのソフトウェアのステートメントを確認します。<br> <br> Zendesk を使用してチケットを開き、システムが一時的に使用できなくなったことをユーザーに通知します | 該当なし | V4.0 から使用可能 | V3.0 から使用可能 | はい（3.2 から） |
| R400 | エラー | アプリケーションが登録されていません。 ソフトウェア ステートメントが無効か、無効になっています。<br> <br> メモ：これは回復不可能なエラーです。 アプリケーションが使用不可であることをユーザーに通知します。 | - iOS：お使いのアプリケーションのソフトウェア ステートメントおよびカスタム スキームを確認してください。<br> <br> - JavaScript:Web サイトアプリケーションのソフトウェアのステートメントを確認します。<br> <br> Zendesk を使用してチケットを開き、システムが一時的に使用できなくなったことをユーザーに通知します | 該当なし | V4.0 から使用可能 | V3.0 から使用可能 | はい（3.2 から） |
| REG500 | エラー | 登録コードをサーバーから取得できませんでした。<br> <br> メモ：これは回復不可能なエラーです。 アプリケーションが使用不可であることをユーザーに通知します。 | Zendesk を使用してチケットを開き、システムが一時的に使用できないことをユーザーに通知します。 | 該当なし | V4.0 から使用可能 | V3.0 から使用可能 | はい（3.2 から） |
| REGCODE | 成功 | tvOS プラットフォームでは setSelectedProvider API というアプリケーションです。 | 指定された登録コードを使用して、2 番目のデバイス（画面）を使用してログインするようにユーザーに指示またはプロンプトを表示します。 | 2 番目のデバイス（画面）で regcode を使用して認証を開始します。 | 該当なし | tvOS のみ | 該当なし |
| Z010 | 警告 | 選択した MVPD に対して authenticate-all または authorize-all の最適化規則が設定されている間、ユーザーは認証されました。 | 必要に応じて、MVPD の問題が原因で「無料」の無料アクセス権が取得されていることをユーザーに通知します。 | 該当なし | はい | はい | はい（3.2 から） |
| Z011 | 情報 | ユーザーは TempPass を使用して認証されました | オプションで、ユーザーに通知します | 該当なし | はい | はい | はい（3.2 から） |
| Z100 | エラー | 要求されたリソースのサブスクリプションがユーザーにないか、MVPD から生じたその他の理由（ビデオがユーザーアカウントの保護者による制限の設定と一致しないなど）が原因で、認証に失敗しました |  – 再生を許可しない。<br> <br> - ユーザーに通知します。<br> <br> - エラーメッセージの&#39;message&#39; キーには、MVPD によって提供されるより詳細なメッセージが含まれる場合があります。 | 該当なし | はい | はい | はい（3.2 から） |
| Z110 | エラー | MVPD 拒否が繰り返されたため、認証が拒否されました。 不正な試行または DOS の可能性があります。 |  – 再生を許可しない。<br> <br> - ユーザーに通知します。 | 該当なし | はい | はい | はい（3.2 から） |
| Z120 | エラー | MVPD との通信に技術的な理由があるため、認証が拒否されました。 ネットワーク エラーの可能性があります。 |  – 再生を許可しない。<br> <br> - MVPD が困難を経験したことをユーザーに通知し、後で試してください。 | 後で試してください。 | はい | はい | はい（3.2 から） |
| Z130 | エラー | 無効な/形式が正しくないリソースが使用されたため、認証が拒否されました。 | リソース文字列を確認して修正します。 通常、このエラーは、MRSS の形式が正しくないか、MRSS の代わりにプレーンな文字列を使用することが原因です。 | 該当なし | はい | はい | はい（3.2 から） |
| Z169 | エラー | 指定されたリソースに対して authzNone 最適化規則が適用されているため、承認が拒否されました。 | ユーザーに通知 | 該当なし | はい | はい | はい（3.2 から） |
| Z500 | エラー | 内部エラー。<br> <br> メモ：これは従来の「汎用認証エラー」および「内部認証エラー」です。 このエラーは最終的には廃止されます。 | AccessEnablerDebug を使用し、デバッグ ログ（console.log 出力）を調べて、問題の原因を特定します。 | 該当なし | はい | はい | はい（3.2 から） |
| P100 | エラー | 事前認証に失敗しました。 ほとんどの場合、これはリクエストしたリソースが多すぎることが原因です。 |  – 許可されているリソースの最大数を超えて使用しないでください。<br> <br> – 許可されている最大リソース数を見つけて設定するには、Adobe Pass認証サポートにお問い合わせください。 | 該当なし | V3.0 から使用可能 | はい | はい（3.2 から） |
| IS2XX | エラー | これらのエラーコードは、個別化サーバーのエンドポイント応答データの形式が無効な場合、または必要な個別化情報がない場合に返されます。 | Zendesk を使用してチケットを開き、システムが一時的に使用できなくなったことをユーザーに通知します | 該当なし | V3.0 から使用可能 | 該当なし | 該当なし |
| IS4XX | エラー | これらのエラーコードは、個別化サーバーエンドポイント失敗 4XX の場合に返されます。は応答の HTTP ステータスコードです。 | Zendesk を使用してチケットを開き、システムが一時的に使用できなくなったことをユーザーに通知します | 該当なし | V3.0 から使用可能 | 該当なし | 該当なし |
| IS5XX | エラー | これらのエラーコードは、個別化サーバーエンドポイントのエラーの場合に返されます。5XX – は、応答の HTTP ステータスコードです。 | Zendesk を使用してチケットを開き、システムが一時的に使用できなくなったことをユーザーに通知します | 該当なし | V3.0 から使用可能 | 該当なし | 該当なし |
| IS0 | エラー | このコードは、個人化サーバーのエンドポイントが応答せず、接続がタイムアウトした場合に返されます | Zendesk を使用してチケットを開き、システムが一時的に使用できなくなったことをユーザーに通知します | 該当なし | V3.0 から使用可能 | 該当なし | 該当なし |
| LS011 | 警告 | AccessEnabler は、LSO/LocalStorage の問題と WebStorage の問題（または使用不能）により、揮発性ストレージを使用しています。<br> 認証/承認 <br>、現在のページ以降は保持されません。 ページが読み込まれるたびに、ユーザーの認証が必要になります。 設定済みの TTL は、ページの再読み込み時には適用されません。 |  – 制限をユーザーに通知します。<br> <br> – 使用可能なストレージスペースを増やす方法をユーザーに通知します。<br> <br>：または、ストレージをクリアするためにログアウトします。 | - ストレージを増やす。<br> <br> - ストレージをクリアするためにログアウトします。 | はい | 該当なし | 該当なし |

<br>

## 元のエラーレポート {#original-error-reporting}

この節では、元のエラーコードと共に、元のエラーレポートシステムについて説明します。 元のエラー・レポート・システムでは、AccessEnabler は、`checkAuthentication()` の呼び出し後に `setAuthenticationStatus()`、`checkAuthorization()` または `getAuthorization()` の呼び出し失敗後に `tokenRequestFailed()` という 2 つのコールバック関数にエラーを渡します。

元のエラーレポートおよびステータス API は、引き続き以前とまったく同じように機能します。 ただし、今後、元のエラーレポート API は更新されません。 新しいエラー報告および古いエラーに関する更新は、新しい [ 高度なエラー報告システム ](#advanced-error-reporting) にのみ反映されます。


元のエラーレポートシステムの使用例については、[JavaScript API リファレンス ](/help/authentication/javascript-sdk-api-reference.md):[setAuthenticationStatus （） ](/help/authentication/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) および [tokenRequestFailed （） ](/help/authentication/javascript-sdk-api-reference.md#token-request-failed-error-msg) 関数、[iOS/tvOS API リファレンス ](/help/authentication/iostvos-sdk-api-reference.md):[setAuthenticationStatus （） ](/help/authentication/javascript-sdk-api-reference.md#setAuthNStatus) および [tokentRequestFailed （） ](/help/authentication/javascript-sdk-api-reference.md#tokenReqFailed)、[Android API リファレンス ](/help/authentication/android-sdk-api-reference.md):[setAuthenticationStatus （） ](/help/authentication/android-sdk-api-reference.md#setAuthNStatus) および [tokenRequestFailed （） ](/help/authentication/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed) を参照してください。

### 元のコールバックエラーコード {#original-callback-error-codes}

| **一般的なエラー** | |
|---|---|
| 内部エラー | リクエストの処理中にシステムエラーが発生しました。 |
| プロバイダーが選択されていませんエラー | 顧客がプロバイダ選択ダイアログでキャンセルすると発生します。 |
| プロバイダーを利用できないエラー | 利用できるプロバイダーがない場合に発生します。 |
|  |  |
| **認証エラー** | |
| 汎用認証エラー | 理由が不明または公開できない場合に返されます。 |
| 内部認証エラー | 認証を試行中にシステムエラーが発生しました。 |
| User Not Authenticated エラー | ユーザーが認証されていません。 |
|  |  |
| **認証エラー** |  |
| 汎用認証エラー | 理由が不明または公開できない場合に返されます。 |
| 内部認証エラー | の認証中にシステムエラーが発生しました。 |
| ユーザーが承認されていませんエラー | 顧客は要求されたコンテンツを表示する権限がありません。 |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
