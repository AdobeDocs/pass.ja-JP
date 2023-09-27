---
title: iOS認証エラー — adobepass.ios.app が見つかりません
description: iOS認証エラー — adobepass.ios.app が見つかりません
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS認証エラー — adobepass.ios.app が見つかりません {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 問題 {#issue}

ユーザーは認証フローを経て、プロバイダーに資格情報を入力すると、エラーページ、検索ページ、またはその他のカスタムページにリダイレクトされ、 `adobepass.ios.app` が見つからなかったか、解決されませんでした。

## 説明 {#explanation}

iOS `adobepass.ios.app` は、AuthN フローが完了したことを示す最終的なリダイレクト URL として使用されます。 この時点で、アプリケーションは AuthN トークンを取得し、AuthN フローを最終決定するために、AccessEnabler にリクエストを送信する必要があります。

問題は `adobepass.ios.app` は実際には存在せず、 `webView`. iOS DemoApp の古いバージョンでは、このエラーは常に AuthN フローの最後にトリガーされ、それに応じて処理するように設定されていると想定していました (`indidFailLoadWithError`) をクリックします。

**注意：** この問題は、後のバージョンの DemoApp(iOS SDK ダウンロードに含まれる ) で修正されました。

残念ながら、この想定は正しくありません。 いわゆる「スマート」DNS またはプロキシサーバーの中には、単に発生したエラーを渡すのではなく、次のいずれかを行うものがあります。

- カスタムエラーページの作成
- 検索ページ、またはその他のタイプの顧客ページやポータルに転送します。

この場合、iOS webView に返される応答は、webView に関する限り完全に有効な応答となり、古い DemoApp が依存していたエラーはトリガーされません。

## 解決策 {#solution}

DemoApp と同じ想定をしないでください。 代わりに、実行前にリクエストを切り取ります ( `shouldStartLoadWithRequest`) を参照し、適切に処理します。

実行前のリクエストの切断方法の例：

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

注意事項を以下に示します。

- 使用しない `adobepass.ios.app` コードの任意の場所に直接配置できます。 代わりに、定数を使用します。 `ADOBEPASS_REDIRECT_URL`
- The `return NO;` 文は、ページの読み込みを妨げます
- 必ず `getAuthenticationToken` の呼び出しは、コード内で 1 回だけ呼び出されます。 への複数の呼び出し `getAuthenticationToken` は未定義の結果になります。
