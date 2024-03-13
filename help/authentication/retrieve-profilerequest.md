---
title: Platform SSO プロファイルリクエストの取得
description: Platform SSO プロファイルリクエストの取得
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 1%

---

# Platform SSO プロファイルリクエストの取得 {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

>[!NOTE]
>
> REST API 実装は、 [スロットルメカニズム](/help/authentication/throttling-mechanism.md)

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

このリソースは、要求元 ID と MVPD タプルに対するプロファイルリクエストを生成します。


| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | ストリーミングアプリ</br></br>または</br></br>プログラマーサービス | 1.リクエスト元（パスパラメーター）</br>2. mvpd (path param)</br>3. deviceType（必須） | GET | 実際のペイロードはクライアントアプリケーションに対して不透明なので、応答 Content-Type は application/octet-stream になります。</br></br>応答は、アプリケーションによって Platform に転送される必要があります。</br></br>プロファイル SSO を取得するための SSO エンジン。 | 200 — 成功   </br>400 — 無効なリクエスト |


| 入力パラメーター | 説明 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| mvpd | この操作が有効な MVPD ID です。 |
| deviceType | プロファイルリクエストを取得しようとしているAppleプラットフォーム。  次のいずれか **iOS** または **tvOS**. |
