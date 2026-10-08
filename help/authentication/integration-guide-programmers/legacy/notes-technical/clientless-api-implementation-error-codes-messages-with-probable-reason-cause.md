---
title: クライアントレス APIの実装 – 原因が考えられるエラーコード/メッセージ
description: クライアントレス APIの実装 – 原因が考えられるエラーコード/メッセージ
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# （レガシー） クライアントレス APIの実装 – 原因が考えられるエラーコード/メッセージ {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>


## エラー：許可されていません

### 原因：

1. POSTに認証ヘッダーがありません
1. 認証ヘッダーの問題 – リクエスト時間がミリ秒単位かどうかを確認します。

## エラー：認証中にSC 400

### 原因：

1. 特定の要求者と環境用に作成された登録コードがサーバーに見つかりませんでした。
1. クロスドメインスクリプティングの問題が発生している可能性があります
1. /etc/hosts ファイルに適切なスプーフィングを追加する必要があります

## エラー：400不正なリクエスト

### 原因：

1. POST/GETのURLが正しくありません
1. SAMLAssertionParserException – 暗号化されたSAML アサーションをAdobeのエンドで復号できませんでした

## エラー：403禁止

### 原因：

1. 高速リクエストが多すぎる – DoS攻撃を防ぐためのAPI管理の機能。
2. prequal環境を使用している場合は、スプーフィングを追加します。そうでない場合は、/etc/hosts ファイルからスプーフィングが削除されていることを確認します

## Error: Unable to Login to MVPD page

### 原因：

1. ユーザー名とパスワードが一致しません
2. ログインが無効化されている可能性があります
3. ログインが実稼動用かステージング用かを確認します


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
