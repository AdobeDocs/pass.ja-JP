---
title: iOS/tvOS v3.x 移行ガイド
description: iOS/tvOS v3.x 移行ガイド
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# iOS/tvOS v3.x 移行ガイド {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

>[!TIP]
> 
> **メモ：**
>
> - iOS sdk バージョン 3.1 以降、実装者は WKWebView または UIWebView を同じ意味で使用できるようになりました。 UIWebView は非推奨となっているので、今後のiOSバージョンの問題を回避するために、アプリケーションは WKWebView に移行する必要があります。
> - 移行は、単に UIWebView クラスを WKWebView で切り替えるだけで済むので、Adobeの AccessEnabler に関して行うべき特定の作業はありません。

</br>

## ビルド設定を更新 {#update}

このリリースには、SWIFT 言語で記述された機能が含まれています。 アプリが完全に Objective-C の場合、Target のビルド設定の「Always Embed Swift Standard Libraries」チェックボックスを「Yes」に設定する必要があります。 このオプションを設定すると、Xcode はアプリ内のバンドルされたフレームワークをスキャンし、Swift コードが含まれる場合は、関連するライブラリをアプリのバンドルにコピーします。 ビルド設定を更新しないと、AccessEnabler.framework などの各種の読み込みができないというエラーが表示されてアプリがクラッシュする場合があります `ibswift*` ライブラリ。

</br>

## ソフトウェア文の追加 {#add}

> ソフトウェア文の取得方法に関する情報は、次を参照してください。
> ページ：
> [申請の登録](/help/authentication/iostvos-application-registration.md)

ソフトウェアステートメントを取得したら、リモートサーバーでホストすることをお勧めします。そうすれば、App Storeに新しいバージョンのアプリケーションをデプロイしなくても、簡単に取り消したり、変更したりできます。 アプリケーションが起動したら、リモートの場所からソフトウェア文を取得し、AccessEnabler コンストラクタに渡します。

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API 情報は、こちらを参照してください。 [iOS/tvOS API リファレンス](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## カスタム URL スキームを追加する {#add-custom}

> カスタム URL スキームの取得方法について詳しくは、このページを参照してください。 [顧客 URL スキームの取得](/help/authentication/iostvos-application-registration.md)

カスタム URL スキームを取得したら、そのスキームをアプリケーションの info.plist ファイルに追加する必要があります。 カスタムスキームの形式は次のとおりです。 `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. ファイルに追加する場合は、コロンとスラッシュを省略する必要があります。 上記の例は、 `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## カスタム URL スキームでの呼び出しの傍受 {#intercept}

これは、アプリケーションが以前に手動で Safari ビューコントローラ (SVC) を有効にしていた場合にのみ、 [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/iostvos-sdk-api-reference.md) Safari View Controller(SVC) を必要とする特定の MVPD に対して、およびを呼び出すことで、UIWebView/WKWebView コントローラの代わりに SFSafariViewController コントローラによって認証およびログアウトエンドポイントの URL を読み込む必要があります。

認証およびログアウトフロー中に、アプリケーションは、 `SFSafariViewController `複数のリダイレクトを経る際のコントローラー アプリケーションが、 `application's custom URL scheme` ( 例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. コントローラーがこの特定のカスタム URL を読み込むと、アプリケーションは `SFSafariViewController` AccessEnabler の `handleExternalURL:url `API メソッド。

を `AppDelegate` 次のメソッドを追加します。

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API 情報は、こちらを参照してください。 [外部 URL を処理](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## setRequestor メソッドの署名を更新します。 {#update-setreq}

新しい SDK は新しい認証メカニズムを使用しているので、signedRequestId パラメーターや公開鍵および秘密鍵（tvOS の場合）は不要です。 The `setRequestor` メソッドは簡略化され、requestorID のみ必要です。

### iOS

このコード：

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

次になります。

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

このコード：

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

次になります。

```swift
    accessEnabler.setRequestor(requestorId)
```

> API 情報は、こちらを参照してください。 [要求者の設定](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## getAuthenticationToken メソッドを handleExternalURL メソッドに置き換えました。 {#replace}

`getAuthentication` メソッドは、過去に認証フローの完了に使用されていました。 名前がわかりにくいので、名前をに変更しました。 `handleExternalURL` は url をパラメーターとして取ります。

次のすべての箇所を変更します。

```swift
    accessEnabler.getAuthenticationToken()
```

を次のように変更します。

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API 情報は、こちらを参照してください。 [外部 URL を処理](/help/authentication/iostvos-sdk-api-reference.md)
