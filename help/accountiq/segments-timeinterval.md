---
title: セグメントと時間間隔
description: コホートを定義するか購読者セグメントを選択して、Account IQのグラフィカルツールとレポートを使用するためのチャネルビューアのアカウント共有の可能性とパターンを測定します。
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# セグメントと時間間隔 {#segment-timeinterval}

Account IQにログインすると、ダッシュボードの上にあるセグメントと時間間隔パネルでサブスクライバー [ セグメント ](product-concepts.md#segmet-def) を定義できます。 このパネルは、結果をフィルタリングし、購読者の共有動作とパターンに関するレポートを表示するのに役立ちます。 現在、「プロパティのすべてのアカウント **という名前のセグメントがデフォルトで選択されており、次のオプションを表示できます**。

![](assets/new-segment-selector-collapsed.png){align="left"}

*折りたたまれたセグメントの概要を示すセグメントと時間間隔のパネル*

**A.** 現在選択されているセグメント名 **B.** セグメントリストを開く **C.** セグメントを編集 **D.** 新しいセグメントを作成 **E.** 精度と時間間隔セレクター **F.** セグメント概要を展開するアイコン **G.** 折りたたまれているセグメント概要 **H.** 選択した間隔のセグメント内の口座数

>[!NOTE]
>
> 折りたたまれたセグメントの概要には、Account IQの TV Everywhere 版で使用されている [ ビデオカテゴリ ](product-concepts.md#video-category-def) が表示されます。 D2C サービスとしてログインしている場合、これらのラベルには会社の特定のビデオカテゴリが表示されます。

詳しくは、左パネルの [ セグメント ](work-with-segments.md#create-new-segment) タブから [ セグメントの作成 ](work-with-segments.md#manage-segment) および **セグメントの管理** を参照してください。

## セグメントの選択 {#segment-selection}

特定のセグメントを選択するには、次の手順に従います。

1. セグメントと時間間隔パネル内の「**[!UICONTROL Open segment]**」オプションに移動します。
1. アカウント共有レポートを表示する **セグメント名** を選択します。

   ![](assets/open-segment.png){align="left"}

   *セグメント名を選択*

   >[!NOTE]
   >
   > 前の画像に示す **MVPD**、**Programmers**、**Channels** などのビデオカテゴリは、Account IQの TV Everywhere バージョンで使用されるラベルを表しています。 D2C サービスとしてログインしている場合、これらのラベルには会社の特定のビデオカテゴリが表示されます。

1. 「**[!UICONTROL Open segment]**」を選択します。


## 精度と時間間隔の選択 {#granularity-timeinterval}

**精度と時間間隔** セレクターを使用すると、購読者の共有動作を監視するために毎週/毎月集計された日付と期間を指定できます。 デフォルトの選択は現在の週です。

![ 精度と時間間隔 ](assets/granularity-timeinterval-weekwise.png){align="left"}

*精度と時間間隔のダイアログボックス*

**A.** 精度と時間間隔のセレクター **B.** 右矢印で次の月/週に移動 **C.** 精度を週/月で選択するオプション **D.** 現在選択されている時間間隔 **E.** 左矢印で前の月/週に移動

期間は、次の手順で変更できます。

1. 日付選択から **[!UICONTROL Granularity and Time Interval]** を選択します。

1. **[!UICONTROL Aggregate By]** のオプションから **[!UICONTROL Week]** または **[!UICONTROL Month]** を選択して、評価の精度を設定します。

1. 精度を選択したら、順方向または逆方向の矢印を使用して時間範囲内を移動することができます。

1. 評価する特定の期間を選択します。

1. 「**[!UICONTROL Apply]**」を選択して、選択が有効になることを確認します。

これにより、問題のステートメントを「12 月の選択週にチャネル X、Y、および Z を視聴した MVPD A のサブスクライバ」として定義できます。

## セグメントの概要 {#segment-summary}

セグメントサマリーは、D2C サービスと TV Everywhere で似ています。 ビデオカテゴリは、Account IQのバージョンごとに異なります。

を選択 <img alt= "セグメントの概要を展開" src="./assets/expand-segment-summary.svg" width="25"> イコンをクリックして、詳細なセグメント概要を表示します。 また、選択した期間内の購読者のアカウント数と再生リクエストに関する情報も表示されます。

+++ D2C サービス

![](assets/segment-panel-d2c.png){align="left"}

*D2C サービスのセグメントサマリー*

>[!NOTE]
>
>前の画像で示した [ ビデオカテゴリ ](product-concepts.md#video-category-def) セグメントの **地域** や **コンテンツタイプ** などは、一例です。 Account IQにログインすると、これらのラベルに会社固有のビデオカテゴリが表示されます。

**セグメントの概要** には、セグメントを定義する次の条件が含まれます。

**[地域とコンテンツタイプ ](product-concepts.md#video-category-def) セグメントでは** アカウント共有レポートに表示される、共有アカウントで監視されるビデオストリームに関連付けられたメタデータラベルを参照します。

**[指標 ](product-concepts.md#metric) セグメントでは** アカウント共有レポートで識別するために、サブスクライバーが満たした必要がある属性または条件を参照します。

+++

+++ あらゆる場所のテレビ

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*プログラマー/MVPD のセグメントの概要*

**セグメントの概要** には、セグメントを定義する次の条件が含まれます。

**[プログラマー ](product-concepts.md#programmer-def) セグメントでは** アカウント共有レポートに表示される共有アカウントでビデオストリームを視聴したコンテンツプロバイダーを参照します。

**[チャネル ](product-concepts.md#channel-def) セグメントの中では** アカウント共有レポートに表示される共有アカウントでビデオストリームが視聴されたチャネルを指します。

セグメントの **[MVPD](product-concepts.md#mvpd-def) は** アカウント共有レポートで識別するためにサブスクライバーが関連付けられているマルチビデオプログラミング配信業者を指します。

**[指標 ](product-concepts.md#metric) セグメントでは** アカウント共有レポートで識別するために、サブスクライバーが満たした必要がある属性または条件を参照します。

+++
