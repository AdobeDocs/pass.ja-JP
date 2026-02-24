---
title: Adobe Pass Authentication 3.5.0 リリースノート
description: このリリースの新機能、変更点、既知の問題について説明します。
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Adobe Pass Authentication 3.5.0 リリースノート

最終更新日：2025 年 12 月 9 日（火） 00:00:00 GMT+0000 （協定世界時）

* トピック：
* 認証

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-350}

* [ビルド番号](#build-number-350)
* [リリースの概要](#release-overview-350)

### ビルド番号 {#build-number-350}

Adobe Pass認証：adobe-pass-**3.5.0**\
リリース日：**年 12 月 9 日（PT）～2025 年 12 月 11 日（PT）**

### リリースの概要 {#release-overview-350}

#### REST API v2

* ESM （「api」）に新しいディメンションが追加され、実装タイプ（SDK、REST V1、REST V2）に基づいてイベントを区別するレポートを作成できるようになりました。

#### バグの修正

* TVE ダッシュボードで設定されたカスタム低下メッセージが、REST API V2 エラーの詳細に表示されない問題を修正しました。
* REST API V2 で、認証済みプロファイルの有効期限が切れたときにエラーコード `authenticated_profile_expired` 返されなかった問題を修正しました。
* REST API V2 で、認証待ち時間の計算とプリフライト TTL 値が正しくない問題を修正しました。
* DCR トークンの有効期限が切れると、一貫性のない応答形式が返される問題を修正しました。

## メンテナンス更新 – 2026 年 2 月 {#maintenance-update-february-2026}

Adobe Pass認証：adobe-pass-**3.5.0.5**\
リリース日：2026 年 2 月 24 日（**）～2026 年 2 月 26 日（PT）**

このメンテナンスアップデートリリースには、システムの信頼性とセキュリティを強化するための重要な機能強化が含まれています。

### 機能強化

* REST API V2 でプロキシ化されたMVPD設定の認証低下処理を改善し、MVPD サービスの中断時の動作の一貫性を向上しました。
* URL パラメーターの検証とリダイレクト処理の機能を強化して、セキュリティ制御を強化し、システム全体の整合性を向上しました。
