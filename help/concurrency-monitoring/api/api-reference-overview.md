---
title: API リファレンスの概要
description: エンドポイント、認証、応答形式など、同時視聴数モニタリング APIの完全なリファレンス
exl-id: 6a1c6507-03d5-4003-8b88-502eb4019346
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 2%
---
# API リファレンスの概要 {#api-reference-overview}

同時視聴数モニタリング APIは、ストリーミングセッションを管理し、同時使用ポリシーを適用するためのRESTful インターフェイスを提供します。 このリファレンスでは、すべてのエンドポイント、認証方法、リクエスト/レスポンス形式、エラー処理に関する完全なドキュメントを提供します。

## API ベース URL

### 本番環境

```
https://streams.adobeprimetime.com/v2/
```

### ステージング環境

```
https://streams-stage.adobeprimetime.com/v2/
```

**メモ：**&#x200B;開発とテストには常にステージング環境を使用してください。 実稼動資格情報は、ステージング統合が成功した後にのみ提供されます。

## 認証

すべてのAPI呼び出しには、アプリケーションの資格情報を使用したHTTP Basic認証が必要です。

- **ユーザー名：**&#x200B;お客様のアプリケーション ID （Adobeから提供）
- **パスワード：**&#x200B;空の文字列

### 認証ヘッダーの例

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## 応答形式の標準

### 成功回答

すべての成功した応答は、次の構造に従います。

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### エラー応答

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

ポリシーが評価される場合（特に409件の競合の場合）、応答には評価結果が含まれます。

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

## 一般的なHTTP ステータスコード

| コード | 説明 | 返却時 |
|------|----------------------|------------------------------------------------|
| 200 | OK | 成功したGET リクエスト |
| 202 | 作成済み/承認済み | セッションの作成/ハートビートが正常に記録されました |
| 400 | 不正なリクエスト | 無効なパラメーターまたは必須フィールドがありません |
| 401 | 未承認 | 認証が無効または見つかりません |
| 403 | 禁止 | 不十分な権限 |
| 404 | セッション IDが見つかりません | CM サービスでセッション IDが生成されない |
| 409 | 対立 | ポリシー違反（同時制限に達しました） |
| 410 | ゴーン | セッションは期限切れか終了しました |
| 429 | リクエストが多すぎます | レート制限を超えました |

## パラメーター渡しメソッド

### パスパラメーター

URL パスの一部である必須パラメーター：

1. `{idp}` - ID プロバイダー識別子
2. `{subject}` - ユーザーID （通常はAdobe Passから）
3. `{sessionId}` - セッション識別子（場所ヘッダーに返されます）

### 追加パラメーター

オプションのパラメーターは、次のURLで渡されます。

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### フォームデータ（POST/PUT）

リクエスト本文のメタデータとセッションデータ：

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### ヘッダー

HTTP ヘッダーで渡される特殊パラメーター：

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## ベストプラクティスの処理エラー

### 409紛争処理

409 Conflict responseを受け取った場合：

1. **評価結果を解析**&#x200B;して、ポリシー違反を把握します
2. **競合情報**&#x200B;を`associatedAdvice`から抽出
3. LIFO/FIFO戦略に基づいて&#x200B;**ユーザーにオプションを提示**
4. LIFO動作を実装する場合は、**終了コード**&#x200B;を使用します

### 410 Gone処理

410 Goneの応答を受け取った場合：

1. **応答に本文があるかどうかを確認する** - リモート終了を示します
2. **解析アドバイス**&#x200B;を使用して、セッションが終了した理由を把握します
3. **セッション終了を反映するためにUI**&#x200B;を更新します
4. **適切に処理** - セッションが自然にタイムアウトした可能性があります
5. **新しいセッションを開始** – 適切な場合は、新しいセッションを開始します

### レート制限

429件のリクエストが多すぎる場合：

1. **呼び出し頻度**&#x200B;を1分あたり最大200 リクエストに制限します。これは、CMが許可する最大レベルです
2. **1分に1回の必要な間隔**&#x200B;でハートビートを送信します。

## テストツール

### インタラクティブ API エクスプローラー

[Swagger UI](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)を使用してインタラクティブなテストを行います。

1. 右上隅にアプリケーション IDを入力します
2. 「Explore」をクリックして認証を設定します
3. 実際のパラメーターを使用したエンドポイントのテスト
4. リクエスト/レスポンスの例の表示
