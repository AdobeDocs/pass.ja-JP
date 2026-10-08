---
title: デバイス IDがない場合のクライアントレス API フロー
description: デバイス IDがない場合のクライアントレス API フロー
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%
---
# （レガシー） デバイス IDがない場合のクライアントレス API フロー {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>


## イシュー

すべてのスマートデバイスアプリが一意のデバイス IDを提供できるわけではありません。  deviceIdは必須パラメーターであるため、サービスは渡されない場合400 エラーを返します。


## 一時的なソリューション/回避策

デバイス IDのないクライアントの場合：

1. `deviceId=dummy`で初めて登録コードサービスを呼び出します
1. 応答から、UUIDを抽出します。 UUIDは、登録コード応答（XMLおよびJSON応答形式）の「id」要素で使用できます。
1. 登録サービスを2回呼び出します。 今回は`deviceId=<uuid obtained in step #2>`を渡します
1. 手順3で取得した登録コードをコンソール UIに表示します


これらの手順が完了すると、Adobe Pass AuthenticationはUUIDをデバイス IDとして使用します。 このデバイス ID （UUID）をデバイスのローカルストレージに保存します。 ユーザーが新しい登録コードを生成した場合は、手順1～4を再度実行し、以前に保存したデバイス ID （UUID）を新しいデバイス IDに置き換える必要があります。



## 永続的なソリューション

今後のリリースでAdobeでこれを変更します。reg コードを作成する際に`deviceId`をオプションのペイロードとし、`deviceId`が存在しない場合は`deviceId`ではなくUUIDをトークンキーとして使用します。

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
