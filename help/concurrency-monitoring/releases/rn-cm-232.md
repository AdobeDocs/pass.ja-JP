---
title: Adobe Pass同時実行性モニタリング 2.3.2 リリースノート
description: Adobe Pass同時実行性モニタリング 2.3.2 リリースノート
exl-id: 3996da45-498c-482a-b374-3cda1c5df2f7
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 1%

---

# Adobe Pass同時実行性モニタリング 2.3.2 リリースノート {#cm-232}

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

リリース日：2015 年 12 月 11 日（Pt）

## 新機能と機能強化 {#new-features}

* 使用状況レポートで新しい分類を使用できます。 同時実行の監視と統合されたアプリケーションがカスタムメタデータを送信する場合に、新しい分類を使用できます。
   * application – 呼び出し URL でレポートされるアプリケーション ID
   * mvpd – 呼び出し URL で報告されるMVPD
   * チャネル – カスタムメタデータチャネル
   * platform - カスタムメタデータ applicationPlatform
* **ストリーム時間** に関連する新しい指標が使用状況レポートで使用できます。 新しい指標を使用して、ストリーム時間のヒストグラムを作成できます。 現在、次の間隔（分単位）を使用できます。
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## バグの修正 {#bug-fixes}

該当なし

## 既知の問題 {#known-issues}

該当なし
