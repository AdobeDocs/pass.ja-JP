---
title: 409競合エラーの処理
description: 同時使用制限に達した場合の409の競合エラーの処理方法について説明します
exl-id: 23a73e48-8ae0-4e0e-85db-dfc09d1386a7
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 1%
---
# 409競合エラーの処理 {#handling-409-errors}

ユーザーが新しいストリームを開始しようとし、同時使用制限に達すると、同時視聴数モニタリングは&#x200B;**409 Conflict**&#x200B;応答を返します。 優れたユーザーエクスペリエンスを提供するには、このエラーの対処方法を理解することが重要です。

## 409の対立とは？ {#what-is-a-409-conflict}

409の競合は、次の場合に発生します。

1. **ユーザーが新しいストリームを開始しようとしています**
2. **ポリシーの評価により、リクエストが制限を超えると**&#x200B;が判断されます
3. **システムが409**&#x200B;を返し、詳細な競合情報が表示される
4. **アプリケーションは**&#x200B;競合の処理方法を決定する必要があります

### 409応答構造

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 応答の理解 {#understanding-the-response}

### キーフィールド

- **`decision`**: 409件の競合について、常に「拒否」する
- **`associatedAdvice`**：違反を説明するアドバイスオブジェクトの配列
- **`conflicts`**：競合の原因となっているアクティブなセッションのリスト
- **`terminationCode`**：特定のセッションを終了するための一意のコード

### アドバイスタイプ

#### ルール違反のアドバイス

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### リモート終了のアドバイス

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## 戦略の処理 {#handling-strategies}

- FIFO モードでは、CMは既存のセッションを終了することで新しいセッションを開始できます。
- LIFO モードでは、CMは新しいセッションをブロックし、ユーザーに通知します


## ベストプラクティス {#best-practices}

### &#x200B;1. 明確なユーザーコミュニケーション

- **制限の説明** - ユーザーは、ブロックされている理由を理解する必要があります
- **使用可能なオプションを表示** – 競合を解決するために実行できること
- **コンテキストを提供** - アクティブなセッションを表示

### &#x200B;2. 正常なエラー処理

- **クラッシュしない** - 409 エラーを適切に処理する
- **代替案を提供** – 競合を解決する方法を提供します
- **ユーザー状態を保存** - コンテンツの選択を失わないでください

### &#x200B;3. ユーザーエクスペリエンスに関する検討事項

- **クイック解決** – 競合を簡単に解決する
- **選択肢をクリア** - ユーザーは自分の選択肢を理解する必要があります
- **一貫した動作** – 競合を毎回同じように処理する

### &#x200B;4. 技術的な考慮事項

- **応答を慎重に解析する** – 関連するすべての情報を抽出する
- **エッジケースの処理** – 競合が返されない場合はどうなりますか？
- **競合のログ** – 分析のためのポリシー違反の追跡
