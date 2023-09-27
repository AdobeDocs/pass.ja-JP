---
title: MVPD および選択したプログラマの指標を書き出す
description: MVPD および選択したプログラマの指標を書き出す
exl-id: 868016ec-71aa-44b9-a002-0d124a64c167
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 0%

---

# MVPD および選択したプログラマの指標を書き出す {#export-metric}

ダッシュボード ( [!UICONTROL Account IQ] 選択したセグメント内の購読者アカウントの秘密鍵証明書共有統計の表とグラフが表示されます。 共有パターンとスコアの表示に加えて、選択したセグメントの MVPD およびチャネルの購読者のアカウント使用状況指標と共有スコアを、これらのテーブルから書き出すこともできます。

MVPD および選択したプログラマーの指標を書き出すには、許可された MVPD ユーザーとしてログインした後に、次の手順を実行します。

1. 次の手順に従って、目的のセグメントを定義します。 [セグメントを定義して期間を選択する方法](/help/accountiq/howto-select-segment-timeframe.md) 評価のために [セグメントと期間](/help/accountiq/segments-timeframe.md) パネル。

1. 次のパネルの 1 つに移動します。

   * [!UICONTROL Programmers in segment]
     ![](assets/prog-segment-export-option.png)

   * [!UICONTROL Number of accounts and usage by sharing probability level]

     ![](assets/progr-usage-panel-export.png)

1. 選択 **[!UICONTROL Export]** オプションは、パネルの右上隅にあります。

データは CSV 形式で書き出され、ファイルがデバイス上にローカルにダウンロードされます。 目的の CSV ビューアおよびエディターを使用して、書き出されたレポートを開くことができます。

* セグメント内のプログラマー

  ![](assets/export-progr-in-seg.png)


* 共有の確率レベル別のアカウント数と使用状況

  ![](assets/export-acc-usage.png)
