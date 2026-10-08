---
title: IOS SDK 3.1以降でのWKWebView サポート
description: IOS SDK 3.1以降でのWKWebView サポート
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%
---
# iOS SDK 3.1以降での（レガシー） WKWebViewのサポート {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

**AppleがiOSでUIWebViewを非推奨にしているため、WKWebViewをサポートしてiOS SDK 3.1を更新しました。**

## 互換性 {#compatibility}

IOS SDK バージョン 3.1以降、実装者はWKWebViewまたはUIWebViewを同じ意味で使用できるようになりました。 UIWebViewはAppleで非推奨となっているため、今後のiOS バージョンで発生する問題を回避するために、アプリをWKWebViewに移行する必要があります。

移行は、WKWebViewでUIWebView クラスを切り替えるだけで行われますが、Adobe AccessEnablerに関して行うべき具体的な作業はありません。

## 既知の問題 {#known-issues}

AdobeのAccessEnablerは、非表示の内部UIWebView インスタンスを使用して、特定のMVPDに対して「[&#x200B; パッシブ認証](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)」を実行しました。 「パッシブ」フローは、各依頼者IDに対する認証を必要とするMVPDに役立ち、このフローから、SSO エクスペリエンス（Adobe SSO）をシミュレートするために、複数のiOS アプリケーションで同じチーム IDを使用するプログラマーにメリットをもたらしました。 この機能は現在、限られた数のMVPDで使用されています。

この機能では、UIWebViewのビヘイビアーを使用して、Adobeが認証Cookieを取得し、「パッシブ」フロー中にそれらを再生できるようにしました。 WKWebViewは、Adobeがログイン時に設定されたCookieを取得し、WKWebViewの非表示のインスタンスを使用して再生することを防ぐ、より強力なセキュリティを導入します。 このセキュリティ向上により、「パッシブ」フローは、非常に特定の実装シナリオ（同じチーム IDを使用する複数のアプリケーション）で非常に限定的なMVPDのセットにのみメリットを与えることを考慮して、Adobeは、Web ビューを使用してMVPDを認証する「パッシブ認証」機能を削除しました。

この機能は、SFSafariViewControllerを使用するように設定されたMVPDには引き続き存在しますが、この場合、SFSafariViewControllerは「非表示」の方法で使用できないため、「パッシブ」認証がユーザーに表示されることに注意してください。
