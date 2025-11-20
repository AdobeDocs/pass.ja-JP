---
title: 実装モデル
description: 実装モデル
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# 実装モデル {#imp-models}

## サーバーサイドポリシー {#ss-policies}

このモデルでは、CM をポリシーの決定ポイントとして利用し、アクセスの決定をサービスに委任します。

クライアントは適用されるポリシーを想定しないため、ハートビート応答からの再生中に、実装は定期的に、またセッション初期化の決定を確認する必要があります。
