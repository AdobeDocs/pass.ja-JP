---
title: 登録レコードの削除
description: 登録リソースの削除
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 1%

---

# （従来）登記記録の削除 {#delete-registration-record}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 説明 {#delete-record}

登録コード レコードを削除し、再利用のために登録コードを解放します。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br> 例：</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1.要求者 ID </br>    （パスコンポーネント） </br>2.  登録コード </br>    （パスコンポーネント） | DELETE | なし | 204 |

{style="table-layout:auto"}

</br>

| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| 登録コード | ストリーミングデバイスに表示される（認証フローに入力される）登録コード値。 |

{style="table-layout:auto"}

</br>

### [REST API リファレンスに戻る ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
