---
title: 追跡防止評価Google Chrome
description: 追跡防止評価Google Chrome
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# トラッキング防止評価 — Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要

このドキュメントは、有用なリソースを集約し、サードパーティ Cookie を段階的に廃止する取り組みの一環として、Google Chrome が予定している今後の変更を評価します。

この評価は、Google Chrome ブラウザーで実行され、Adobe Pass Access Enabler JavaScript SDK v4 を使用してAdobe Pass Authentication バックエンドサービスと統合している TV Everywhere(TVE) アプリケーションに対して実行されます。

## パブリックリソース

Googleの開発者向け Web サイトや公式ブログから集計したリソースのリストを以下に示します。このブログは、お客様に相談を勧めるものです。

* [Chrome でサードパーティ Cookie を段階的に廃止するための次の手順](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [プライバシーサンドボックスの開発者向けドキュメント](https://developers.google.com/privacy-sandbox)
* [サードパーティ Cookie の制限に対する準備](https://developers.google.com/privacy-sandbox/3pcd)
* [サードパーティ Cookie のフェーズアウトの準備](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [サードパーティ Cookie の終了の準備](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Chrome ユーザーの 1%に対し、デフォルトで制限されるサードパーティ cookie](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## タイムライン

簡単な概要として、Google Chrome のテスト開始時 [追跡保護](https://privacysandbox.com/)：すべてのサードパーティ cookie に影響するクロスサイトトラッキングを制限する新機能。

最初は、2024 年の初めに開始され、ユーザーの約 1%に影響を与えます。（暫定的な）計画は、2024 年の第 3 四半期以降、最大 100%のユーザーに対してこれを拡張する予定です。

## 評価

Googleは、サードパーティ cookie フェーズアウトに備えて、推奨されるプレイブックをまとめたドキュメントを、 https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseoutのリンクで公開しました。

Google Chrome ブラウザーで動作し、Adobe Pass Access Enabler JavaScript SDK v4 を使用してAdobe Pass認証バックエンドサービスと統合している TV Everywhere(TVE) アプリケーションの評価については、このプレイブックに従いました。

### 結論

アドビのテストに基づき、主な TVE ビジネスフローであるGoogle Chrome の今後のアップデートをシミュレートします **は引き続き期待どおりに機能します**.

ただし、Googleの広範な戦略を認識することが重要です。これには、サードパーティ Cookie の廃止だけでなく、サードパーティストレージのパーティション分割も含まれます。

その結果、Chrome ユーザーは、シングルサインオン (SSO)、シングルログアウト (SLO) およびパッシブ認証機能で中断が発生し、使用する TVE アプリケーションごとに別々のログイン/ログアウトアクションが必要になります（Safari での現在の操作と一致）。

## 自己評価を求める

当社は、お客様に対し、同様の評価を事前に行い、潜在的な問題を十分に特定し、改訂されたGoogle Chrome ユーザーエクスペリエンスに慣れることを強くお勧めします。

この評価は、特にAdobe Pass Access Enabler JavaScript SDK v4 統合に関して、ファーストパーティのサービスとサードパーティのサービスの両方を対象とする必要があります。

認証、事前認証、承認、ユーザーメタデータ、ログアウトなど、TVE ビジネスフローに関する問題が発生した場合は、Zendesk チケットを通じてカスタマーケアチームとレポートを提出するようお勧めします。

自己評価計画の策定に関するサポートについては、以下の節を参照してください。

### cookie の使用の監査

Chrome 118 以降、 [開発ツールの問題](https://developer.chrome.com/docs/devtools/issues/) 「 」タブで、影響を受ける可能性のある cookie が次のメッセージで強調表示されます。 `Cookie sent in cross-site context will be blocked in future Chrome versions`.

サードパーティの使用としてマークされた cookie は、 `SameSite=None` 属性値。

詳しくは、次のリンクを参照してください。 https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### 破損のテスト

破損をテストするには、次を使用して Chrome を起動します。 `--test-third-party-cookie-phaseout` コマンドラインフラグまたは Chrome 118 から有効 `#test-third-party-cookie-phaseout` in `chrome://flags/`.

これにより、サードパーティ Cookie をブロックするGoogle Chrome が設定され、フェーズアウト後の状態を最適にシミュレートするために、将来の機能がアクティブになるようになります。

以下の Chrome フラグの技術仕様を詳しく見てみるとよいでしょう。

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

詳しくは、次のリンクを参照してください。 https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## その他のブラウザー

### Firefox

Firefox には、と呼ばれるメカニズムがロールアウトされていました。 `Enhanced Tracking Protection` 2 年前に

Firefox の役に立つリソースを以下に示します。

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari には、と呼ばれるメカニズムがロールアウトされていました。 `Intelligent Tracking Prevention` 2 年前に

Safari の役に立つリソースを以下に示します。

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Adobe Passの役に立つリソースを以下に示します。

* [追跡防止の評価 — Apple Safari](tracking-prevention-assessment-apple-safari.md)
