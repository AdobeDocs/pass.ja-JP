---
title: レポート
description: TVE ダッシュボードレポートでデータを集計する方法を説明します。
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# レポート {#Reports}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

この **報告書** tve ダッシュボードの「」セクションでは、AuthN TTL、AuthZ TTL および SSO レポートの集計データにアクセスできます。 これらのレポートには、すべての異なる MVPD とのチャネル統合が含まれます [プラットフォーム](#platforms).

レポートを使用すると、でデータをフィルタリングし、インサイトを収集できます [特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds). レポートを CSV ファイルに書き出して、さらに分析することもできます。

## レポートの表示 {#view-reports}

特定のレポートを表示するには、次の手順に従います。

1. 「」を選択します **報告書** 左パネルの「」タブ。
1. 次のタブのいずれかを選択して、含まれるチャネルと MVPD の集計データを表示および書き出します。
   * [AuthN TTL レポート](#authn-ttl-reports)
   * [AuthZ TTL レポート](#authz-ttl-reports)
   * [SSO レポート](#sso-reports)

   ![レポートのタイプ](assets/type-of-reports.png)

   *レポートのタイプ*

### AuthN TTL レポート {#authn-ttl-reports}

AuthN TTL レポート（認証有効期間（TTL）とも呼ばれます）は、すべての MVPD とのチャネル統合に対して認証トークンが設定されている期間を表示します [プラットフォーム](#platforms). これらのレポートを使用すると、特定の MVPD およびプラットフォームに対してユーザーが認証されたままになっている時間を調べることができます。 期間の値は、次のような使いやすい形式で表示されます。 **days**, **時間**, **分**、および **秒**. AuthN TTL レポート テーブルには、異なる画面サイズに対応する水平および垂直のスクロール機能があります。

のデータの表示とダウンロードもできます [特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds).

![AuthN TTL レポートの書き出し](assets/authn-ttl-reports.png)

*AuthN TTL レポートの書き出し*

>[!IMPORTANT]
>
> この **MVPD で設定** プレースホルダーは、MVPD がAdobe Pass Authentication 設定ではなく AuthN TTL 値を適用する場合に使用されます。

を選択 **レポートを書き出し** ローカルマシン上に CSV ファイルとしてデータを保存する場合は、次のようにします。

### AuthZ TTL レポート {#authz-ttl-reports}

AuthZ TTL レポート（Authorization Time-To-Live （TTL）とも呼ばれます）は、すべての MVPD とのチャネル統合のために設定された認証トークンの期間を表示します [プラットフォーム](#platforms). これらのレポートを使用すると、特定の MVPD およびプラットフォームでユーザーがコンテンツの視聴を許可されている時間を調べることができます。 期間の値は、次のような使いやすい形式で表示されます。 **days**, **時間**, **分**、および **秒**. AuthZ TTL レポートテーブルは、異なる画面サイズに対応する水平および垂直スクロールを備えています。

のデータを表示およびダウンロードすることもできます。 [特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds).

![AuthZ TTL レポートの書き出し](assets/authz-ttl-reports.png)

*AuthZ TTL レポートの書き出し*

>[!IMPORTANT]
>
> この **MVPD で設定** プレースホルダーは、MVPD がAdobe Pass認証設定ではなく AuthZ TTL 値を適用する場合に使用されます。

を選択 **レポートを書き出し** ローカルマシン上に CSV ファイルとしてデータを保存する場合は、次のようにします。

### SSO レポート {#sso-reports}

SSO レポート（シングルサインオンとも呼ばれます）には、すべてのチャネルと様々な MVPD の統合に設定されたシングルサインオンステータスが表示されます [プラットフォーム](#platforms). これらのレポートを使用すると、特定の MVPD およびプラットフォームで期待されるユーザー認証 SSO エクスペリエンスを調べることができます。 値は、次のようなユーザーにわかりやすい形式で表示されます。 **SSO 無効**, **SSO 有効**、および **SSO が不明です**. SSO レポートテーブルには、様々な画面サイズに対応する水平および垂直のスクロールが用意されています。

のデータの表示とダウンロードもできます [特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds).

![Export SSO Reports](assets/sso-reports.png)

*Export SSO Reports*

>[!IMPORTANT]
>
> この **SSO が不明です** プレースホルダーは、シングルサインオン（SSO）が有効で、動作する可能性があることを示します。 ただし、次の例で説明するように、次の設定では SSO 認証が禁止される場合があります。
>
> * ユーザープラットフォーム設定：サードパーティ Cookie をブロックするオプションです。
> * ユーザーの決定：ユーザーは、自社の TV プロバイダー購読へのプラットフォームアクセスを拒否します。
> * MVPD 設定：MVPD は、各チャネルの認証をリクエストします。

を選択 **レポートを書き出し** ローカルマシン上に CSV ファイルとしてデータを保存する場合は、次のようにします。

## プラットフォーム {#platforms}

この [AuthN TTL レポート](#authn-ttl-reports), [AuthZ TTL レポート](#authz-ttl-reports)、および [SSO レポート](#sso-reports) 次のような様々なプラットフォームにデータを表示します。

* **デスクトップ**:Adobe Pass認証 JavaScript SDK を使用してプログラマー実装に適用された値を表示します。

* **モバイル**

  **iOS**:Adobe Pass Authentication iOS SDK を使用して適用された値を表示します。

  **Android**:Adobe Pass Authentication Android SDK を通じて適用された値が表示されます。

  **その他**：モバイルデバイス用に開発されたAdobe Pass認証 REST API を使用して、適用された値を表示します。

* **TVCD**

  **Roku**:Roku をデバイスタイプとして識別し、Adobe Pass認証 REST API を介して適用された値を表示します。

  **FireTV**:Adobe Pass認証 FireTV SDK を通じて適用された値を表示します。

  **AppleTV**:Adobe Pass認証 tvOS SDK を通じて適用された値が表示されます。

  **その他**：テレビに接続されているデバイスに対して、Adobe Pass認証 REST API を使用して適用された値を表示します。

* **プラットフォーム未識別**:Adobe Pass Authentication Services が不明なデバイスタイプを検出した場合に、プログラマー実装に適用される値を表示します。

目的のデバイスタイプの共有方法の詳細（例：） **Roku** Adobe Pass認証 REST API または SDK を使用すると、次のメカニズムを確認できます [クライアント情報を渡す](/help/authentication/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> 集計されるデータは、各Adobe Pass Authentication Environment の具体的な設定に基づいています。 異なる TVE ダッシュボード環境を切り替えると、レポート間でデータにバリエーションが生じることが予想されます。 を参照してください。 [Adobe Pass認証環境](/help/authentication/tve-dashboard-environments.md) もっと知りたい。

## 特定のチャネルと MVPD の選択 {#selecting-specific-channels-mvpds}

この [AuthN TTL レポート](#authn-ttl-reports), [AuthZ TTL レポート](#authz-ttl-reports)、および [SSO レポート](#sso-reports) ～のデータを提示する **すべてのチャネル** との統合 **すべての MVPD** デフォルトでは。

>[!NOTE]
>
> を選択解除した場合 **すべてのチャネル** または **すべての MVPD** それぞれのドロップダウンメニューには、意味のあるレポートを表示するために選択するためのメッセージが表示されます。

特定のチャネルに関するレポートを生成するには：

1. 「」を選択します **含まれるチャネル** 選択したレポートの上部にあるドロップダウンメニュー。

   ![含まれるチャネル ドロップダウンメニュー](assets/include-channels.png)

   *含まれるチャネル ドロップダウンメニュー*

1. 選択を解除 **すべてのチャネル**.
1. から必要なチャネルを選択 **含まれるチャネル** データを生成するドロップダウンメニュー。

>[!NOTE]
>
> で使用できるオプションを使用するには **組み込み MVPD** ドロップダウンメニューの。「」で少なくとも 1 つのチャネルを選択する必要があります **含まれるチャネル** ドロップダウンメニュー。

特定の MVPD のレポートを生成するには：

1. 「」を選択します **組み込み MVPD** 選択したレポートの上部にあるドロップダウンメニュー。

   ![含まれる MVPD ドロップダウンメニュー](assets/include-mvpds.png)

   *含まれる MVPD ドロップダウンメニュー*

1. 選択を解除 **すべての MVPD**.
1. から必要な MVPD を選択 **MVPD を含む** データを生成するドロップダウンメニュー。
