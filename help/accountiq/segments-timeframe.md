---
title: 購読者のセグメントと時間間隔
description: コホートを定義するか、購読者セグメントを選択して、チャネル閲覧者が Account IQ のグラフィカルツールとレポートを使用できるように、アカウント共有の可能性とパターンを測定します。
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: 6b790728f3d6a8eed5dfc0f8b3d0dad283af6418
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---


# 購読者のセグメントと時間間隔 {#cohorts-segments}


アカウント IQ にログインすると、上部のセグメントランチャーパネルで購読者を指定できます [セグメント](/help/accountiq/product-concepts.md#segment-segmet-def). これにより、購読者の共有動作とパターンに関するレポートを表示する際に、結果をフィルタリングできます。 プロパティの「すべてのアカウント」という名前のデフォルトのセグメントが既に選択されています。セグメントランチャーには次のオプションが表示されます。

![](assets/new-segment-selector-collapsed.png){width="800" align="left"}

*図：折りたたまれたセグメント概要を含むセグメントランチャー*

**A** 現在選択されているセグメント名<br/>
**B** 時間間隔と精度の選択<br/>
**C** セグメントの概要が折りたたまれました<br/>
**D** セグメントサマリを展開するオプション<br/>
**E** セグメントデータ（ある期間のセグメント内の購読者アカウント数）<br/>
**金** 「セグメントリストを開く」オプション<br/>
**G** セグメントを編集オプション<br/>
**H** 新しいセグメントを作成オプション<br/>

## セグメントの選択 {#segment-selection}

プログラマーまたは MVPD ユーザーの場合は、 **セグメントを開く** オプション。 リストからセグメントを選択し、「 」を選択します。 **セグメントを開く** をクリックして、アカウント共有レポートを表示します。

以下を使用します。 **目** アイコンをクリックすると、詳細なセグメントの概要が表示され、選択した期間内の購読者のアカウント数と再生リクエスト数に関する情報が表示されます。

+++プログラマー/MVPD 用のセグメント選択パネル

![](assets/segment-panel-programmers-mvpds.png) {width="800" align="left"}

*図：プログラマー/MVPD のセグメントパネル*

+++

セグメント概要は、次のパラメーターの定義に使用します。

**[!UICONTROL Programmers in segment]**

**[!UICONTROL Channels in segment]**

**[!UICONTROL MVPD in segment]**

**[!UICONTROL Metrics in segment]**

<!-- The definitions of these parameters will be defined in the glossary article-->

## [!UICONTROL Granularity and time interval] {#granularity-timeinterval}

The **[!UICONTROL Granularity and time interval]** セレクターを使用すると、購読者のアカウント共有行動を監視するために、週単位/月単位で集計した日付と期間を指定できます。 期間のデフォルトで選択される期間は現在の週ですが、画像に表示されるオプションを使用して期間を変更できます。

![[!UICONTROL Granularity and timeinterval]](assets/granularity-timeinterval-weekwise.png){width="350" align="left"}

*図：「精度と時間間隔」ダイアログボックス*

**A** 日付選択から日付を選択<br/>
**B** 左向き矢印を選択して後ろに移動<br/>
**C** 右向き矢印を選択して前に進みます。<br/>
**D** 週別/月別の精度を選択<br/>
**E** 選択した時間間隔<br/>

これらの制御を適用すると、問題文を「10 月にチャネル X、Y、Z を視聴した MVPD A の購読者」として定義できます。

