---
title: 登録レコードを削除
description: 登録リソースを削除
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 689e2f86550d9fa59337c15dd38767975a1d6d30
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---

# （レガシー）登録記録の削除 {#delete-registration-record}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!NOTE]
>
> REST APIの実装は[&#x200B; スロットル メカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって制限されています

## REST API エンドポイント {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 説明 {#delete-record}

reg コードレコードを削除し、再利用のためにreg コードをリリースします。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>例：</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者ID </br> （パスコンポーネント） </br>2。  登録コード </br> （パスコンポーネント） | DELETE | なし | 204 |

{style="table-layout:auto"}

</br>

| 入力パラメーター | 説明 |
| --- | --- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| 登録コード | ストリーミングデバイスに表示される登録コードの値（認証フローに入力される）。 |

{style="table-layout:auto"}

</br>

**[REST API リファレンスに戻る](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)**
