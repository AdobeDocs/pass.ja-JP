---
title: スコアの高いアカウントの情報を書き出し
description: スコアの高いアカウントの情報を書き出します。
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# スコアの高いアカウントの情報を書き出し {#export-account-info-high-score}

[!UICONTROL Account IQ] には、上位 1000 件の購読者アカウントに関するアカウント共有の詳細を、それぞれのアカウントに基づいてエクスポートするオプションが用意されています [共有確率](/help/accountiq/product-concepts.md#account-sharing-probability-def). 書き出された CSV ファイル内のデータは、サブスクライバアカウントの共有確率の低い順に並べ替えられます。 [セグメント](/help/accountiq/product-concepts.md#segment-def)（の場合） [指定された時間枠](/help/accountiq/product-concepts.md#time-frame-def).

アカウント共有情報を書き出すオプションは、で使用できます。 [一般使用状況レポート](/help/accountiq/general-usage-reports.md) および [共有アカウントレポート](/help/accountiq/shared-acc-reports.md) ページ。

>[!NOTE]
>
>ダウンロードした CSV ファイルの数は、一般使用状況レポートページと共有アカウントレポートページで異なります。 これは、一般使用状況レポートページには、プログラマーがデバイス数、IP、郵便番号のしきい値を選択するためのフィルターが追加されているからです。 したがって、一般使用状況レポートから書き出されるデータは、適用された追加のしきい値フィルターに基づいています。

![「一般的な使用方法」の「エクスポート」オプション](assets/export.png)

購読者のアカウント共有情報を書き出すには：

1. 次の手順に従って、目的のセグメントを定義します。 [セグメントを定義して期間を選択する方法](/help/accountiq/howto-select-segment-timeframe.md) 評価のために [セグメントと期間](/help/accountiq/segments-timeframe.md) パネル。

1. を選択します。 **[!UICONTROL Export top 1000 accounts]** 共有の確率が最も高い 1,000 人の購読者のアカウント情報を書き出すオプションが追加されました。

エクスポートオプションを使用すると、（定義された期間の）共有確率が最も高い 1,000 件のアカウントの統計が、ローカルマシンの Downloads フォルダーにダウンロードされます。

>[!NOTE]
>
>ダウンロードした CSV ファイルは、CSV ファイルを読み取る任意のアプリケーション ( 例：Microsoft Excel) を使用して開くことができます。

![csv 形式で書き出したデータ](assets/exported-csv.png)

*図：CSV 形式で書き出された共有アカウントデータ*

## エクスポートされたレポートの列 {#columns-in-export}

**週/月**

曜日または月（で選択した月） **[!UICONTROL Granularity and Time Frame]** オプションを使用します。

**MVPD**

プログラマーユーザーの場合、列には、サブスクライバーアカウントが属する MVPD が示されます。

**購読者 ID**

私たちが連続して話している特定のアカウント。

**最小デバイス数**

実際の（コンテンツをストリーミングする）デバイスの数は、特定のアカウントに指定されているデバイスの最小数よりも、ほぼ確実に多くなります。

>[!NOTE]
>
>実際のデバイス数（コンテンツをストリーミング）は、特定のアカウントに指定されているデバイスの最小数よりも確実に多くなります。

**最低人数**

これらのデバイスを使用してアクティブなストリーミングコンテンツを行ったユーザーの絶対最小数です。

>[!NOTE]
>
>実際の人数（ストリーミングコンテンツ）は、特定のアカウントに指定された「最低人数」よりも、ほぼ確実に多くなります。

**[!UICONTROL # IPs]**

コンテンツのストリーミング元となる IP アドレスの数。

**[!UICONTROL # Locations]**

コンテンツのストリーミング元となる場所（郵便番号に基づく）の数。

**[!UICONTROL # Cities]**

ストリーミングが行われた都市の数。

**[!UICONTROL # States]**

ストリーミングが実行された状態の数。

**[!UICONTROL # Clusters]**

ユニークの数 [クラスター](/help/accountiq/product-concepts.md#cluster-def) ストリーミングが行われた場所です

**[!UICONTROL Geographic span (miles)]**

アカウントに関連付けられたストリーミングの場所間の最大距離です。

**[!UICONTROL # AuthN OK]**

そのアカウントを使用して、期間中にユーザーがログインした回数。

**[!UICONTROL # AuthZ OK]**

MVPD がそのアカウントに対してストリームまたは（コンテンツへの）アクセスを許可した回数。

>[!NOTE]
>
>The **[!UICONTROL # AuthZ OK]** は **[!UICONTROL # Play Requests]**；より小さい **[!UICONTROL # Play Requests]** Adobeは、通常 24 時間 MVPD に対して提供される認証をキャッシュするからです。

**[!UICONTROL # Play Requests]**

期間中のストリームの実際の数。

**[!UICONTROL # Channels]**

期間中にアカウントが閲覧した様々なチャネルの合計数。

>[!NOTE]
>
>**[!UICONTROL # Channels]** には、必ずしもログインしたプログラマーに属していないチャネルが含まれます。
>
>アカウントがチャネルを視聴したが、その期間中に他のチャネルにもアクセスしたため、アカウントのこの数が表示されました。

**使用パターン**

この列の数値は、すべてのユーザーアカウントを識別する 14 のパターンの 1 つにマッピングされる識別子です。

*表：書き出された CSV マッピングの使用パターン識別子と使用パターン*

| ID | 1 | 2 | 3 | 4 | 5 および 8 | 6 | 7 | 9 | 10 および 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 使用パターン | 通常のユーザー | 旅行者又は通勤者 | 大家族 | 親しい家族と友人 | ソーシャルグループ共有 | 多くの友人 | 同時ストリーミング | コミュニティの共有 | 不確実な動作 | 小家族 | セカンドホーム | 異常使用 |

{style="table-layout:auto"}

**共有の可能性**

共有の可能性とは、特定のアカウントが資格情報を共有している確率を指します。

>[!NOTE]
>
> （選択したセグメント内の）すべてのアカウントの共有確率の平均が、 [共有レベル](/help/accountiq/dashboard.md#sharing-level) の [集計された共有スコア](/help/accountiq/dashboard.md#aggregated-sharing).
