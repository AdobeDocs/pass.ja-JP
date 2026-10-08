---
title: Adobe Concurrency Monitoring 2.9 リリースノート
description: Adobe Concurrency Monitoring 2.9 リリースノート
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 3%
---
# 同時実行モニタリング 2.9 リリースノート {#rn-cm29}

このページでは、このリリースの新機能、変更点、既知の問題について説明します。

## リリース日 {#release-date}

03/14/2019


## リリースの概要 {#release-overview}

* このバージョンから、同時使用を理解するための新しいレポートが導入されました。同時使用レベルのヒストグラムは、次のように表示されます。

* 各粒度区間で各同時実行レベルに達したユーザーの数（つまり、2つの同時ストリームや3つの同時ストリームを持ったことがあるユーザーの数）
* 同時実行レベルごとの合計時間（分単位）（平均値は、この値を上記の数で割るだけで計算できます）
* 同時実行レベルごとにユーザーが発生した合計回数。影響を受けるユーザーと集計されたユーザーエクスペリエンスの両方で、特定のルールの影響を推定します
詳細については、[利用状況レポート &#x200B;](/help/concurrency-monitoring/reports/cm-usage-reports.md) ページを参照してください。

また、SQL インジェクションの保護を改善し、いくつかのバグ修正を追加しました。

## 既知の問題 {#known-issues}

なし。
