---
title: API エンドポイント
description: 同時実行モニタリング APIの完全なリスト
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# API エンドポイント

## コアセッション管理

| エンドポイント | メソッド | 説明 |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | 投稿する | 新しいストリーミングセッションの作成 |
| `/sessions/{idp}/{subject}/{session}` | 投稿する | セッションを維持するためのハートビートの送信 |
| `/sessions/{idp}/{subject}/{session}` | DELETE | セッションの終了 |
| `/runningStreams/{idp}/{subject}` | GET | 件名のすべてのアクティブなセッションを取得 |

## メタデータ管理

| エンドポイント | メソッド | 説明 |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | アプリケーションの必須メタデータフィールドを取得 |
