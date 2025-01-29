---
title: Adobe Pass Authentication 2.69 リリースノート
description: Adobe Pass Authentication 2.69 リリースノート
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Adobe Pass Authentication 2.69 リリースノート {#pt-authn-269-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-269}

* [ビルド番号](#build-number-269)
* [新機能](#new-features-269)

### ビルド番号 {#build-number-269}

Adobe Pass認証：adobe-pass-**2.69**
リリース日：2024 年 2 月 27 日（**）～2024 年 2 月 29 日（PT）**

### 新機能 {#new-features-269}

#### その他 {#misc}

* パッチが適用されたセキュリティ脆弱性。
* Dynamic Client Registration （DCR）による一時パスセキュリティレイヤーのリセットの機能強化。
   * 詳しくは、こちらを参照してください。[TempPass 機能 ](../integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
* プラットフォーム識別レポートの機能強化。

#### REST API {#rest-apis}

* 新しい REST API への継続的な開発。
   * 今後の専用リリースでは、新しいエンドポイントとフローが導入され、別の通知で発表される予定です。
   * これらの新しい API を使用するためのドキュメントの更新が進行中です。

#### TVE ダッシュボード {#tve-dashboard}

* 新しい TVE ダッシュボードへの継続的な開発。
   * 今後の専用リリースでは、新しい TVE ダッシュボードが導入されます。これは別の通知で発表されます。
   * この新しい TVE ダッシュボードを使用するためのドキュメントの更新を処理中です。

#### JavaScript SDK 4.7.0 {#js-sdk}

* セキュリティの脆弱性により、Access Enabler JavaScript SDKの非推奨バージョン 2.0.1 を削除しました。
   * 詳しくは、[Adobe Pass Authentication JavaScript 4.7.0 リリースノート ](authn-rn-javascript-470.md) のリンクを参照してください。
