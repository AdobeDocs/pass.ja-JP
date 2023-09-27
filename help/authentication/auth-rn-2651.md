---
title: Adobe Pass Authentication 2.65.1リリースノート
description: Adobe Pass Authentication 2.65.1リリースノート
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Adobe Pass Authentication 2.65.1リリースノート {#authn-2651-rn}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバー側および Web クライアント {#server-side-web-clients-2651}

* [ビルド番号](#build-number-2651)
* [リリースの概要](#release-overview-2651)

### ビルド番号 {#build-number-2651}

Adobe Pass認証：adobe-pass-**2.65.1**
リリース日： **06/20/2023 - 06/22/2023**

### リリースの概要 {#release-overview-2651}

このリリースでは、次の機能が追加されました。

このリリースからは、エコシステム全体を誤った使用から保護するために、すべての API に対してレート制限ルールを追加するためのテクニカルサポートを導入します。

最初の目標は、適切な制限値を確立するためにアプリケーションの動作を監視することで、このリリースの一部として制限は適用されません。 特定のアプリケーションで生成されたトラフィックが特定の制限を超える場合は、各お客様と連携して実装を検証します。

レート制限ルールは、すべてのお客様のアプリケーションから生成されたトラフィックを検証した後、今年後半に適用されます。
