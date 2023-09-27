---
title: Adobe Pass Concurrency Monitoring 2.3.2 リリースノート
description: Adobe Pass Concurrency Monitoring 2.3.2 リリースノート
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Adobe Pass Concurrency Monitoring 2.3.2 リリースノート {#cm-232}

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

リリース日： 12/11/2015

## 新機能および改善点 {#new-features}

* 使用状況レポートで使用できる新しい分類。 同時実行監視と統合されたアプリケーションがカスタムメタデータを送信する場合は、新しい分類が使用可能です。
   * application — 呼び出し URL で報告されるアプリケーション ID。
   * mvpd — 呼び出し URL で報告される MVPD
   * channel — カスタムメタデータチャネル
   * platform — カスタムメタデータ applicationPlatform
* 次に関連する新しい指標： **ストリーム時間** が使用可能になる問題を修正しました。 新しい指標を使用して、ストリームの時間のヒストグラムを作成できます。 現在、次の間隔（分単位）を使用できます。
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## バグの修正 {#bug-fixes}

該当なし

## 既知の問題 {#known-issues}

該当なし
