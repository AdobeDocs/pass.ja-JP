---
title: 制限事項
description: Account IQ のプログラマー向けの制限事項と分離モード MVPD について説明します。
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# 制限事項 {#limitations}

Account IQ の D2C バージョンと TV Everywhere バージョンは、ストリーミングプロバイダーに使用状況とサブスクリプションの共有に関する分析を提供します。 ただし、現在のバージョン 1.3 には一定の制限があり、今後のリリースで対処される予定です。

* この [共有の全体的なスコア](/help/accountiq/data-panels.md#overall-sharing-score) 現在インクルード中 [共有レベル](/help/accountiq/data-panels.md#sharing-level) および [共有アカウントからの使用状況](/help/accountiq/data-panels.md#usage-from-shared-accounts). 今後のリリースには、より多くの指標が含まれる予定です。

* ダッシュボードまたは使用パターンでセグメントを定義する場合、 [セグメントのビデオカテゴリ](/help/accountiq/data-panels.md#video-categories-segment), [チャネルおよび MVPD によるスコアの共有](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds)、および [ビデオカテゴリの使用パターンの分布](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) レポートには、20 までのデータのみを表示できます [ビデオカテゴリ](product-concepts.md#video-category-def). 20 を超えるビデオカテゴリを含むセグメントの場合、これらのレポートにはデータが表示されません。

* 現在、アカウント統計を書き出すオプションは、1,000 件のアカウントの書き出しに制限されています。

* 操作を定義する場合、選択するオプション [セグメントタイプ](/help/accountiq/operations.md#segment) 次に制限： **アカウントの固定数**. この **アカウントの数が可変** オプションは、今後のリリースで使用できるようになります。

* この **ベンチマーク**, **検出モデル**, **アクション**、および **設定** 左側のナビゲーションのセクションは、現在無効になっており、今後のリリースで使用できるようになります。

* 操作を作成する際に適用できるのは、次の 2 種類のみです [アクション](/help/accountiq/operations.md#action)  – 同時実行監視ルールと外部アクション。

* 現在、以下のみ可能です [作成](/help/accountiq/operations.md#create-new-operation) および [スケジュール](/help/accountiq/operations.md#schedule) 操作。 今後のリリースでは、一時停止、再開、完全な管理が可能になります。

* 精度および時間間隔を選択する場合、一度に 1 週間または 1 か月のデータのみを分析できます。

* 他の MVPD を使用してセグメント定義に分離モード MVPD を追加することはできません。 一部の MVPD は、複数のプログラマーチャネルをまたいでサブスクライバーを一意に識別しません。 したがって、これらの MVPD は、TV Everywhere プログラマーのために別々に扱われます。



