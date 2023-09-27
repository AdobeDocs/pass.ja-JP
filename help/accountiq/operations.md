---
title: アカウント IQ の操作
description: アカウント IQ の操作には、購読者アカウントに対して自動化と一括操作を実行し、その効果を追跡するアクションを実行する必要があります。
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# 運用 {#operations-tab-next-steps}

選択したセグメント ( [!DNL Analytics] （アカウント IQ 内）では、目標に対してターゲット設定されたアクションを実行し、パスワードの共有を軽減できます。

アカウント IQ の Operations 機能は、操作と呼ばれる重点的な手順を通じて、資格情報の共有に効果的に取り組み、管理するのに役立ちます。 このツールでは、目的を設計し、特定の購読者アカウントのグループに合わせて（目標に基づいて）ターゲットアクションを調整し、将来の期間にわたって自動化するオプションを提供します。 操作機能を使用すると、操作の作成と実行だけでなく、その影響を測定することができます。 そのため、影響を調整することで、借り手の変換や資格情報の共有の軽減など、効果を最適化するための戦略を調整できます。

表示するには **運用** ページ選択 **運用** 下のオプション **アクション** （アカウント IQ アプリケーションの左側のナビゲーション） [ 操作 ] ページには、アカウント IQ システム上に既に存在するすべての操作とその詳細が表示されます。

![](assets/operations-page.png)

*図：アカウント IQ の既存の操作のリストと詳細*

「操作」ページでは、次の操作を実行できます。

* アカウント IQ に既に存在する操作のリストを表示します

* 次のような操作の詳細を表示します。

   * ステータス（スケジュール済み、実行中、終了、エラーまたは停止）

   * 進行状況（完了率）

   * ターゲットオーディエンス（操作を実行するセグメント）

   * スケジュール（操作の開始日と終了日）

   * 操作の作成日と終了日

* [新しい操作を作成](/help/accountiq/operation-affecting-user-segment.md)

* [操作レポートを表示](#operation-reports)

<!--* Search from the list of operations using Search field

* Stop an operation.

* Create a duplicate operation.

* [Configure columns of Operations details page](#configure-columns)-->

## 操作レポートを表示 {#operation-reports}

操作のレポートを表示して、操作の影響を分析できます。 工程のレポートを表示する手順は、次のとおりです。

1. メインの操作ページで操作名を選択します。

   レポートは、積み重ね列グラフの形式で表示されます。

   ![](assets/operation-impact-report.png)

   *図：操作レポートで操作の影響を確認*

   X 軸は評価期間を表し、y 軸は操作の影響を示します（評価期間中のセグメント内のアカウント数の観点から）。 各バーは 3 つの部分に分かれています。

   * 1 つの部分は、操作セグメントの条件を満たすアカウントの数を表します。

   * 別の部分は、元々セグメント内にあったが、操作セグメントの基準を満たさなくなった、その期間のアクティブなアカウントの数を表します。

   * 3 番目の部分は、その期間にアクティブでなかったアカウントを表します。

   >[!NOTE]
   >
   >最初のバーは、評価期間の最初に操作セグメントの条件を満たすアカウント数を表します。

   時間の経過と共に、元の条件に対する行動を変更した（例えば、共有の確率が 90 を超え、5 台以上のデバイスを使用する）アカウントの数、または非アクティブになったアカウントの数を示すことで、（操作を通じて）アクションの効果がグラフに表示されます。

<!--For example, in the above image the variable on the y-axis is number of accounts. Looking at the graph you can compare the number of accounts that are in the operations' segment versus the number of accounts that are outside the operations segment at a particular time (such as week 2nd of the operations evaluation period). Therefore, you can analyze how over the evaluation period do number of accounts vary within the operation segment and outside the segment.

So, if your operation was to send out warning emails to suspecting accounts, and accounts in operations segment were those with sharing probability more than 90 and using more than 5 devices to stream content, then in the beginning of the evaluation period accounts in segment are more than 17 thousand. This number changes over the evaluation period as shown in the graph, thereby indicating the impact of operation. Based on the evaluation, you can take remedial measures on suspecting accounts, or continue with the operation, or adjust your strategy for better outcomes to curb credential sharing.-->

1. レポートを閉じて、メインの操作ページに戻るには、「 **運用** 下のオプション **アクション** をクリックします。

<!--

![](assets/operations-details.png)

*Figure: Operation details*
## Configure columns {#configure-columns}

You can select the icon to **Configure columns** on the top of the operations table.

![](assets/config-columns.png)

*Figure: Configure columns of Operations details page*-->