---
title: デバイス ID がない場合のクライアントレス API フロー
description: デバイス ID がない場合のクライアントレス API フロー
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# デバイス ID がない場合のクライアントレス API フロー {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>


## 問題

一部のスマートデバイスアプリで、一意のデバイス ID を提供できるわけではありません。  deviceId は必須のパラメーターなので、渡されないと 400 エラーが返されます。


## 一時的な解決策/回避策

デバイス ID のないクライアントの場合：

1. 登録コードサービスを初めて `deviceId=dummy`
1. 応答から、UUID を抽出します。 UUID は、登録コード応答の「id」要素で使用できます（XML および JSON 応答形式）。
1. 2 回目に登録サービスを呼び出します。 今回は、 `deviceId=<uuid obtained in step #2>`
1. 手順 3 で取得した登録コードをコンソール UI に表示します。


これらの手順が完了したら、 Adobe Pass Authentication は UUID をデバイス ID として使用します。 このデバイス ID(UUID) をデバイスのローカルストレージに保存します。 ユーザーが新しい登録コードを生成するイベントでは、手順 1 ～ 4 を再度実行し、以前に保存したデバイス ID(UUID) を新しい登録コードに置き換える必要があります。



## 恒久的なソリューション

Adobeは、今後のリリースで `deviceId` reg コードを作成する際のオプションのペイロードで、UUID をトークンキーとして使用する場合はではなく `deviceId`を、 `deviceId` が存在しない。

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
