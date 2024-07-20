---
title: アカウント IQ の操作
description: Account IQの操作には、購読者のアカウントの自動化と一括操作を行い、その影響を追跡するアクションが含まれます。
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# 運用 {#operations-tab-next-steps}

[!DNL Account IQ] analytics を使用して、購読者の使用パターンを分析し、選択したセグメントのパスワード共有のインスタンスを特定したら、[!DNL Account IQ] の操作と呼ばれる手順に沿ってターゲットを絞ったアクションを実行できます。

**操作** を使用すると、アカウントのグループに対する資格情報の共有を効果的に追跡および管理して、パスワードの共有を軽減し、大切な購読者のエクスペリエンスを向上させることができます。

定義された [ セグメント ](/help/accountiq/product-concepts.md#segment-def) にアクションを適用して、特定の [ 時間間隔内のパスワード共有に対処し ](/help/accountiq/product-concepts.md#time-interval-def) 将来の日付に実行するように操作をスケジュールできます。 これらのアクションには、パスワード共有を最小限に抑える制限や、非共有アカウントに対する制約を緩和する制限が含まれます。

操作を使用すると、アクションとその範囲を指定するだけでなく、結果も測定できます。

結果を評価することで、借り手をコンバージョンしたり、資格情報の共有を軽減したり、チャーンを減らしたりして、戦略を調整して効果を最適化できます。

操作では、次のような様々な機能を実行できます。

* [運用レポートの表示](#operation-reports)
* [新しい操作の作成](#create-new-operation)
* [操作を停止](#stop-operation)

## 運用レポートの表示 {#operation-reports}

操作レポートを使用して、操作の影響を確認できます。 処理レポートを表示するには、Account IQ アプリケーションの左側のパネルで **アクション** の下にある「**処理**」タブを選択します。 システムで使用可能な操作のリストが表示されます。 各操作に関する主要な詳細には表形式でアクセスできます。 詳細は次のとおりです。

* 操作の名前
* 現在のステータス（スケジュール済み、実行中、終了、エラー、停止など）
* 進行状況完了率
* 操作が適用されるターゲットオーディエンスまたはセグメント
* 操作に対して選択されたアクションのタイプ
* 操作の開始日
* 操作の終了日
* 操作の作成日
* 操作の最終変更日

![](assets/operations-page.png)

*Account IQの既存のオペレーションのリストと詳細*

操作のリストから目的の **操作名** を選択します。 次のレポートが表示されます。

### 操作パフォーマンス {#operation-performance}

操作パフォーマンスでは、操作の [ 評価期間 ](/help/accountiq/product-concepts.md#evaluation-period-def) 中に、影響を受けたアカウント数、操作の進行状況、セグメント内のアカウントの全体的な共有スコアを要約した上位の行の読み取り値を提供します。

![ 運用実績報告書 ](assets/operation-performance.png)

*運用実績報告書*

**A.** 影響を受けたアカウント **B.** 操作の進行状況 **C.** 全体的な共有スコア

#### 影響を受けたアカウント {#impacted-accounts}

この数には、操作の評価期間中に実行されたアクションの影響を受けたサブスクライバーのアカウントの数が表示されます。

#### 操作の進行状況 {#operation-progress}

このゲージは、計画済スケジュールから完了した操作の日数と割合を示します。

#### 共有の全体的なスコア {#overall-sharing-score}

この折れ線グラフは、[ 全体的な共有スコア ](/help/accountiq/data-panels.md#overall-sharing-score) を表します。これには、操作の評価期間中の各週の共有アカウントの共有レベルと使用状況が含まれます。

### 操作の影響：セグメント内のアカウント {#impact-accounts}

このレポートは、操作の影響の推移を示す積み重ね柱状グラフとして表示されます。

![ セグメントグラフ内のアカウントに対する操作の影響 ](assets/accounts-in-segment.png)

*セグメントグラフ内のアカウントに対する操作の影響*

X 軸は操作の [ 評価期間 ](/help/accountiq/product-concepts.md#evaluation-period-def) を表し、Y 軸は操作のセグメント内の勘定科目のステータスを表します。 グラフの各棒グラフは、次の 3 つの色に分かれています。

* ピンク色は、この操作で使用されるセグメントの条件を満たすアカウントの数を表します。

* 青は、元々セグメントに含まれていたが、操作の [ 評価期間 ](/help/accountiq/product-concepts.md#evaluation-period-def) 内の各週または各月でセグメントの条件を満たさなかったアクティブなアカウントの数を表します。

* グレーは、評価期間中に非アクティブだったアカウントを表します。

>[!NOTE]
>
>最初のピンク色のバーは、評価期間の開始時にオペレーションセグメントの条件を満たすアカウントの数を表します。

元の基準に対するアカウントの行動の変化が経時的に示されます（例えば、共有確率が 90 を超え、5 台を超えるデバイスが非アクティブになったなど）。

### 操作への影響：共有アカウントの指標 {#impact-shared-accounts}

共有アカウント・メトリックは、操作の [ 評価期間 ](/help/accountiq/product-concepts.md#evaluation-period-def) 中に、操作のセグメントにおけるサブスクライバ・アカウントによる共有レベルおよび再生要求の概要を示します。

#### 共有レベル {#share-level}

この折れ線グラフは、操作の評価期間の各週の [ 共有レベル ](/help/accountiq/data-panels.md#sharing-level) を表します。

![ 共有レベル折れ線グラフ ](assets/share-level.png){width="550" align="left"}

*共有レベル折れ線グラフ*

#### 再生リクエスト数 {#play-requests}

この折れ線グラフは、操作の評価期間の各週の [ 再生リクエスト ](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) を表します。

![ 再生リクエスト数の折れ線グラフ ](assets/number-play-requests.png){width="550" align="left"}

*再生リクエスト数の折れ線グラフ*

### 操作の影響：一般的な使用状況指標 {#impact-general-usage}

一般的な使用状況指標は、操作 [ 評価期間 ](/help/accountiq/product-concepts.md#evaluation-period-def) 中の操作のセグメントにおける平均デバイス数、IP 数およびロケーションの概要を提供します。

#### デバイス数 {#devices}

この折れ線グラフは、操作の評価期間中の各週の平均 [ デバイス数 ](/help/accountiq/general-usage-reports.md#devices-week-account) を表します。

![ デバイス数の折れ線グラフ ](assets/number-devices.png){width="550" align="left"}

*デバイス数の折れ線グラフ*

#### IP と場所の数 {#IPs-locations}

この折れ線グラフは、操作の評価期間における各週の平均 [IP 数 ](/help/accountiq/general-usage-reports.md#ip-week-account) と [ 場所 ](/help/accountiq/general-usage-reports.md#locations-week-account) を表します。

![IP と場所の折れ線グラフの数 ](assets/number-ips-locations.png){width="550" align="left"}

*IP と場所の折れ線グラフの数*

レポートを閉じてメインの **操作** ページに戻るには、左側のパネルの **アクション** の下にある **操作** タブを選択します。

## 新しい操作を作成 {#create-new-operation}

左側のパネルの **アクション** の下の **操作** タブに移動したら、**操作** ページの上部にある **新規操作を作成** を選択します。

新しい操作を作成するには、次の節の手順に従います。

* [操作の詳細](#operation-details)
* [セグメント](#segment)
* [アクション](#action)
* [スケジュール](#schedule)

### 操作の詳細 {#operation-details}

このセクションでは、**操作名** に操作の名前を入力します。

>[!TIP]
>
>操作の目的やアクションの特性を **操作名** に記述して、すばやく識別できるようにします。 **説明とタグを追加** するオプションは、今後のリリースで使用できるようになります。

![ 操作の詳細に操作名を追加 ](assets/operation-details.png)

*操作名を追加*

### セグメント {#segment}

このセクションで「**セグメントを選択**」をクリックし、この操作を使用するセグメントを選択します。 詳しくは [ セグメントの選択方法 ](/help/accountiq/segments-timeinterval.md#segment-selection) を参照してください。

セグメントを選択したら、を使用します <img alt= "セグメントの概要を展開" src="./assets/expand-segment-summary.svg" width="25"> イコンをクリックして、詳細なセグメント概要を表示します。 詳しくは、[ セグメントの概要 ](segments-timeinterval.md#segment-summary) を参照してください。

![ セグメントと時間間隔の選択 ](assets/select-segment-timeinterval.png)

*セグメントと時間間隔の選択*

>[!NOTE]
>
>前の図に示す [ ビデオカテゴリ ](product-concepts.md#video-category-def)**MVPD**、**プログラマー**、**チャンネル** など）は、Account IQの TV Everywhere バージョンで使用されるラベルを表しています。 D2C サービスとしてログインしている場合、これらのラベルには会社の特定のビデオカテゴリが表示されます。

必要に応じて、を使用します 選択 <img alt= "セグメントを編集" src="./assets/edit-segment.svg" width="25"> たセグメントを編集するためのアイコン  新 <img alt= "新しいセグメントを作成" src="./assets/create-new-segment.svg" width="25"> いセグメントを作成するためのアイコン。 詳しくは、[ 新しいセグメントの作成 ](work-with-segments.md#create-new-segment) または [ セグメントの編集 ](work-with-segments.md#edit-segment) の手順を参照してください。

>[!IMPORTANT]
>
>**セグメントタイプ** 名前付き **[!UICONTROL Fixed number of accounts]** は、現在、デフォルトで選択されています。 **[!UICONTROL Variable number of accounts]** を選択するオプションは、今後のリリースで利用できるようになります。

**精度と時間間隔** を選択して、特定の期間中の操作を監視します。 [ 精度と時間間隔の選択方法 ](/help/accountiq/segments-timeinterval.md#granularity-timeinterval) の詳細はこちら。

### アクション {#action}

このセクションでは、ドロップダウンメニューから、選択したセグメントに対して実行する **アクション** を選択します。

![ アクションのタイプの選択 ](assets/apply-actions.png)

*アクションのタイプの選択*

次の 2 つのオプションを使用できます。

* Account IQと統合された同時実行監視システムの場合は、「**CM ポリシー**」を選択します。

* **外部アクション** を選択して、Account IQ システムと統合されていない、Account IQ外部のワークフローを作成および処理します。

>[!NOTE]
>
>外部アクションは、パスワードの共有に直接関連するとは限りませんが、新しいシーズンの開始など、パスワードの共有に影響を与える可能性があります。

### スケジュール {#schedule}

このセクションで、日付選択から **開始日** および **終了日** を選択して、操作のアクティブ化を設定します。

>[!IMPORTANT]
>
>現在、デフォルトのアクティベーション **開始日** および **終了日** は、**開始日** に設定されています。 **条件が満たされた場合** および **手動** を選択するオプションは、今後のリリースで使用できるようになります。

>[!NOTE]
>
>開始日と終了日の両方が、**手順 4** で評価のために選択した精度に合っていることを確認します。

* 週ごとに集計する精度を選択した場合は、開始日と終了日を週単位で指定します（例：週 10）。
* 月ごとに集計した精度を選択した場合は、開始日と終了日を月で選択します。

![ 日付の選択から開始日と終了日を選択 ](assets/add-schedule.png)

*日付選択から開始日と終了日を選択*

**A.** 開始日ピッカー **B.** 終了日ピッカー

>[!NOTE]
>
>**開始日** は、評価期間と現在の日付の両方より後である必要があり、**終了日** は、将来の期間で操作をスケジュールおよび実行するために、開始日と現在の日付より後である必要があります。

**操作** ページの上部にある「**保存操作**」を選択して、新しい操作を処理します。

## 操作を停止 {#stop-operation}

現在実行中ステータスの操作のみを停止でき **す**。 既存の操作を停止するには、次の手順に従います。

1. Account IQ アプリケーションの左側のナビゲーションで、「**アクション** の下にある「**操作**」タブに移動します。
1. 停止する操作の **オプション** メニューを選択します。

   ![ 操作を停止するオプション メニューを選択 ](assets/stop-operation.png)

   *オプション メニューを選択して操作を停止*

1. **停止** を選択します。



