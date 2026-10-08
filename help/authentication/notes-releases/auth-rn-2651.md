---
title: Adobe Pass Authentication 2.65.1 リリースノート
description: Adobe Pass Authentication 2.65.1 リリースノート
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%
---
# Adobe Pass Authentication 2.65.1 リリースノート {#authn-2651-rn}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-2651}

* [ビルド番号](#build-number-2651)
* [リリースの概要](#release-overview-2651)

### ビルド番号 {#build-number-2651}

Adobe Pass認証：adobe-pass-**2.65.1**

リリース日：**06/20/20 - 06/22/2023**

### リリースの概要 {#release-overview-2651}

このリリースでは、次の機能が追加されています。

このリリースから、エコシステム全体を誤った使用から保護するために、すべてのAPIにレート制限ルールを追加するためのテクニカルサポートを導入します。

最初の目標は、アプリケーションの動作を監視して、適切な制限値を設定することであり、このリリースの一部として制限は適用されません。 特定のアプリケーションによって生成されたトラフィックが特定の制限を超える場合は、各顧客と協力して実装を検証します。

レート制限ルールは、すべての顧客のアプリケーションから生成されたトラフィックを検証した後、今年後半に適用されます。
