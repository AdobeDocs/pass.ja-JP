---
title: トラッキング防止評価Google Chrome
description: トラッキング防止評価Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%
---
# （レガシー） トラッキング防止評価 – Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要

このドキュメントでは、Google Chromeが、サードパーティ Cookieを段階的に廃止する取り組みの一環として予定している今後の変更について、有益なリソースを集計し、評価します。

Google Chrome ブラウザー上で動作し、Adobe Pass Access Enabler JavaScript SDK v4を使用してAdobe Pass Authentication バックエンドサービスと統合しているTV Everywhere （TVE）アプリケーションに対して行われます。

## パブリックリソース

Googleの開発者向けweb サイトおよび公式ブログから収集したリソースの一覧を以下に示します。お客様に相談することをお勧めします。

* [Chromeでサードパーティ Cookieを廃止するための次のステップ](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [プライバシーサンドボックスの開発者用ドキュメント](https://developers.google.com/privacy-sandbox)
* [サードパーティ Cookieの制限に備える](https://developers.google.com/privacy-sandbox/3pcd)
* [サードパーティ Cookieのフェイズアウトに備える](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [サードパーティクッキーの廃止に備える](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Chrome利用者の1%を対象に、デフォルトで制限されているサードパーティ Cookie](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## タイムライン

概要として、Google Chromeは、すべてのサードパーティ Cookieに影響を与えるクロスサイトトラッキングを制限する新機能[Tracking Protection](https://privacysandbox.com/)のテストを開始しました。

当初、これは2024年の初めに開始され、ユーザーの約1%に影響を与え、2024年の第3四半期から最大100%のユーザーにこれを拡張する（暫定的）計画です。

## 評価

Googleは、サードパーティ Cookieのフェーズに備えて推奨されるプレイブックを次のリンクで公開しました。https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

このプレイブックに従って、Google Chrome ブラウザーで動作するTV Everywhere （TVE）アプリケーションの評価を行いました。Adobe Pass Access Enabler JavaScript SDK v4を使用してAdobe Pass Authentication バックエンドサービスと統合しているアプリケーションです。

### まとめ

Google Chromeの今後の更新をシミュレートしたテストに基づいて、主要なTVE ビジネス フロー&#x200B;**は期待どおりに機能し続けます**。

ただし、サードパーティ Cookieを廃止するだけでなく、サードパーティストレージを分割することも含む、Googleの広範な戦略を認識することが重要です。

その結果、Chromeのユーザーは、シングルサインオン（SSO）、シングルログアウト（SLO）、パッシブ認証機能で中断し、使用するTVE アプリケーションごとに個別のログイン/ログアウトアクションが必要になります（Safariの現在のエクスペリエンスに合わせて）。

## 自己評価の要求

お客様に対しても、同様の評価を積極的に行い、潜在的な問題を事前に特定し、改訂されたGoogle Chromeのユーザーエクスペリエンスに慣れるようにしてください。

この評価は、特にAdobe Pass Access Enabler JavaScript SDK v4統合に関するファーストパーティサービスとサードパーティサービスの両方を対象とする必要があります。

認証、事前認証、認証、認証、ユーザーメタデータ、ログアウトなど、TVE ビジネスフローに関連する問題が発生した場合は、Zendesk チケットを通じてカスタマーケアチームにレポートを提出することをお勧めします。

自己評価プランの作成については、以下のセクションを参照してください。

### Cookieの使用を監査する

Chrome 118以降、「[開発ツールの問題](https://developer.chrome.com/docs/devtools/issues/)」タブには、影響を受ける可能性のあるCookieが次のメッセージで表示されます：`Cookie sent in cross-site context will be blocked in future Chrome versions`。

サードパーティで使用するようにマークされたCookieは、`SameSite=None`属性値で識別できます。

詳しくは、このリンクを参照してください：https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### 破損のテスト

破損をテストするには、`--test-third-party-cookie-phaseout` コマンドラインフラグを使用してChromeを起動するか、Chrome 118から`chrome://flags/`で`#test-third-party-cookie-phaseout`を有効にします。

これにより、Google Chromeがサードパーティ Cookieをブロックするように設定され、フェーズアウト後の状態を最適にシミュレートするために、将来の機能がアクティブになります。

次のChrome フラグの技術仕様について深く掘り下げる価値があります。

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

詳しくは、このリンクを参照してください：https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## その他のブラウザー

### Firefox

Firefoxは数年前に`Enhanced Tracking Protection`というメカニズムを導入しました。

Firefoxの役に立つリソースを以下に示します：

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safariでは、数年前に「`Intelligent Tracking Prevention`」というメカニズムを導入しました。

Safariの便利なリソースを以下に示します。

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Adobe Passに関する次の資料をご確認ください。

* [トラッキング防止評価 – Apple Safari](tracking-prevention-assessment-apple-safari.md)
