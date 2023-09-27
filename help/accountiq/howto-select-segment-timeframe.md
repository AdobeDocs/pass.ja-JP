---
title: セグメントと時間枠の定義
description: セグメントと時間枠の定義
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# セグメントと時間枠の定義 {#define-segment}

でのすべての分析またはレポートの表示 [!UICONTROL Account IQ] まず、セグメントを定義し、評価の時間枠を選択します。 [セグメント](/help/accountiq/product-concepts.md#segmet-def) は、評価の条件（MVPD を購読し、特定のチャネルを表示する）を満たすすべての購読者またはビューアを指します。

![](assets/segment-panel.png)

*図：セグメントと時間枠の選択*

のすべてのレポートページの上部 [!UICONTROL Account IQ]を使用する場合、MVPD、チャネルプログラマー、精度と時間枠を選択してセグメントを定義するパネルがあります。

## セグメントの選択 {#select-segment}

### セグメント内の MVPD を選択 {#select-segment-mvpds}

MVPD を次の中から選択するには： **[!UICONTROL MVPDs in segment]** オプション：

1. クリックまたはタップ **[!UICONTROL MVPDs in segment]** ドロップダウンオプション。

   >[!NOTE]
   >
   >**すべて** 業界 MVPD がデフォルトで選択されています。 ここから、次のいずれかを選択できます。 **スコアの共有による上位 10 件の MVPD**, **使用別上位 10 件の MVPD**, **アカウント別上位 10 件の MVPD**、または個々の MVPD。 ただし、個々の MVPD を選択するには、選択を解除する必要があります **すべて**.

1. 目的の MVPD をクリックまたはタップします。

   MVPD の選択を解除すると、選択から MVPD を削除できます。

1. クリックまたはタップ **[!UICONTROL Apply selection]** 選択を有効にするために。 そうしないと、作成した選択が解放されます。

   >[!NOTE]
   >
   >[ 分離モード ] を選択した場合、他の MVPD は選択できません。

### セグメント内のチャネルを選択 {#select-segment-channels}

目的のプログラマーチャネルを **[!UICONTROL Channels in segment]** オプション：

1. クリックまたはタップ **[!UICONTROL Channels in segment]** ドロップダウンオプション。

   >[!NOTE]
   >
   >**すべて** 会社のプログラマーチャネルがデフォルトで選択されています。 個々のチャネルまたはプログラマーを選択するには、まず選択を解除する必要があります **すべて**.

1. 目的のチャネルまたはプログラマーをクリックまたはタップします。

   次の項目の最上位のリスト項目： **[!UICONTROL Channels in segment]** が [プログラマー](/help/accountiq/product-concepts.md#programmer-def) プログラマー名の下の企業とリスト項目が、その企業名です。 [channels](/help/accountiq/product-concepts.md#channel-def). プログラマの下で個々のチャネルを選択するか、プログラマの下でのチャネルのすべてのアクティビティを選択してレポートとグラフの結果に含めることができます。

   ![](assets/programmer-channels.png)


   *図：チャネルセレクターに表示されたプログラマーとチャネル*

   >[!IMPORTANT]
   >
   >プログラマの下で個々のチャネルを選択する結果は、プログラマを選択する結果とは異なります。
   >
   >
   >個々のチャネルを選択する場合、一部のレポートでは、これらのチャネルのアクティビティが個別に分類されます。 ただし、これらのチャネルの親プログラマーを選択すると、これらのチャネルのすべてのアクティビティが含まれますが、レポートで個別に分類されることはありません。

1. クリックまたはタップ **[!UICONTROL Apply selection]** 選択を有効にするために。

>[!NOTE]
>
>MVPD またはプログラマーのプルダウンメニューでは、10 個を超える項目を選択することはできません。

### MVPD とチャネルの選択を解除 {#deselect-segment-mvpds-channels}

選択内容を変更する以外に、 **[!UICONTROL MVPDs in segment]** および **[!UICONTROL Channels in segment]** セグメントセレクターを使用する場合、以下の方法で、以前に選択した MVPD およびチャネルの選択を解除できます。

* の選択 **[!UICONTROL Remove]** アイコン (![アイコンを削除](assets/remove-icon.png)) が、選択した MVPD およびチャネルの名前に対してセグメントセレクターの下に表示されます。

* また、 **[!UICONTROL Clear Selection]** をクリックして、以前に選択したすべての MVPD またはチャネルを削除します。

![](assets/segment-panel-selection.png)

*図：セグメントおよび期間パネルで選択された MVPD およびチャネル*

## 精度と時間枠の選択 {#granularity-timeframe}

評価期間を選択する手順は、次のとおりです。

1. を選択します。 **[!UICONTROL Granularity and time frame]** 日付選択。

1. 次のいずれかを選択 **[!UICONTROL Week]** または **[!UICONTROL Month]** から **[!UICONTROL Aggregate By]** オプションを使用して、評価の精度を設定します。

   ![](assets/granularity-timeframe-weekwise.png)


   *図：精度と時間枠を選択する日付選択ツール*

1. 精度を選択したら、前方向または後方向の矢印を使用して、時間の経過に合わせて前方または後方に移動できます。

1. 評価の期間を過去（選択した精度に基づいて月または週）に指定します。

1. 選択 **[!UICONTROL Apply Selection]** 選択を確実に有効にするには、をクリックします。
