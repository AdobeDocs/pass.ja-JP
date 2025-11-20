---
title: API リファレンスの概要
description: エンドポイント、認証、応答形式を含む、同時実行性の監視 API の完全なリファレンス
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# API リファレンスの概要 {#api-reference-overview}

同時実行性監視 API は、ストリーミングセッションを管理し、同時使用ポリシーを適用するための RESTful インターフェイスを提供します。 このリファレンスでは、すべてのエンドポイント、認証方法、リクエスト/応答の形式、エラー処理に関する完全なドキュメントを提供します。

## API のベース URL

### 実稼動環境

```
https://streams.adobeprimetime.com/v2/
```

### ステージング環境

```
https://streams-stage.adobeprimetime.com/v2/
```

**メモ：** 開発およびテストには、常にステージング環境を使用してください。 実稼働用の資格情報は、ステージング統合が成功した後にのみ提供されます。

## 認証

すべての API 呼び出しには、アプリケーション資格情報を使用した HTTP Basic Authentication が必要です。

- **ユーザー名：** アプリケーション ID （Adobeから提供）
- **Password:** 空の文字列

### 認証ヘッダーの例

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## 応答形式の標準

### 成功の回答

成功した応答はすべて、次の構造に従います。

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### エラーの回答

すべてのエラー応答は、次の構造に従います。

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### 評価結果形式

ポリシーを評価する場合（特に 409 の競合の場合）、応答には評価結果が含まれます。

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 一般的な HTTP ステータスコード

| コード | 説明 | 返された場合 |
|------|----------------------|------------------------------------------------|
| 200 | OK | 成功したGET リクエスト |
| 202 | 作成済み/許可 | 成功したセッション作成/ハートビートが記録されました |
| 400 | リクエストが正しくありません | パラメーターが無効か、必須フィールドがありません |
| 401 | 未認証 | 認証が無効または見つかりません |
| 403 | 禁止 | 不十分な権限 |
| 404 | セッション ID が見つかりません | CM サービスでセッション ID が生成されていません |
| 409 | 競合 | ポリシー違反（同時制限に達しました） |
| 410 | 廃止 | セッションの有効期限が切れたか、終了しました |
| 429 | リクエストが多すぎます | レート制限を超えています |

## パラメーター渡しメソッド

### パスパラメーター

URL パスの一部である必須パラメーター：

1. `{idp}` - ID プロバイダーの識別子
2. `{subject}` - ユーザー ID （通常はAdobe Passから）
3. `{sessionId}` - セッション識別子（Location ヘッダーで返されます）

### 追加のパラメーター

オプションのパラメーターが URL で渡されます。

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### フォームデータ（POST/PUT）

リクエスト本文のメタデータとセッションデータ :

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### ヘッダー

HTTP ヘッダーで渡される特別なパラメーター：

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## エラー処理のベストプラクティス

### 409 競合処理

409 Conflict 応答を受信した場合：

1. **評価結果を解析** して、ポリシー違反を把握します
2. **競合情報を抽出** from `associatedAdvice`
3. LIFO/FIFO 戦略に基づいて **ユーザーにオプションを提示**
4. **終了コードを使用** LIFO 動作を実装する場合

### 410 削除された処理

410 Gone 応答を受信した場合：

1. **応答に本文があるかどうかを確認** - リモート終了を示します。
2. セッションが終了した理由を理解するための **アドバイスを解析**
3. セッションの終了を反映するように **UI を更新** します。
4. **適切に処理** - セッションが自然にタイムアウトした可能性があります
5. **新しいセッションの開始** – 適切と思われる場合は、新しいセッションを開始します

### レート制限

429 Too Many のリクエストを受け取った場合：

1. **呼び出し頻度を制限** 1 分あたり最大 200 リクエストまで。これは CM が受け入れる最大レベルです。
2. **必要な間隔でハートビートを送信** します（1 分あたり 1 回）。

## テストツール

### インタラクティブ API エクスプローラー

インタラクティブテストには、[Swagger UI](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) を使用します。

1. 右上隅にアプリケーション ID を入力します
2. 「探索」をクリックして認証を設定
3. 実際のパラメーターでエンドポイントをテスト
4. リクエスト/応答の例を表示
