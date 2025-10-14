---
title: iOS SDK 3.1 以降での WebView のサポート
description: iOS SDK 3.1 以降での WebView のサポート
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# （従来の）iOS SDK 3.1 以降での WKWebView のサポー {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

**AppleによりiOSの UIWebView が非推奨（廃止予定）となったので、WKWebView をサポートするようにiOS SDK 3.1 を更新しました。**

## 互換性 {#compatibility}

iOS SDK バージョン 3.1 以降では、実装者は WKWebView または UIWebView を区別なく使用できるようになりました。 UIWebView はAppleによって非推奨（廃止予定）となったので、今後のiOS バージョンの問題を回避するため、アプリを WKWebView に移行する必要があります。

マイグレーションは、WKWebView を使用して UIWebView クラスを切り替えるだけで済むことに注意してください。Adobeの AccessEnabler に関しては、特に必要な作業はありません。

## 既知の問題 {#known-issues}

Adobeの AccessEnabler は、隠された内部 UIWebView インスタンスを使用して、特定の MVPD に対して [&#x200B; パッシブ認証 &#x200B;](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md) を実行します。 「パッシブ」フローは、各リクエスター ID に対して認証を必要とする MVPD で役に立ち、このフローから、SSO エクスペリエンス（AdobeSSO）をシミュレーションするために複数のiOS アプリケーションで同じチーム ID を使用するプログラマーのメリットが得られました。 この機能は、現在、限られた数の MVPD で使用されています。

この機能では、Adobeが認証 Cookie をキャプチャし、「パッシブ」フロー中に再生できる UIWebView の動作を使用しました。 WKWebView は、Adobeがログイン時に設定された Cookie を取得し、WKWebView の隠しインスタンスを使用して再生するのを防ぐ、より強力なセキュリティを導入しています。 このセキュリティの向上により、「パッシブ」フローでは、非常に限定的な MVPD のセット（同じチーム ID を使用する複数のアプリケーション）しかメリットを受けなかったことを考えると、Adobeでは、Web ビューを使用して認証を行う MVPD の「パッシブ認証」機能を削除しました。

この機能は、SFSafariViewController を使用するように設定された MVPD に対しては引き続き存在しますが、この場合、SFSafariViewController は「隠し」方法で使用できないため、「パッシブ」認証がユーザーに表示されることに注意してください。
