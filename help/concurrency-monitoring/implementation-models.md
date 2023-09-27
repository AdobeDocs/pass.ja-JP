---
title: 実装モデル
description: 実装モデル
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# 実装モデル {#imp-models}

## サーバー側ポリシー {#ss-policies}

このモデルでは、CM をポリシー決定ポイントとして使用し、アクセスの決定をサービスに委任します。

クライアントは適用されたポリシーを前提としないので、ハートビート応答からの再生中に、セッションの初期化と定期的なベースで、実装で判定を確認する必要があります。
