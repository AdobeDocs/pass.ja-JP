---
title: トラッキング防止評価GoogleChrome
description: トラッキング防止評価GoogleChrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# （従来の）トラッキング防止評価 – Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要

このドキュメントでは、サードパーティ cookie を段階的に廃止する取り組みの一環として、役に立つリソースを集計し、Google Chromeで予定されている今後の変更を評価します。

この評価は、Adobe Pass Chrome ブラウザーで動作し、Google Access Enabler JavaScript SDK v4 を使用してAdobe Pass Authentication のバックエンドサービスと統合している、TV Everywhere （TVE）アプリケーションに対して実行されます。

## パブリックリソース

Googleの開発者向け web サイトと、アドビが推奨する公式ブログから集約されたリソースのリストを以下に示します。

* [Chromeでのサードパーティ cookie の廃止に向けた次のステップ &#x200B;](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [&#x200B; プライバシーサンドボックスに関する開発者向けドキュメント &#x200B;](https://developers.google.com/privacy-sandbox)
* [&#x200B; サードパーティ cookie 制限の準備 &#x200B;](https://developers.google.com/privacy-sandbox/3pcd)
* [&#x200B; サードパーティ cookie のフェーズアウトの準備 &#x200B;](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [&#x200B; サードパーティ cookie の提供終了に向けた準備 &#x200B;](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Chrome ユーザーの 1% に対して、デフォルトでサードパーティ Cookie が制限される &#x200B;](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## タイムライン

概要を簡単に説明するために、Google Chromeでは、すべてのサードパーティ cookie に影響を与えるクロスサイトトラッキングを制限する新機能である [&#x200B; トラッキング保護 &#x200B;](https://privacysandbox.com/) のテストを開始しました。

当初は 2024 年初めに開始し、ユーザーの約 1% に影響を与えており、2024 年第 3 四半期からユーザーの最大 100% に拡張する（暫定）計画です。

## 評価

Googleは、サードパーティ cookie の廃止に備えて推奨プレイブックを集計したドキュメントを次のリンクで公開しました：https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout。

Google Chrome ブラウザーで動作し、Adobe Pass Access Enabler JavaScript SDK v4 を使用してAdobe Pass Authentication のバックエンドサービスと統合している TV Everywhere （TVE）アプリケーションを評価する際には、このプレイブックを採用しています。

### 結論

アドビのテストに基づき、今後のGoogle Chromeのアップデートをシミュレートすると、主要な TVE ビジネスフローは **引き続き期待どおりに機能します** です。

ただし、Googleの幅広い戦略を認識することが重要です。この戦略では、サードパーティ cookie を廃止するだけでなく、サードパーティのストレージを分割することもできます。

その結果、Chrome ユーザーは、シングルサインオン（SSO）、シングルログアウト（SLO）、パッシブ認証機能で混乱し、使用する TVE アプリケーションごとに個別のログイン/ログアウトアクションが必要になります（Safari の現在のエクスペリエンスに合わせて）。

## 自己評価の呼び出し

GoogleChromeの利用者の経験を見直し、課題を事前に洗い出し、理解を深めていただくためにも、同様の検討を積極的に行っていただくことを求めます。

この評価には、特にAdobe Pass Access Enabler JavaScript SDK v4 の統合に関して、ファーストパーティのサービスとサードパーティのサービスの両方を含める必要があります。

認証、事前認証、認証、ユーザーメタデータ、ログアウトなど、TVE ビジネスフローに関連する問題が発生した場合は、カスタマーケアチームの Zendesk チケットを通じてレポートを提出することをお勧めします。

自己評価プランの作成については、以下のセクションを参照してください。

### Cookie の使用の監査

Chrome 118 以降の「[DevTools の問題 &#x200B;](https://developer.chrome.com/docs/devtools/issues/)」タブでは、影響を受ける可能性のある Cookie がハイライト表示され、次のメッセージが表示されます。`Cookie sent in cross-site context will be blocked in future Chrome versions`

サードパーティで使用するためにマークされた Cookie は、`SameSite=None` 属性値で識別できます。

詳しくは、https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookiesを参照してください。

### 破損のテスト

破損のテストを行うには、`--test-third-party-cookie-phaseout` コマンドラインフラグを使用するか、Chrome 118 からChromeを起動して、`chrome://flags/` で `#test-third-party-cookie-phaseout` を有効にします。

これにより、Google Chromeがサードパーティ cookie をブロックするように設定され、フェーズ アウト後の状態を最適にシミュレーションするために、今後の機能がアクティブになります。

次のChrome フラグの技術仕様を詳しく調べる価値があります。

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

詳しくは、https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakageを参照してください。

## その他のブラウザー

### Firefox

Firefox は、数年前に `Enhanced Tracking Protection` と呼ばれるメカニズムのロールアウトを行いました。

Firefox の役立つリソースの一部を以下に示します。

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari は彼らのメカニズムのロールアウトを呼び出しました：数年前に `Intelligent Tracking Prevention`。

Safari の便利なリソースの一部を以下に示します。

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Adobe Passの役立つリソースの一部を以下に示します。

* [トラッキング防止の評価 – Apple Safari](tracking-prevention-assessment-apple-safari.md)
