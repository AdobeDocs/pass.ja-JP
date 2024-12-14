---
title: クライアントレス API の実装 - エラーコード/考えられる理由/原因を含むメッセージ
description: クライアントレス API の実装 - エラーコード/考えられる理由/原因を含むメッセージ
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (レガシー)クライアントレス API の実装 - エラーコード/考えられる理由/原因を含むメッセージ {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>このページの内容は、情報提供のみを目的として提供されています。 この API を使用するには、Adobe Systems からの最新のライセンス版が必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [製品発表](/help/authentication/product-announcements.md)ページに集約された最新のAdobe Pass Authentication製品発表と廃止措置のタイムラインについて常に最新情報を入手してください。

</br>


## エラー:許可されていません

### 原因：

1. POST内に認証ヘッダーがありません
1. 認証ヘッダーの問題 - 時間がミリ秒単位であるかどうかを確認しますリクエスト。

## エラー:認証中の SC 400

### 原因：

1. サーバーは、特定のリクエスターおよび環境用に作成された登録コードを見つけられませんでした。
1. クロスドメインスクリプト問題が発生している可能性があります
1. 適切なスプーフィングを /etc/hosts ファイルに追加する必要があります

## エラー：400 無効なリクエスト

### 原因：

1. POST/GET の URL の形式が正しくありません
1. SAMLAssertionParserException – 暗号化された SAML アサーションをAdobe Systems側で復号化できませんでした

## エラー:403 アクセス不可

### 原因：

1. 迅速なリクエストが多すぎる - DoS 攻撃を防ぐための API 管理の機能。
2. prequal環境を使用している場合はスプーフィングを追加し、それ以外の場合はスプーフィングが/ etc / hostsファイルから削除されたことを確認してください

## エラー:MVPD ページにログインできません

### 原因：

1. ユーザー名とパスワードが一致しません
2. ログインが無効になっている可能性があります
3. ログインが実稼働用かステージング用かを確認する


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
