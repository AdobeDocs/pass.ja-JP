---
title: iOS SDK 3.1 以降での WKWebView のサポート
description: iOS SDK 3.1 以降での WKWebView のサポート
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# iOS SDK 3.1 以降での WKWebView のサポート {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

**iOSでの UIWebView の廃止に伴い、Apple SDK 3.1 が更新され、WKWebView がサポートされました。**

## 互換性 {#compatibility}

iOS SDK バージョン 3.1 以降、実装者は、WKWebView または UIWebView を同じ意味で使用できるようになりました。 UIWebView はAppleで非推奨となっているので、今後のiOSバージョンでの問題を回避するために、アプリケーションは WKWebView に移行する必要があります。

移行は、単に UIWebView クラスを WKWebView で切り替えるだけで済むので、Adobeの AccessEnabler に関して行うべき特定の作業はありません。

## 既知の問題 {#known-issues}

Adobeの AccessEnabler は、非表示の内部 UIWebView インスタンスを使用して、「[パッシブ認証](/help/authentication/sso-passive-authn.md)」が表示されます。 「パッシブ」フローは、各要求者 ID に対する認証が必要な MVPD で役立ち、このフローから、SSO エクスペリエンス (AdobeSSO) をシミュレートするために、複数のiOSアプリケーションで同じチーム ID を使用したプログラマーの役に立ちました。 この機能は、現在、限られた数の MVPD で使用されています。

この機能では、Adobeが認証 Cookie を取り込み、「パッシブ」フローの間に再生できる UIWebView の動作を使用しました。 WKWebView は、Adobeがログイン時に設定された Cookie を取得し、WKWebView の隠しインスタンスを使用して Cookie を再生するのを防ぐ、より強力なセキュリティを導入します。 このセキュリティの向上と、「パッシブ」フローは、非常に特定の実装シナリオ ( 同じチーム ID を使用する複数のAdobe) で、非常に限られた MVPD のセットにのみ利益を与えたと考えたため、WebViews を使用して認証する MVPD の「パッシブ認証」機能を削除しました。

この機能は、SFSafariViewController を使用するように設定された MVPD にはまだ存在しますが、この場合、SFSafariViewController を「隠し」方法で使用することはできないので、「パッシブ」認証はユーザーに対して表示されます。
