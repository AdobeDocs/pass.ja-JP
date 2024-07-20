---
title: Platform SSO profile-request の取得
description: Platform SSO profile-request の取得
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 1%

---

# Platform SSO profile-request の取得 {#retrieve-platform-sso-profile-request}

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

このリソースは、リクエスター ID と MVPD の組に対するプロファイルリクエストを生成します。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1. リクエスター（パスパラメーター） </br>2. mvpd （パスパラメーター） </br>3. deviceType （必須） | GET | 実際のペイロードはクライアントアプリケーションにとって不透明なので、応答 Content-Type は application/octet-stream になります。</br></br> 応答は、プロファイル SSO を取得するために、アプリケーションによって Platform</br></br>SSO エンジンに転送される必要があります。 | 200 – 成功   </br>400 – 無効なリクエスト |


| 入力パラメーター | 説明 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| mvpd | この操作が有効な MVPD ID です。 |
| deviceType | プロファイルリクエストを取得しようとしているApple プラットフォーム。  **iOS** または **tvOS**。 |
