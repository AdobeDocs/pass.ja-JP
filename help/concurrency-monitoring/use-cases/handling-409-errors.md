---
title: 409 競合エラーの処理
description: 同時使用制限に達した場合の 409 競合エラーの処理方法を説明します
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 1%

---


# 409 競合エラーの処理 {#handling-409-errors}

ユーザーが新しいストリームを開始しようとして同時使用制限に達すると、同時実行監視は **409 Conflict** 応答を返します。 このエラーの処理方法を理解することは、優れたユーザーエクスペリエンスを提供するために重要です。

## 409 の競合とは {#what-is-a-409-conflict}

409 の競合は、次の場合に発生します。

1. **ユーザーが新しいストリームの開始を試みる**
2. **ポリシー評価により** リクエストが制限を超えることが判明しました
3. **システムは 409 を返し** 詳細な競合情報が返される
4. **アプリケーションが決定する必要がある** 競合の処理方法

### 409 応答構造

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

## 応答について {#understanding-the-response}

### キーフィールド

- **`decision`**:409 個の競合に対して常に「拒否」します
- **`associatedAdvice`**：違反を説明するアドバイス・オブジェクトの配列
- **`conflicts`**：競合の原因となっているアクティブなセッションのリスト
- **`terminationCode`**：特定のセッションを終了するための一意のコード

### 通知タイプ

#### 規則違反に関するアドバイス

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

#### リモート終了アドバイス

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## 戦略の取り扱い {#handling-strategies}

- FIFO モードでは、CM は既存のセッションを終端することで新しいセッションを開始できます。
- LIFO モードでは、CM は新しいセッションをブロックし、ユーザーに通知します


## ベストプラクティス {#best-practices}

### &#x200B;1. ユーザー通信のクリア

- **制限の説明** - ユーザーは、ブロックされる理由を理解する必要があります
- **使用可能なオプションを表示** – 競合を解決するために実行できる操作
- **コンテキストの提供** - アクティブなセッションを表示します

### &#x200B;2. 正常なエラー処理

- **クラッシュしない** - 409 エラーを適切に処理します
- **代替手段の提供** – 競合を解決する方法を提供します
- **ユーザー状態の保存** - コンテンツの選択を失わない

### &#x200B;3. ユーザーエクスペリエンスの考慮事項

- **迅速な解決** – 競合の解決を容易にします
- **選択肢のクリア** - ユーザーがオプションを理解する必要がある
- **一貫した動作** – 毎回同じ方法で競合を処理します

### &#x200B;4. 技術上の考慮事項

- **応答を慎重に解析** – すべての関連情報を抽出します
- **エッジケースの処理** – 競合が返されない場合はどうなりますか？
- **競合をログに記録** – 分析のためのポリシー違反を追跡します


