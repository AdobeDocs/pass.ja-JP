---
title: デバイス ID がない場合のクライアントレス API フロー
description: デバイス ID がない場合のクライアントレス API フロー
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# デバイス ID がない場合のクライアントレス API フロー {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>


## 問題

すべてのスマートデバイスアプリが一意のデバイス ID を提供できるわけではありません。  deviceId は必須パラメーターなので、このサービスが渡されない場合、400 エラーが返されます。


## 一時的な解決策/回避策

デバイス ID のないクライアントの場合：

1. `deviceId=dummy` で初めて登録コードサービスを呼び出す
1. 応答から UUID を抽出します。 UUID は、登録コード応答の「id」要素（XML および JSON 応答形式）で使用できます。
1. 登録サービスをもう一度呼び出します。 今回は、`deviceId=<uuid obtained in step #2>` を渡します
1. 手順 3 で取得した登録コードをコンソール UI に表示します


これらの手順が完了すると、Adobe Pass Authentication は UUID をデバイス ID として使用します。 このデバイス ID （UUID）をデバイスのローカルストレージに保存します。 ユーザーが新しい登録コードを生成した場合は、手順 1 ～ 4 を再度実行し、以前に保存したデバイス ID （UUID）を新しい UUID に置き換える必要があります。



## 永続的な解決策

Adobeでは、今後のリリースでこれを変更する予定です。reg コードの作成時には `deviceId` をオプションのペイロードとし、`deviceId` が存在しない場合は `deviceId` の代わりに UUID をトークンキーとして使用します。

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
