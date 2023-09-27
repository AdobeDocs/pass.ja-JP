---
title: アカウント IQ のベストプラクティス
description: 詳細 [!UICONTROL Account IQ] Adobe Passで
exl-id: 55901f5d-d9c1-4ee5-9349-afa28445b324
source-git-commit: ffc7caa9a857a60380d15f69abf346718de6fa4c
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# ベストプラクティス {#aiq-best-practices}

[!UICONTROL Account IQ] を使用すると、資格情報の共有を識別し、その範囲と影響を測定し、関連するコホートをセグメント化して、ターゲットアクションの影響を追跡できます。 様々な方法で使用できる共有アカウントを理解および管理するための様々なツール、グラフ、レポートが用意されています。 各ストリーミングサービスは、この動作に対応し、独自の目標に合ったプロセスを開発し、そのニーズに柔軟に対応できるように設計されています。  ただし、一般的な慣行には、様々なシナリオに適用できるルールがいくつかあります。

## 分析と理解 {#analyze-understand}

[!UICONTROL Account IQ] ツールは、共有アカウントの割合を表示する最上位レベルのグラフから、個々のアカウントの特性を報告する最下位レベルの書き出しまで、ビジネス上の資格情報共有の特性と範囲を理解するのに役立ちます。 特に、最初は、これらのツールを使用してデータを調べ、例えば高いレベルの共有を示す、異常で興味深いコホートや行動パターンを特定します。 その後、特定のオポチュニティまたは目標を表すセグメントを識別できます。

共有がビジネスに与える影響と特性を理解するには：

* まず、関連する可能性のあるすべてのデータを確認します。

* 目標のコンテキストで共有を特定し、分析します。

* ターゲットとするパターンおよび動作を特定します。

## ターゲットを絞った増分処理アクションを実行する {#incremental-targeted-action}

定義済みのターゲットセグメントで、アクションを実行します。 明確に定義されたセグメントから小さなセグメントを開始すると、意図しない結果が生じるリスクを軽減し、結果をより深く理解できます。 パスのどこにいても、操作のターゲットを調整または拡張できます。
小さく立ち上がるには、慎重なアプローチです。 以前に識別したセグメントを使用し、特定の目的に対して（実験として）アクションを実行します。

操作ツールを使用して、ターゲットセグメントと運用期間を定義します。 これにより、次のフェーズでエフェクトを追跡できます。

* 行動の対象となる、明確に定義された代表的なユーザーセット（またはサブセット）を特定します。

* ターゲットセグメントと操作の期間を定義する操作を作成します。

* アプリ内オファー、追加広告、多要素認証の必要など、アップセル、広告読み込みの増加、不正アカウントへのアクセスの軽減などの目標に応じて、定義済みのユーザーセットに対して関連するアクションを実行します。

<!--If necessary, gauge the affect [by measuring the impact of actions taken](#track-measure-impact).-->

## アクションの影響の追跡と測定 {#track-measure-impact}

アクションを実行するには時間が必要です。 操作と関連グラフを使用して、操作期間の後続の週または数ヶ月にわたって、操作状態とセグメントの特性を追跡します。 この情報を他の分析と組み合わせて、結果に対する回答や理解を行います。 例えば、購読者にコンバートした借り手の割合は？ その他に何個の広告が表示されましたか？ 借り手の数は減少したか。

* 操作図やその他の分析を使用して、アクションの効果を追跡し測定します。

## 改善と繰り返し {#improve-repeat}

実験の結果と一連のユーザーに対するターゲット化されたアクションに基づいて、戦略の範囲を広げたり、アプローチを調整またはリセットしたりできます。

* 実験の結果が良好な場合は、実験またはアクションをスケールアップし、大きなグループに対してこれらのアクションを繰り返すことができます。

* 実験の結果が良くない場合は、アクションまたはセグメントを調整できます。

<!--

Best Practices
[!UICONTROL Account IQ] enables you to maximize your business ROI, and eventually grow your subscribers and revenue by understanding subscriber usage patterns and password sharing. Read on to know how you can make the best use of [!UICONTROL Account IQ] to manage credential sharing.

Analyze and understand
Authorized access of streaming services generates vast sums of data representing user activity. Use [!UICONTROL Account IQ] analytics tools to explore the data and identify interesting cohorts or behavioral patterns that indicate sharing. Then, segments representing a particular opportunity or objective can be identified.

To understand nature and impact of sharing on your business:

Use [!UICONTROL Account IQ] to access all relevant data.

Identify and analyze sharing in the context of your objectives.

Identify patterns and behavior to target.

Take targeted incremental action
To start small and ramp up is a prudent approach. Use previously identified segments, and take actions (as experiments) with specific objectives.

Identify a well-defined, representative subset of users in the segment to act on.

Depending on objectives such as upselling, increasing ad load, or mitigating access to fraudulent accounts, take relevant actions to include customer messaging or offers, extra ads, or requiring multi-factor authentication.

Target users are likely to respond to offers to upgrade and pay for sharing.

Align enterprise stakeholders to update strategy, such as:

Revisit partner agreements to enlist cooperation or concessions.

Simplify access and enhance the user experience for good customers.

Mitigate sharing by limiting access to obvious moochers.

If necessary, gauge the affect by measuring the impact of actions taken.

Track and measure the impact of actions
Once you have acted on some set of users within a segment, it is important to measure the effect of those actions over a subsequent period of weeks or months. For example, you would want to understand:

What percentage of borrowers converted to subscribers?

How many additional ads were viewed?

Did the number of borrowers decrease?

[!UICONTROL Account IQ]'s sophisticated machine learning based models help you analyze and measure the impacts of your experiments (or actions).

Improve and repeat
Based on the outcomes of your experiments and targeted actions on small groups of users, you can expand the reach of your strategies to rest of the user segment or reset the strategy and audience to act on.

Based on the usage insights from risk indices, sharing levels, and usage patterns, you can create experiments (or operations) and tailor your actions for strategic goals or desired outcomes.

If the results of the experiment are favorable, then you can scale up the experiment, and repeat those actions on a larger group.

If the results of the experiment are unfavorable, then you can adjust your action or the experiment group.

Therefore, understanding, acting, and tracking are the keys to optimally mitigate and manage credential sharing in your subscribers.
-->
