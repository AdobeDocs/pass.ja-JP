---
title: レポート
description: TVE ダッシュボードレポートでデータを集計する方法を説明します。
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# レポート {#Reports}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

TVE ダッシュボードの「**レポート**」セクションでは、AuthN TTL、AuthZ TTL および SSO レポートの集計データにアクセスできます。 これらのレポートには、すべての [ プラットフォーム ](#platforms) で異なる MVPD とのチャネル統合が含まれます。

レポートを使用すると、データをフィルタリングし、[ 特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds) をまたいでインサイトを収集できます。 レポートを CSV ファイルに書き出して、さらに分析することもできます。

## レポートの表示 {#view-reports}

特定のレポートを表示するには、次の手順に従います。

1. 左パネルの「**レポート**」タブを選択します。
1. 次のタブのいずれかを選択して、含まれるチャネルと MVPD の集計データを表示および書き出します。
   * [AuthN TTL レポート](#authn-ttl-reports)
   * [AuthZ TTL レポート](#authz-ttl-reports)
   * [SSO レポート](#sso-reports)

   ![ 報告の種類 ](assets/type-of-reports.png)

   *報告の種類*

### AuthN TTL レポート {#authn-ttl-reports}

AuthN TTL レポート（認証有効期間（TTL）とも呼ばれます）は、すべての [ プラットフォーム ](#platforms) をまたいだ様々な MVPD とのチャネル統合に対して認証トークンが設定されている期間を表示します。 これらのレポートを使用すると、特定の MVPD およびプラットフォームに対してユーザーが認証されたままになっている時間を調べることができます。 期間の値は、**days**、**hours**、**minutes**、**seconds** など使いやすい形式で表示されます。 AuthN TTL レポート テーブルには、異なる画面サイズに対応する水平および垂直のスクロール機能があります。

また、[ 特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds) のデータを表示してダウンロードすることもできます。

![AuthN TTL レポートのエクスポート ](assets/authn-ttl-reports.png)

*AuthN TTL レポートのエクスポート*

>[!IMPORTANT]
>
> **Set by MVPD** プレースホルダーは、MVPD がAdobe Pass Authentication 設定ではなく AuthN TTL 値を適用する場合に使用されます。

**レポートを書き出し** を選択して、ローカルマシン上に CSV ファイルとしてデータを保存します。

### AuthZ TTL レポート {#authz-ttl-reports}

AuthZ TTL レポート（Authorization Time-To-Live （TTL）とも呼ばれます）は、すべての [ プラットフォーム ](#platforms) にわたる様々な MVPD とのチャネル統合のために設定された認証トークンの期間を表示します。 これらのレポートを使用すると、特定の MVPD およびプラットフォームでユーザーがコンテンツの視聴を許可されている時間を調べることができます。 期間の値は、**days**、**hours**、**minutes**、**seconds** など使いやすい形式で表示されます。 AuthZ TTL レポートテーブルは、異なる画面サイズに対応する水平および垂直スクロールを備えています。

また、[ 特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds) のデータを表示してダウンロードすることもできます。

![AuthZ TTL レポートのエクスポート ](assets/authz-ttl-reports.png)

*AuthZ TTL レポートのエクスポート*

>[!IMPORTANT]
>
> **Set by MVPD** プレースホルダーは、MVPD がAdobe Pass Authentication 設定ではなく AuthZ TTL 値を適用する場合に使用されます。

**レポートを書き出し** を選択して、ローカルマシン上に CSV ファイルとしてデータを保存します。

### SSO レポート {#sso-reports}

SSO レポート（シングルサインオンとも呼ばれます）には、すべての [ プラットフォーム ](#platforms) をまたいだ様々な MVPD とのチャネル統合に設定されたシングルサインオンステータスが表示されます。 これらのレポートを使用すると、特定の MVPD およびプラットフォームで期待されるユーザー認証 SSO エクスペリエンスを調べることができます。 値は、**SSO 無効**、**SSO 有効**、**SSO 不明** などの使いやすい形式で表示されます。 SSO レポートテーブルには、様々な画面サイズに対応する水平および垂直のスクロールが用意されています。

また、[ 特定のチャネルまたは MVPD](#selecting-specific-channels-mvpds) のデータを表示してダウンロードすることもできます。

![SSO レポートのエクスポート ](assets/sso-reports.png)

*SSO レポートのエクスポート*

>[!IMPORTANT]
>
> **SSO が不明** プレースホルダーは、シングルサインオン（SSO）が有効で、動作している可能性があることを示しています。 ただし、次の例で説明するように、次の設定では SSO 認証が禁止される場合があります。
>
> * ユーザープラットフォーム設定：サードパーティ Cookie をブロックするオプションです。
> * ユーザーの決定：ユーザーは、自社の TV プロバイダー購読へのプラットフォームアクセスを拒否します。
> * MVPD 設定：MVPD は、各チャネルの認証をリクエストします。

**レポートを書き出し** を選択して、ローカルマシン上に CSV ファイルとしてデータを保存します。

## プラットフォーム {#platforms}

[AuthN TTL レポート ](#authn-ttl-reports)、[AuthZ TTL レポート ](#authz-ttl-reports) および [SSO レポート ](#sso-reports) は、次のような様々なプラットフォームでデータを表示します。

* **デスクトップ**:Adobe Pass Authentication JavaScript SDK を介してプログラマー実装に適用される値を表示します。

* **モバイル**

  **iOS**: Adobe Pass Authentication iOS SDK を使用して適用された値を表示します。

  **Android**: Adobe Pass Authentication Android SDK を通じて適用された値を表示します。

  **その他**：モバイルデバイス用に開発されたAdobe Pass認証 REST API を使用して適用された値を表示します。

* **TVCD**

  **Roku**:Roku をデバイスタイプとして識別し、Adobe Pass認証 REST API を介して適用された値を表示します。

  **FireTV**:Adobe Pass認証 FireTV SDK を通じて適用された値を表示します。

  **AppleTV**:Adobe Pass Authentication tvOS SDK を通じて適用された値を表示します。

  **その他**：テレビに接続されているデバイスにAdobe Pass Authentication REST API を使用して適用された値を表示します。

* **Platform 未識別**: Adobe Pass Authentication Services が不明なデバイスタイプを検出した場合に、プログラマーの実装に適用される値を表示します。

**Roku** などの目的のデバイスタイプをAdobe Pass認証 REST API または SDK と共有する方法について詳しくは、[ クライアント情報を渡す ](/help/authentication/passing-client-information-device-connection-and-application.md) メカニズムを参照してください。

>[!IMPORTANT]
>
> 集計されるデータは、各Adobe Pass Authentication Environment の具体的な設定に基づいています。 異なる TVE ダッシュボード環境を切り替えると、レポート間でデータにバリエーションが生じることが予想されます。 詳しくは、[Adobe Pass認証環境 ](/help/authentication/tve-dashboard-environments.md) を参照してください。

## 特定のチャネルと MVPD の選択 {#selecting-specific-channels-mvpds}

[AuthN TTL レポート ](#authn-ttl-reports)、[AuthZ TTL レポート ](#authz-ttl-reports) および [SSO レポート ](#sso-reports) は、デフォルトで **すべてのチャネル** 統合 **すべての MVPD** のデータを表示します。

>[!NOTE]
>
> それぞれのドロップダウンメニューで **すべてのチャネル** または **すべての MVPD** の選択を解除すると、意味のあるレポートを表示するための選択を行うためのメッセージが表示されます。

特定のチャネルに関するレポートを生成するには：

1. 選択したレポートの上部にある **含まれるチャネル** ドロップダウンメニューを選択します。

   ![ 含まれるチャネル ドロップダウンメニュー ](assets/include-channels.png)

   *含まれるチャネル ドロップダウンメニュー*

1. 「**すべてのチャネル**」の選択を解除します。
1. **含まれるチャネル** ドロップダウンメニューから、データを生成するために必要なチャネルを選択します。

>[!NOTE]
>
> **含まれる MVPD** ドロップダウンメニューでオプションを使用するには、**含まれるチャネル** ドロップダウンメニューで少なくとも 1 つのチャネルを選択する必要があります。

特定の MVPD のレポートを生成するには：

1. 選択したレポートの上部にある「**含まれる MVPD**」ドロップダウンメニューを選択します。

   ![ 含まれる MVPD ドロップダウンメニュー ](assets/include-mvpds.png)

   *含まれる MVPD ドロップダウンメニュー*

1. 「**すべての MVPD**」の選択を解除します。
1. データを生成する必要のある MVPD を「**Included MVPD&#39;s**」ドロップダウン・メニューから選択します。
