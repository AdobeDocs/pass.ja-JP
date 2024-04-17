---
title: Adobe Pass Authentication 2.70 リリースノート
description: Adobe Pass Authentication 2.70 リリースノート
source-git-commit: 30e2507c0622b882744cf8291525c388575df32f
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication 2.70 リリースノート {#pt-authn-270-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-270}

* [ビルド番号](#build-number-270)
* [新機能](#new-features-270)

### ビルド番号 {#build-number-270}

Adobe Pass認証：adobe-pass-**2.70**
リリース日： **2024/4/23 - 2024/4/25**

### 新機能 {#new-features-270}

#### その他 {#misc}

* パッチが適用されたセキュリティ脆弱性。
* API サービスの低下を改善しました。
   * Degradation API のセキュリティメカニズムとして DCR を使用します。
   * 詳しくは、こちらを参照してください。 [低下 API の概要](degradation-api-overview.md)

#### REST API {#rest-apis}

* 新しい REST API への継続的な開発。
   * 今後の専用リリースでは、新しいエンドポイントとフローが導入され、別の通知で発表される予定です。
   * これらの新しい API を使用するためのドキュメントの更新が進行中です。
