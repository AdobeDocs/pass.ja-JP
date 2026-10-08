---
title: iOS/tvOS v3.x移行ガイド
description: iOS/tvOS v3.x移行ガイド
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%
---
# （レガシー） iOS/tvOS v3.x移行ガイド {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!TIP]
> 
> **メモ：**
>
> - IOS sdk バージョン 3.1以降、実装者はWKWebViewまたはUIWebViewを同じ意味で使用できるようになりました。 UIWebViewは非推奨（廃止予定）であるため、今後のiOS バージョンで発生する問題を回避するために、アプリをWKWebViewに移行する必要があります。
> - 移行は、WKWebViewでUIWebView クラスを切り替えるだけで行われますが、Adobe AccessEnablerに関して行うべき具体的な作業はありません。

</br>

## ビルド設定の更新 {#update}

このリリースには、SWIFT言語で記述された機能が含まれています。 アプリが完全にObjective-Cである場合は、ターゲットのビルド設定で「Swift Standard Librariesを常に埋め込む」チェックボックスを「はい」に設定する必要があります。 このオプションを設定すると、Xcodeはアプリ内のバンドルされたフレームワークをスキャンし、いずれかのフレームワークにSwift コードが含まれている場合は、関連するライブラリをアプリのバンドルにコピーします。 ビルド設定を更新しないと、AccessEnabler.frameworkまたは様々な`ibswift*` ライブラリを読み込めないというエラーが表示され、アプリがクラッシュする可能性があります。

</br>

## ソフトウェアステートメントの追加 {#add}

> ソフトウェア ステートメントの取得方法については、こちらを参照してください。
> ページ：
> [アプリケーション登録](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

ソフトウェアステートメントが完成したら、これをリモートサーバー上でホスティングすることをお勧めします。これにより、App Storeに新しいバージョンのアプリケーションをデプロイすることなく、リモートサーバーを簡単に取り消したり変更したりできます。 アプリケーションが起動したら、リモートの場所からソフトウェア文を取得し、AccessEnabler コンストラクターに渡します。

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> API情報はこちら：[iOS / tvOS API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## カスタム URL スキームの追加 {#add-custom}

> カスタム URL スキームの取得方法について詳しくは、次のページを参照してください。[顧客URL スキームの取得](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

カスタム URL スキームを取得したら、それをアプリケーションのinfo.plist ファイルに追加する必要があります。 カスタムスキームの形式は`adbe.u-XFXJeTSDuJiIQs0HVRAg://`です。 ファイルに追加する際は、コロンとスラッシュを省略する必要があります。 上記の例は`adbe.u-XFXJeTSDuJiIQs0HVRAg`になります。

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

これは、以前に[setOptions （\[&quot;handleSVC&quot;:true&quot;\]） ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)呼び出しを介した手動Safari View Controller （SVC）処理をアプリケーションで有効にし、特定のMVPDでSafari View Controller （SVC）を必要としていたため、UIWebView/WKWebView コントローラーではなくSFSafariViewController コントローラーで認証エンドポイントとログアウトエンドポイントのURLを読み込む必要がある場合場合場合場合にのみ適用されます。

認証フローとログアウトフロー中に、アプリケーションが複数のリダイレクトを通過する際に`SFSafariViewController ` コントローラーのアクティビティを監視する必要があります。 アプリケーションは、`application's custom URL scheme`によって定義された特定のカスタム URL （例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`）を読み込む瞬間を検出する必要があります。コントローラーがこの特定のカスタム URLを読み込むと、アプリケーションは`SFSafariViewController`を閉じ、AccessEnablerの`handleExternalURL:url `API メソッドを呼び出す必要があります。

`AppDelegate`で、次のメソッドを追加します。

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> API情報：[外部URLを処理](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## setRequestor メソッド署名を更新します {#update-setreq}

新しいSDKは新しい認証方式を使用しているため、signedRequestId パラメーターや公開鍵と秘密鍵（tvOS用）は必要ありません。 `setRequestor` メソッドは簡略化されており、必要なのは依頼者IDのみです。

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

> API情報はこちら：[ リクエスト者を設定](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## getAuthenticationToken メソッドをhandleExternalURL メソッドに置換します {#replace}

`getAuthentication` メソッドは、認証フローを完了するために過去に使用されました。 名前が誤解を招くため、名前が`handleExternalURL`に変更され、URLがパラメーターとして取得されました。

これのすべてのオカレンスを変更：

```swift
    accessEnabler.getAuthenticationToken()
```

これに対して：

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> API情報：[外部URLを処理](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
