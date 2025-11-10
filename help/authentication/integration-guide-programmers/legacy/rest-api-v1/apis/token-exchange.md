---
title: Platform SSO トークンとAdobe トークンの交換
description: Platform SSO トークンとAdobe トークンの交換
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# （従来の） Platform SSO トークンとAdobe トークンの交換 {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Platform SSO プロファイルをAdobe トークンと「交換」できるようにします。

| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | ストリーミングアプリ </br></br> プログラマ </br></br> サービス | 1.要求者（必須） </br>    </br>2。  deviceId （必須） </br>    </br>3。  mvpd （必須） </br>    </br>4。  deviceType （必須） </br>    </br>5。  SAMLResponse （必須） </br>    </br>6。  deviceUser （非推奨） </br>    </br>7。  appId （非推奨） | POST | 正常な応答は「204 No Content」になります。これは、トークンが正常に作成され、authz フローで使用する準備が整ったことを示します。 | 204 - コンテンツなし   </br>400 – 無効なリクエスト |


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| deviceId | デバイス ID のバイト。 |
| mvpd | この操作が有効なMVPD ID。 |
| deviceType | プロファイルリクエストを取得しようとしているApple プラットフォーム。  **iOS** または **tvOS**。 |
| SAMLResponse | Platform SSO によって返される実際のプロファイル。 |
| _deviceUser_ | デバイスユーザー識別子。 |
| _appId_ | アプリケーション ID/名前。 |
