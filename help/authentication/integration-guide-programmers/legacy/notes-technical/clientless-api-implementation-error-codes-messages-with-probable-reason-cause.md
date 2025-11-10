---
title: クライアントレス API の実装 – エラーコード/考えられる理由/原因を含むメッセージ
description: クライアントレス API の実装 – エラーコード/考えられる理由/原因を含むメッセージ
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# （レガシー）クライアントレス API の実装 – エラーコード/考えられる理由や原因を含むメッセージ {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>


## エラー：許可されていません

### 原因：

1. POST に認証ヘッダーがない
1. 認証ヘッダーの問題 – リクエスト時間がミリ秒単位かどうかを確認します。

## エラー：認証中の SC 400

### 原因：

1. サーバーは、特定のリクエスターおよび環境用に作成された登録コードを見つけられませんでした。
1. クロスドメインスクリプティングの問題が発生している可能性があります
1. 適切なスプーフィングを/etc/hosts ファイルに追加する必要があります。

## エラー：400 無効なリクエスト

### 原因：

1. POST/GETの URL の形式が正しくありません
1. SAMLAssertionParserException – 暗号化された SAML アサーションをAdobeの終了側で復号化できませんでした

## エラー：403 Forbidden

### 原因：

1. 高速リクエストが多すぎます – DoS 攻撃を防ぐための API 管理機能。
2. 初期環境を使用する場合は、スプーフィングを追加します。使用しない場合は、/etc/hosts ファイルからスプーフィングが削除されていることを確認します。

## エラー：MVPDにログインできません

### 原因：

1. ユーザー名とパスワードが一致しません
2. ログインが無効になっている可能性があります
3. ログインが実稼動用かステージング用かを確認


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
