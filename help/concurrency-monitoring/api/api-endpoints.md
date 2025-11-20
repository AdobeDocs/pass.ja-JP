---
title: API エンドポイント
description: 同時実行監視 API の完全なリスト
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%

---


# API エンドポイント

## コアセッション管理

| エンドポイント | メソッド | 説明 |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | 新しいストリーミングセッションの作成 |
| `/sessions/{idp}/{subject}/{session}` | POST | セッションを維持するためにハートビートを送信 |
| `/sessions/{idp}/{subject}/{session}` | DELETE | セッションの終了 |
| `/runningStreams/{idp}/{subject}` | GET | サブジェクトのアクティブなセッションをすべて取得する |

## メタデータの管理

| エンドポイント | メソッド | 説明 |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | アプリケーションに必須のメタデータフィールドを取得 |
