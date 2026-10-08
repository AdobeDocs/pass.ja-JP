---
title: Platform SSO トークンとAdobe トークンの交換
description: Platform SSO トークンとAdobe トークンの交換
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 3%
---
# （レガシー） Platform SSO トークンとAdobe トークンの交換 {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Platform SSO プロファイルをAdobe トークンと「交換」できるようにします。

| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1.  依頼者（必須） </br>    </br>2.  deviceId （必須） </br>    </br>3.  mvpd （必須） </br>    </br>4.  deviceType （必須） </br>    </br>5.  SAMLResponse （必須） </br>    </br>6.  deviceUser （非推奨） </br>    </br>7.  appId （非推奨） | 投稿する | 応答が成功すると、トークンが正常に作成され、認証フローに使用する準備ができていることを示す204 No Contentになります。 | 204 - コンテンツなし</br>400 – 不正なリクエスト |


| 入力パラメーター | 説明 |
| --- | --- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| deviceId | デバイス ID バイト。 |
| mvpd | この操作が有効なMVPD ID。 |
| deviceType | プロファイルリクエストを取得しようとしているApple プラットフォーム。  **iOS**&#x200B;または&#x200B;**tvOS**&#x200B;のいずれか。 |
| SAMLResponse | Platform SSOによって返される実際のプロファイル。 |
| _deviceUser_ | デバイスユーザーID。 |
| _appId_ | アプリケーション ID/名前。 |
