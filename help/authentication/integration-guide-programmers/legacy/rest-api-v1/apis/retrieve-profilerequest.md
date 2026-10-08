---
title: プラットフォーム SSO プロファイル要求の取得
description: プラットフォーム SSO プロファイル要求の取得
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 1%
---
# （レガシー） プラットフォーム SSO プロファイル要求の取得 {#retrieve-platform-sso-profile-request}

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

## 説明 {#description}

このリソースは、依頼者IDとMVPD タプルに対するプロファイルリクエストを生成します。


| エンドポイント | </br>様に呼び出されました | 入力</br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | ストリーミングアプリ </br></br>または</br></br> プログラマーサービス | &#x200B;1. 依頼者（パス パラメーター） </br>2。 mvpd （パス パラメーター） </br>3。 deviceType （必須） | GET | 実際のペイロードはクライアントアプリケーションに対して不透明であるため、応答Content-Typeはapplication/octet-streamになります。</br></br>応答は、プロファイル SSOを取得するために、アプリケーションによってPlatform</br></br>SSO エンジンに転送される必要があります。 | 200 – 成功</br>400 – 不正なリクエスト |


| 入力パラメーター | 説明 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 依頼者 | この操作が有効なプログラマの依頼者Id。 |
| mvpd | この操作が有効なMVPD ID。 |
| deviceType | プロファイルリクエストを取得しようとしているApple プラットフォーム。  **iOS**&#x200B;または&#x200B;**tvOS**&#x200B;のいずれか。 |
