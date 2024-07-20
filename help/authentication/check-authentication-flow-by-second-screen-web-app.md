---
title: 2 番目の画面の Web アプリによる認証フローの確認
description: 2 番目の画面の Web アプリによる認証フローの確認
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 2 番目の画面の Web アプリによる認証フローの確認 {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

この API は、2 番目の画面のログイン Web アプリで使用され、Adobe Pass Authentication が MVPD からの正常なログインを確認したことを確認します。 エンドユーザーに対して、ワークフローを続行するにはデバイスコンソールに進むようにという成功メッセージを表示する前に、この API を呼び出すことをお勧めします。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{ 登録コード } | ログイン Web アプリ | （1）登録記号 </br>    （パスコンポーネント） </br>2.  要求者 </br>    （必須） | GET | 失敗した場合にエラーの詳細を含む XML または JSON。 | 200 – 成功   </br>403 – 禁止されています |

</br>

| 入力パラメーター | 説明 |
| ----------------- | --------------------------------------------------------------------------------------------- |
| 登録コード | 認証フローの開始時にユーザーによって指定された登録コード値。 |
| 要求者 | この操作が有効なプログラマ requestorId です。 |


### 応答のサンプル （エラーの場合） {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [REST API リファレンスに戻る ](/help/authentication/rest-api-reference.md)
