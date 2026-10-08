---
title: iOS認証エラー – adobepass.ios.appが見つかりません
description: iOS認証エラー – adobepass.ios.appが見つかりません
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%
---
# （レガシー） iOS認証エラー – adobepass.ios.appが見つかりません {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## イシュー {#issue}

ユーザーは認証フローを実行しており、プロバイダーで資格情報を正常に入力した後、エラーページ、検索ページ、またはその他のカスタムページにリダイレクトされ、`adobepass.ios.app`が見つからなかったか解決できなかったことを知らせます。

## 説明 {#explanation}

IOSでは、`adobepass.ios.app`が最終的なリダイレクト URLとして使用され、AuthN フローが完了したことを示します。 この時点で、アプリはAccessEnablerにリクエストを行ってAuthN トークンを取得し、AuthN フローを確定する必要があります。

問題は、`adobepass.ios.app`が実際には存在せず、`webView`にエラーメッセージがトリガーされることです。 古いバージョンのiOS DemoAppでは、このエラーは常にAuthN フローの最後でトリガーされることを想定しており、それに応じて処理するように設定されていました（`indidFailLoadWithError`）。

**注意：**&#x200B;この問題は、後のバージョンのDemoApp （iOS SDK ダウンロードに含まれる）で修正されました。

残念ながら、この仮定は正しくありません。 いわゆる「スマート」 DNSまたはプロキシサーバーがいくつかあり、単に発生したエラーを渡すのではなく、代わりに次のいずれかを行います。

- カスタムエラーページの作成
- 検索ページや、その他のタイプの顧客ページやポータルに移動します。

そのような場合、iOS webViewに返される応答は、webViewに関する限り完全に有効な応答となり、古いDemoAppが依存していたエラーはトリガーされません。

## Solution {#solution}

DemoAppと同じ仮定を行わないでください。 代わりに、リクエストを実行する前にインターセプトし、（`shouldStartLoadWithRequest`で） リクエストを適切に処理します。

リクエストを実行する前にインターセプトする方法の例：

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

注意すべき点がいくつかあります。

- コード内の任意の場所で`adobepass.ios.app`を直接使用しないでください。 代わりに、定数`ADOBEPASS_REDIRECT_URL`を使用してください
- `return NO;` ステートメントは、ページの読み込みを妨げます
- `getAuthenticationToken`呼び出しは、コード内で1回だけ呼び出されるようにしてください。 `getAuthenticationToken`への複数呼び出しでは、結果が未定義になります。
