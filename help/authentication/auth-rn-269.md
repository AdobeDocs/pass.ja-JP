---
title: Adobe Pass Authentication 2.69 リリースノート
description: Adobe Pass Authentication 2.69 リリースノート
source-git-commit: 4417c5c50873c73c615f9845cadd4a42c26f3068
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Adobe Pass Authentication 2.69 リリースノート {#pt-authn-269-rn}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバー側および Web クライアント {#server-side-web-clients-269}

* [ビルド番号](#build-number-269)
* [新機能](#new-features-269)

### ビルド番号 {#build-number-269}

Adobe Pass認証：adobe-pass-**2.69**
リリース日： **02/27/2024 - 02/29/2024**

### 新機能 {#new-features-269}

#### その他 {#misc}

* セキュリティの脆弱性にパッチを適用しました。
* 動的クライアント登録 (DCR) を使用した一時パスのリセットセキュリティレイヤーの機能強化。
   * 詳しくは、以下を参照してください。 [一時パスをリセット](reset-temp-pass.md)
* プラットフォーム識別レポートの機能強化。

#### REST API {#rest-apis}

* 新しい REST API の継続的な開発。
   * 今後の専用リリースでは、新しいエンドポイントとフローが導入され、それが別の通知で通知されます。
   * これらの新しい API を使用するためのドキュメントの更新が進行中です。

#### TVE ダッシュボード {#tve-dashboard}

* 新しい TVE ダッシュボードへの継続的な開発。
   * 今後の専用リリースでは、別の通知で発表される新しい TVE ダッシュボードが導入されます。
   * この新しい TVE ダッシュボードを使用するためのドキュメントの更新が進行中です。

#### JavaScript SDK 4.7.0 {#js-sdk}

* セキュリティの脆弱性が原因で、Access Enabler JavaScript SDK の廃止されたバージョン 2.0.1 が削除されました。
   * 詳しくは、次のリンクを参照してください。 [Adobe Pass Authentication JavaScript 4.7.0 リリースノート](authn-rn-javascript-470.md)
