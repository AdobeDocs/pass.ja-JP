---
title: 実装モデル
description: 実装モデル
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# 実装モデル {#imp-models}

## サーバーサイドポリシー {#ss-policies}

このモデルでは、CMをポリシー決定ポイントとして使用し、アクセス決定をサービスにデリゲートします。

クライアントは適用されるポリシーについて何も仮定しないので、実装はハートビートレスポンスからの再生中に、セッションの初期化に関する決定を定期的に確認する必要があります。
