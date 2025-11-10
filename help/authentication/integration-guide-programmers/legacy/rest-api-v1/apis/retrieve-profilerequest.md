---
title: Platform SSO profile-request の取得
description: Platform SSO profile-request の取得
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# （レガシー） Platform SSO profile-request の取得 {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[&#x200B; スロットルメカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

このリソースは、リクエスター ID とMVPD タプルのプロファイルリクエストを生成します。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | &#x200B;1. リクエスター（パスパラメーター） </br>2. mvpd （パスパラメーター） </br>3. deviceType （必須） | GET | 実際のペイロードはクライアントアプリケーションにとって不透明なので、応答 Content-Type は application/octet-stream になります。</br></br> 応答は、プロファイル SSO を取得するために、アプリケーションによって Platform</br></br>SSO エンジンに転送される必要があります。 | 200 – 成功   </br>400 – 無効なリクエスト |


| 入力パラメーター | 説明 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| mvpd | この操作が有効なMVPD ID。 |
| deviceType | プロファイルリクエストを取得しようとしているApple プラットフォーム。  **iOS** または **tvOS**。 |
