---
title: Adobe Pass Authentication 2.65.1 リリースノート
description: Adobe Pass Authentication 2.65.1 リリースノート
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Adobe Pass Authentication 2.65.1 リリースノート {#authn-2651-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-2651}

* [ビルド番号](#build-number-2651)
* [リリースの概要](#release-overview-2651)

### ビルド番号 {#build-number-2651}

Adobe Pass認証：adobe-pass-**2.65.1**
リリース日：**2023/6/20 - 2023/6/22**

### リリースの概要 {#release-overview-2651}

このリリースでは、次の機能が追加されました。

このリリース以降、エコシステム全体を誤った使用から保護するために、すべての API にわたってレート制限ルールを追加するためのテクニカルサポートを導入します。

最初の目標は、アプリケーションの動作を監視して適切な制限値を設定することです。このリリースでは、制限値は適用されません。 特定のアプリケーションによって生成されたトラフィックが特定の制限を超えている場合、アドビは各顧客と協力して実装を検証します。

レート制限ルールは、すべてのお客様のアプリケーションから生成されたトラフィックを検証した後、今年後半に適用されます。
