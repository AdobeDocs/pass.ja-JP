---
title: Adobe Pass Authentication 3.5.0 リリースノート
description: このリリースの新機能、変更点、既知の問題について説明します。
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%
---
# Adobe Pass Authentication 3.5.0 リリースノート

最終更新日：2025年12月9日（火） 00:00:00 GMT+0000 （協定世界時）

* トピック：
* 認証

>[!IMPORTANT]
>
> [製品のお知らせ](https://experienceleague.adobe.com/ja/docs/pass/authentication/product-announcements) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## サーバーサイドおよびWeb クライアント {#server-side-web-clients-350}

* [ビルド番号](#build-number-350)
* [リリースの概要](#release-overview-350)

### ビルド番号 {#build-number-350}

Adobe Pass認証：adobe-pass-**3.5.0**\
リリース日：**12/09/2025 - 12/11/2025**

### リリースの概要 {#release-overview-350}

#### REST API v2

* ESM （「api」）に新しいディメンションを追加し、実装タイプ（SDK、REST V1、REST V2）に基づいてイベントを区別するレポートを作成できるようにしました。

#### バグ修正

* TVE ダッシュボードで設定されたカスタム劣化メッセージがREST API V2 エラーの詳細に表示されない問題を修正しました。
* 認証されたプロファイルの有効期限が切れたときに`authenticated_profile_expired` エラーコードが返されないREST API V2の問題を修正しました。
* REST API V2で承認待ち時間の計算とプリフライト TTL値が正しくない問題を修正しました。
* DCR トークンの有効期限が切れたときに一貫しない応答形式が返される問題を修正しました。

## メンテナンス更新 – 2026年2月 {#maintenance-update-february-2026}

Adobe Pass認証：adobe-pass-**3.5.0.5**\
リリース日：**02/24/2026 - 02/26/2026**

このメンテナンスアップデート リリースには、システムの信頼性とセキュリティを強化するための重要な改善点が含まれています。

### 機能強化

* REST API V2のプロキシ化されたMVPD設定の認証劣化の処理が改善され、MVPD サービスの中断時により一貫性のある動作が保証されました。
* セキュリティ制御を強化し、システム全体の整合性を向上させるために、URL パラメーターの検証とリダイレクト処理を強化しました。
