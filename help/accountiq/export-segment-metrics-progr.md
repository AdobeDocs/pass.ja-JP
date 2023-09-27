---
title: プログラマーの指標と選択した MVPD をセグメント内にエクスポートする
description: プログラマーの指標と選択した MVPD をセグメント内にエクスポートする
exl-id: 129f3ae3-09ff-4b40-98a1-157dbd14f13e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# プログラマーの指標と選択した MVPD をセグメント内にエクスポートする {#export-metric}

ダッシュボード ( [!UICONTROL Account IQ] 選択したセグメント内の購読者アカウントの秘密鍵証明書共有統計の表とグラフが表示されます。 共有パターンとスコアの表示に加えて、選択したセグメントの MVPD およびチャネルの購読者のアカウント使用状況指標と共有スコアを、これらのテーブルから書き出すこともできます。

特定のプログラマーおよびセグメントで選択された MVPD の指標をエクスポートするには、認証済みのプログラマーユーザーとしてログインした後に、次の手順を実行します。

1. 次の手順に従って、目的のセグメントを定義します。 [セグメントを定義して期間を選択する方法](/help/accountiq/howto-select-segment-timeframe.md) 評価のために [セグメントと期間](/help/accountiq/segments-timeframe.md) パネル。

1. 次のパネルの 1 つに移動します。

   * [!UICONTROL Industrywide overall sharing scores, for selected MVPDs]
     ![](assets/ind-sharpanel-export-option.png)

   * [!UICONTROL Sharing Score by channels and MVPDs]

     ![](assets/sharscorepanel-export-option.png)

   * [!UICONTROL Number of accounts and usage by sharing probability level]

     ![](assets/usage-panel-export-option.png)

1. 選択 **[!UICONTROL Export]** オプションは、パネルの右上隅にあります。

データは CSV 形式で書き出され、ファイルがデバイス上にローカルにダウンロードされます。 目的の CSV ビューアおよびエディターを使用して、書き出されたレポートを開くことができます。

* 選択した MVPD の業界レベルの共有スコア

  ![](assets/export-ind-sharing-score.png)

* セグメント内のチャネルおよび MVPD によるスコアの共有

  ![](assets/export-risk-index-by-mvpdchannels.png)

* 共有の確率レベル別のアカウント数と使用状況

  ![](assets/export-acc-usage.png)
