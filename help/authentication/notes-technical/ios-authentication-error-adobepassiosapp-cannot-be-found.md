---
title: iOS認証エラー – adobepass.ios.app が見つかりません
description: iOS認証エラー – adobepass.ios.app が見つかりません
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS認証エラー – adobepass.ios.app が見つかりません {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 問題 {#issue}

ユーザーは認証フローを実行中で、プロバイダーを使用して資格情報を正常に入力した後、エラーページ、検索ページ、またはその他のカスタムページにリダイレクトされ、見つからなかったこ `adobepass.ios.app` や解決できなかったことが通知されます。

## 説明 {#explanation}

iOSでは、AuthN フローが完了したことを示す最終的なリダイレクト URL として `adobepass.ios.app` が使用されます。 この時点で、AuthN トークンの取得と AuthN フローの最終処理を行うために、アプリケーションは AccessEnabler に対してリクエストを行う必要があります。

問題は、`adobepass.ios.app` が実際には存在せず、`webView` にエラーメッセージがトリガーされることです。 iOS DemoApp の以前のバージョンは、このエラーが常に AuthN フローの最後にトリガーされると想定し、適切に処理するように設定されていました（`indidFailLoadWithError`）。

**メモ：** この問題は、DemoApp の後続バージョン（iOS SDK のダウンロードに含まれる）で修正されました。

残念ながら、この仮定は正しくありません。 いわゆる「スマート」な DNS またはプロキシサーバーには、発生したエラーを単純に渡さず、代わりに次のいずれかを行うものがあります。

- カスタムエラーページの作成
- 検索ページ、またはその他のタイプの顧客ページまたはポータルに転送します。

その場合、webView に関する限り、iOS webView に戻る応答は完全に有効な応答であり、古い DemoApp が依存していたエラーはトリガーしません。

## 解決策 {#solution}

DemoApp と同じ想定をしないでください。 代わりに、（`shouldStartLoadWithRequest` で）実行する前にリクエストをインターセプトし、適切に処理します。

実行前にリクエストをインターセプトする方法の例：

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

注意すべき点は次のとおりです。

- コード内の任意の場所で直接 `adobepass.ios.app` を使用しないでください。 代わりに、定数 `ADOBEPASS_REDIRECT_URL` を使用します
- `return NO;` ステートメントは、ページの読み込みを防ぎます
- `getAuthenticationToken` 呼び出しは、コード内で 1 回だけ呼び出してください。 `getAuthenticationToken` に対して複数の呼び出しを行うと、結果が未定義になります。
