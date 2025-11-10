---
title: iOS/tvOS v3.x 移行ガイド
description: iOS/tvOS v3.x 移行ガイド
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---

# （従来の）iOS/tvOS v3.x 移行ガイド {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!TIP]
> 
> **注：**
>
> - iOS sdk バージョン 3.1 以降、実装者は、WKWebView または UIWebView を区別なく使用できるようになりました。 UIWebView は非推奨（廃止予定）なので、今後のiOSのバージョンに関する問題を回避するため、アプリを WKWebView に移行する必要があります。
> - マイグレーションは、WKWebView を使用して UIWebView クラスを切り替えるだけで済むことに注意してください。Adobeの AccessEnabler に関しては、特に必要な作業はありません。

</br>

## ビルド設定の更新 {#update}

このリリースには、SWIFT 言語で記述された機能が含まれています。 アプリが完全に Objective-C の場合、ターゲットのビルド設定の「Swift 標準ライブラリを常に埋め込む」チェックボックスを「はい」に設定する必要があります。 このオプションを設定すると、Xcode はアプリにバンドルされているフレームワークをスキャンし、Swift コードが含まれている場合は、関連するライブラリをアプリのバンドルにコピーします。 ビルド設定を更新しないと、AccessEnabler.framework や各種 `ibswift*` ライブラリを読み込めないというエラーが表示され、アプリがクラッシュする場合があります。

</br>

## ソフトウェア ステートメントを追加する {#add}

> ソフトウェア ステートメントの取得方法の詳細については、このページを参照してください。
> ページ：
> [アプリケーションの登録 &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

ソフトウェアのステートメントを取得したら、それをリモートサーバーでホストすることをお勧めします。これにより、App Storeに新しいバージョンのアプリケーションをデプロイしなくても、簡単にソフトウェアを失効させたり、変更したりできます。 アプリケーションが起動したら、リモート・サイトからソフトウェア・ステートメントを取得し、それを AccessEnabler コンストラクタに渡します。

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API 情報はこちら：[iOS/tvOS API リファレンス &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## カスタム URL スキームの追加 {#add-custom}

> カスタム URL スキームの取得方法については、次のページを参照してください。[&#x200B; 顧客 URL スキームの取得 &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

カスタム URL スキームを取得したら、それをアプリケーションの info.plist ファイルに追加する必要があります。 カスタム スキームの形式は `adbe.u-XFXJeTSDuJiIQs0HVRAg://` です。 ファイルに追加する際は、コロンとスラッシュを省略する必要があります。 上記の例は `adbe.u-XFXJeTSDuJiIQs0HVRAg` になります。

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

## カスタム URL スキームでの呼び出しのインターセプト {#intercept}

これは、以前にアプリケーションで [setOptions （\[&quot;handleSVC&quot;:true&quot;\]）呼び出しを介した手動の Safari View Controller （SVC）処理が有効になっており &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)Safari View Controller （SVC）を必要とする特定の MVPD に対してのみ適用されます。そのため、認証の URL を読み込み、UIWebView/WKWebViewController コントローラではなくコントローラでエンドポイントをからログアウトするする必要があります。

認証およびログアウトのフロー中、アプリケーションは `SFSafariViewController `controller のアクティビティを監視する必要があります。これは、複数のリダイレクトを実行するためです。 `application's custom URL scheme` ーザーが定義した特定のカスタム URL （など）をアプリケーションが読み込んだ瞬間を検出する必要があり `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)` す。 コントローラがこの特定のカスタム URL を読み込むとき、アプリケーションは `SFSafariViewController` を閉じて AccessEnabler の `handleExternalURL:url `API メソッドを呼び出す必要があります。

`AppDelegate` に次のメソッドを追加します。

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API 情報はこちら：[&#x200B; 外部 URL を処理 &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## setRequestor メソッドの署名の更新 {#update-setreq}

新しいSDKでは新しい認証機構が使用されるので、signedRequestId パラメーターや（tvOS の場合は）公開鍵と秘密鍵は必要ありません。 `setRequestor` のメソッドは簡略化されており、必要なのは requestorID のみです。

### iOS

このコード：

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

次のようになります。

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

次のようになります。

```swift
    accessEnabler.setRequestor(requestorId)
```

> API 情報はこちら：[&#x200B; リクエスターを設定 &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## getAuthenticationToken メソッドを handleExternalURL メソッドに置き換えます {#replace}

認証フローを完了するために、`getAuthentication` の方法が過去に使用されていました。 名前が誤解を招くので、名前を `handleExternalURL` に変更し、url をパラメーターとして取ります。

こののすべての箇所を変更：

```swift
    accessEnabler.getAuthenticationToken()
```

この中に：

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API 情報はこちら：[&#x200B; 外部 URL を処理 &#x200B;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
