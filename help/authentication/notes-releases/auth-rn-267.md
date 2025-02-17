---
title: Adobe Pass Authentication 2.67 リリースノート
description: Adobe Pass Authentication 2.67 リリースノート
exl-id: d899fe96-a273-4681-90a5-bde54cc2f3b3
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 0%

---

# Adobe Pass Authentication 2.67 リリースノート {#authn-267-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-267}

* [ビルド番号](#build-number-267)
* [リリースの概要](#release-overview-267)

### ビルド番号 {#build-number-267}

Adobe Pass認証：adobe-pass-**2.67.0.1**

リリース日：**09/12/2023 - 9/14/2023**

### リリースの概要 {#release-overview-267}

* 新しい REST API の内部アップデートを継続しました。
* 内部アーキテクチャの改善を継続。

#### MVPDの更新

* **DirecTV プエルトリコ** とAdobeの統合に関する更新。 詳細は TAM にお問い合わせください。

#### バグの修正

* REST API を使用するアプリケーションと FireTV SDKを使用するアプリケーションの間で、Adobe-Subject-Token ヘッダーを使用して取得される SSO が機能しなくなる問題を修正しました。
