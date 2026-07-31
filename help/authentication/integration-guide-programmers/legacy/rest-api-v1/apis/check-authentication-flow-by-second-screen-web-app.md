---
title: 2番目のScreen Web アプリによる認証フローの確認
description: 2番目のScreen Web アプリによる認証フローの確認
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 689e2f86550d9fa59337c15dd38767975a1d6d30
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 1%

---

# （レガシー） 2番目のScreen Web アプリによる認証フローの確認 {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!NOTE]
>
> REST APIの実装は[ スロットル メカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって制限されています

## REST API エンドポイント {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

このAPIは、2番目のスクリーンログイン web アプリで使用して、MVPDからの正常なログインがAdobe Pass認証で確認されます。 エンドユーザーに成功メッセージを表示する前に、このAPIを呼び出して、デバイスコンソールに進んでワークフローを続行するように指示することをお勧めします。


| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{registration code} | Web アプリへのログイン | &#x200B;1.  登録コード </br> （パスコンポーネント） </br>2。  依頼者</br> （必須） | GET | 失敗した場合のエラーの詳細を含むXMLまたはJSON。 | 200 – 成功</br>403 – 禁止 |

</br>

| 入力パラメーター | 説明 |
| ----------------- | --------------------------------------------------------------------------------------------- |
| 登録コード | 認証フローの開始時にユーザが提供する登録コード値。 |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |


### サンプル応答（エラーの場合） {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

**[REST API リファレンスに戻る](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)**
