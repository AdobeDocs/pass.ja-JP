---
title: 2 画面目の Web アプリによる認証フローの確認
description: 2 画面目の Web アプリによる認証フローの確認
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# 2 画面目の Web アプリによる認証フローの確認 {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

この API は、2 つ目の画面ログイン Web アプリで使用して、Adobe Pass Authentication が MVPD からのログインの成功を確認したことを確認する必要があります。 この API を呼び出してからエンドユーザーに成功メッセージを表示し、ワークフローを続行するようにデバイスコンソールに進むように指示することをお勧めします。


| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{ 登録コード } | Web アプリにログイン | (1) 登録番号  </br>    （パスコンポーネント）</br>2.  要求者  </br>    （必須） | GET | 失敗した場合は、エラーの詳細を含む XML または JSON。 | 200 — 成功   </br>403 — 禁止 |

</br>

| 入力パラメーター | 説明 |
| ----------------- | --------------------------------------------------------------------------------------------- |
| 登録コード | 認証フローの開始時にユーザーが提供する登録コード値。 |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |


### レスポンスのサンプル（エラーの場合） {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [REST API リファレンスに戻る](/help/authentication/rest-api-reference.md)
