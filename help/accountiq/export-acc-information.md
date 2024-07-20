---
title: 共有スコアの高いアカウントの情報の書き出し
description: 共有スコアが高いアカウントの情報の書き出し。
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# 共有スコアの高いアカウントの情報の書き出し {#export-account-info-high-score}

[!UICONTROL Account IQ] を使用すると、「共有確率 [ に基づいて、上位 1000 個の購読者アカウントのアカウント共有の詳細を書き出すことができ ](/help/accountiq/product-concepts.md#account-sharing-probability-def) す。 現在の [ セグメント ](/help/accountiq/product-concepts.md#segment-def) および [ 指定された時間間隔 ](/help/accountiq/product-concepts.md#time-interval-def) のアカウント共有情報は、「[ 共有アカウントレポート ](/help/accountiq/shared-acc-reports.md)」ページでエクスポートできます。

特定のセグメントの購読者アカウントのアカウント共有情報をエクスポートするには、次の手順に従います。

1. 自分の資格情報を使用してログインします。
1. 「**レポート** セクションの下の「**共有アカウント** タブに移動します。
1. 必要なセグメントと時間間隔をセグメントと時間間隔パネルから選択します。 詳しくは [ セグメントと時間間隔の選択方法 ](segments-timeinterval.md) を参照してください。

   必要に応じて、[ セグメントの作成 ](work-with-segments.md#create-new-segment) または [ セグメントの編集 ](work-with-segments.md#edit-segment) の手順を参照してください。

1. セグメント **[!UICONTROL Export top 1000 accounts]** 時間間隔パネルの右上隅にある「セグメント」を選択します。

   ![ 上位 1000 件のアカウントの書き出し ](assets/export-top-1000-accounts.png)

   *「上位 1000 件のアカウントを書き出し」オプションを選択*

ファイルは.csv 形式でローカルマシンに自動的にダウンロードされます。

このファイルには、現在のセグメントのサブスクライバーアカウントの共有確率に基づく、上位 1000 件のアカウントのデータが降順で含まれています。

書き出された.csv ファイルの例を次に示します。

![.csv ファイルに書き出されたデータ ](assets/exported-csv.png)

*.csv ファイルに書き出されたデータ*

## 書き出されたレポートの列 {#columns-in-export}

**週/月**

セグメントセレクターの「**[!UICONTROL Granularity and Time Interval]**」オプションで選択した週または月。

**MVPD**

プログラマーの場合、列にはアカウントが購読されているディストリビューターが表示されます。

>[!NOTE]
>
> **MVPD** 列は、TV Everywhere バージョンでのみ使用できます。

**サブスクライバー ID**

特定のアカウントの一意の ID。

**最小デバイス数**

ユーザーがコンテンツをアクティブにストリーミングするデバイスの最小数。

>[!NOTE]
>
>コンテンツをストリーミングする実際のデバイス数が、特定のアカウントに指定されたデバイスの最小数を超えています。

**最小人数**

これらのデバイスを使用してコンテンツをアクティブにストリーミングした個人の最小数。

>[!NOTE]
>
>コンテンツをストリーミングする個人の実際の数が、特定のアカウントに割り当てられた個人の最小数を超えています。

**[!UICONTROL # IPs]**

コンテンツのストリーミング元の IP アドレスの数。

**[!UICONTROL # Locations]**

コンテンツのストリーミング元の場所（郵便番号に基づく）の数。

**[!UICONTROL # Cities]**

ストリーミングアクティビティが発生した都市の数。

**[!UICONTROL # States]**

ストリーミングアクティビティが発生した状態の数。

**[!UICONTROL # Clusters]**

ストリーミングが行われた個別の [ クラスター ](/help/accountiq/product-concepts.md#cluster-def) の数。

**[!UICONTROL Geographic span (miles)]**

アカウントに関連付けられたストリーミングの場所の間の最大距離。

**[!UICONTROL # AuthN OK]**

そのアカウントを使用して指定された期間中にユーザーが行ったログインの数。

>[!NOTE]
>
> 一部の D2C サービスでは、会社のデータに含まれていない可能性があるので、**[!UICONTROL # AuthN OK]** のデータが表示されない場合があります。

**[!UICONTROL # AuthZ OK]**

MVPD がそのアカウントのコンテンツへのストリームを承認またはアクセスを許可した回数。

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** は、D2C サービスでは使用できません。

>[!NOTE]
>
>Tv Everywhere の場合、**[!UICONTROL # AuthZ OK]** れは **[#件の再生リクエスト](/help/accountiq/product-concepts.md##play-requests-def)** の数と相関します。 Adobeは通常、約 24 時間 MVPD から認証をキャッシュするので、常に **[!UICONTROL # Play Requests]** 未満になります。


**[!UICONTROL # Play Requests]**

指定した期間中に発生したストリームの実際の数。

>[!NOTE]
>
>[#件の再生要求 ](/help/accountiq/product-concepts.md##play-requests-def) 列は、TV Everywhere MVPD バージョンでは使用できません。

**[!UICONTROL # Channels]**

指定された期間にアカウントが視聴したチャネルの合計数。

>[!NOTE]
>
> D2C サービスの場合、**[!UICONTROL # Channels]** は **[!UICONTROL # Video categories]** 数と同じです。

>[!NOTE]
>
>TV Everywhere の場合、ログインしているプログラマーに属さない可能性のあるチャンネルが含まれます。 アカウントのこの数には、指定された期間中にアクセスしたチャネルとその他のチャネルが含まれます。


**使用パターン**

これらの列内の値は、すべてのユーザーアカウントの分類に使用する 14 のパターンのいずれかに対応する識別子として機能します。

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">使用パターン</th>
      </tr>
      <tr>
        <td>1</td>
        <td>通常のユーザー</td>
      </tr>
      <tr>
        <td>2</td>
        <td>旅行者又は通勤者</td>
      </tr>
      <tr>
        <td>3</td>
        <td>大家族</td>
      </tr>
      <tr>
        <td>4</td>
        <td>家族や友だちを閉じる</td>
      </tr>
      </tr>
         <td>5 と 8</td>
         <td>ソーシャルグループ共有</td>
      </tr>
      </tr>
         <td>6</td>
         <td>多数の友人グループ</td>
      </tr>
      </tr>
         <td>7</td>
         <td>同時ストリーミング</td>
      </tr>
      </tr>
         <td>9</td>
         <td>コミュニティ共有</td>
      </tr>
      </tr>
         <td>10 と 11</td>
         <td>不確かな行動</td>
      </tr>
      </tr>
         <td>12</td>
         <td>小家族</td>
      </tr>
      </tr>
         <td>13</td>
         <td>セカンドホーム </td>
      </tr>
      </tr>
         <td>14</td>
         <td>異常な使用</td>
      </tr>
    </tbody>
  </table>

*使用パターンを含む、エクスポートされた.csv マッピングの使用パターン識別子*

**共有確率**

特定のアカウントが資格情報を共有している可能性。

>[!NOTE]
>
> 選択したセグメント内のすべてのアカウントの共有確率の平均が、[ 平均共有スコア ](/help/accountiq/data-panels.md#sharing-level) の [ 共有レベル ](/help/accountiq/data-panels.md#aggregated-sharing) の計算に使用されます。
