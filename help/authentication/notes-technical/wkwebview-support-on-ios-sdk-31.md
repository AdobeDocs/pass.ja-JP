---
title: iOS SDK 3.1 以降での WKWebView のサポート
description: iOS SDK 3.1 以降での WKWebView のサポート
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# iOS SDK 3.1 以降での WKWebView のサポ {#wkwebview-support-on-ios-sdk-3.1} ト

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

**AppleによりiOSの UIWebView が廃止されたため、iOS SDK 3.1 が更新されて、WKWebView がサポートされるようになりました。**

## 互換性 {#compatibility}

iOS SDK バージョン 3.1 以降、実装者は、WKWebView または UIWebView を区別なく使用できるようになりました。 UIWebView はAppleによって非推奨（廃止予定）となったので、今後のiOS バージョンの問題を回避するため、アプリを WKWebView に移行する必要があります。

マイグレーションは、WKWebView を使用して UIWebView クラスを切り替えるだけで済むことに注意してください。Adobeの AccessEnabler に関しては、特に必要な作業はありません。

## 既知の問題 {#known-issues}

Adobeの AccessEnabler は、隠された内部 UIWebView インスタンスを使用して、特定の MVPD に対して [ パッシブ認証 ](/help/authentication/integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md) を実行します。 「パッシブ」フローは、各リクエスター ID に対して認証を必要とする MVPD で役に立ち、このフローから、SSO エクスペリエンス（AdobeSSO）をシミュレーションするために複数のiOS アプリケーションで同じチーム ID を使用するプログラマーのメリットが得られました。 この機能は、現在、限られた数の MVPD で使用されています。

この機能では、Adobeが認証 Cookie をキャプチャし、「パッシブ」フロー中に再生できる UIWebView の動作を使用しました。 WKWebView は、Adobeがログイン時に設定された Cookie を取得し、WKWebView の隠しインスタンスを使用して再生するのを防ぐ、より強力なセキュリティを導入しています。 このセキュリティの向上により、「パッシブ」フローでは、非常に限定的な MVPD のセット（同じチーム ID を使用する複数のアプリケーション）しかメリットを受けなかったことを考えると、Adobeでは、Web ビューを使用して認証を行う MVPD の「パッシブ認証」機能を削除しました。

この機能は、SFSafariViewController を使用するように設定された MVPD に対しては引き続き存在しますが、この場合、SFSafariViewController は「隠し」方法で使用できないため、「パッシブ」認証がユーザーに表示されることに注意してください。
