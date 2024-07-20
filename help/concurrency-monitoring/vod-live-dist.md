---
title: 同時実行性監視で VOD コンテンツとライブコンテンツを区別する方法
description: 同時実行性監視で VOD コンテンツとライブコンテンツを区別する方法
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 同時実行性監視で VOD コンテンツとライブコンテンツを区別する方法 {#dist-vod-live}

**Q:** 同時実行監視サービスでは、再生中のコンテンツの種類（ライブコンテンツとビデオオンデマンド）を区別できますか。



**A:** 同時実行性モニタリングでは、ライブコンテンツとビデオオンデマンド（VOD）を直接区別することはできません。 ビデオプレーヤーは、再生中のコンテンツのタイプを知っている必要があり、この情報を [ セッション初期化呼び出し ](/help/concurrency-monitoring/cm-api-overview.md#session-initial) 中に送信する必要があります（同時実行監視で必要）。 通常のワークフローは次のようになります。

1. 同時実行性監視のお客様は、ルールを実装する一連のメタデータ（content-type=live|vod、device-type=mobile|console|desktop など）を定義します。
1. 同時実行性の監視チームが、目的のポリシーを実装します。 例：
   1. content-type=live、max streams=3 の場合、最新の勝利
   1. content-type=vod、max streams=1 の場合、最新の勝者

1. エンドユーザーがコンテンツを再生する場合、ビデオプレーヤーは、ポリシーの一部として設定されたこれらのメタデータフィールドの値を送信する必要があります。

1. 同時実行性モニタリングサービスは、定義されたポリシーと受信した値に基づいて、決定（再生/停止）を発行します。

1. システムを動作させるには、ビデオプレーヤーが決定に従う必要があります。



## 関連情報 {#related-info-vod-live-dist}

* [標準メタデータ属性の同時監視](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [同時実行の監視 API の概要](/help/concurrency-monitoring/cm-api-overview.md)
