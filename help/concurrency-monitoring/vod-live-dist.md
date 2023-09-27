---
title: 同時実行監視で VOD とライブコンテンツを区別する方法
description: 同時実行監視で VOD とライブコンテンツを区別する方法
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# 方法：同時実行監視で VOD とライブコンテンツを区別する {#dist-vod-live}

**Q:** 同時実行監視サービスは、再生中のコンテンツのタイプ（ライブコンテンツとビデオオンデマンド）を区別できますか？



**A:** 同時実行監視は、ライブコンテンツと Video on Demand(VOD) を直接区別できません。 ビデオプレーヤーは、再生中のコンテンツのタイプを把握し、 [セッション初期化呼び出し](/help/concurrency-monitoring/cm-api-overview.md#session-initial) （同時実行監視に必要）。 通常のワークフローは次のようになります。

1. 同時実行監視のお客様は、ルールを実装する一連のメタデータを定義します（例：content-type=live|vod、device-type=mobile|console|desktop）。
1. 同時実行監視チームが、目的のポリシーを実装します。 例：
   1. content-type=live の場合、max streams=3 の場合、最新の勝ち
   1. content-type=vod, max streams=1 の場合、最新の勝ち

1. エンドユーザーがコンテンツを再生する際、ビデオプレーヤーは、ポリシーの一部として確立されたメタデータフィールドの値を送信する必要があります。

1. 定義済みのポリシーと受け取った値に基づいて、同時実行監視サービスが判定（再生/停止）を発行します。

1. システムが動作するには、ビデオプレーヤーが決定に従う必要があります。



## 関連情報 {#related-info-vod-live-dist}

* [同時実行監視の標準メタデータ属性](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [同時実行監視 API の概要](/help/concurrency-monitoring/cm-api-overview.md)
