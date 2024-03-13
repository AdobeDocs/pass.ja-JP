---
title: Adobeトークンの Platform SSO トークンの交換
description: Adobeトークンの Platform SSO トークンの交換
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Adobeトークンの Platform SSO トークンの交換 {#exchange-a-platform-sso-token-for-an-adobe-token}

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

Platform SSO プロファイルをAdobeトークンと「交換」できるようにします。

| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn | ストリーミングアプリ</br></br>または</br></br>プログラマーサービス | 1.要求者（必須）</br>    </br>2.  deviceId（必須）</br>    </br>3.  mvpd （必須）</br>    </br>4.  deviceType（必須）</br>    </br>5.  SAMLResponse （必須）</br>    </br>6.  deviceUser （非推奨）</br>    </br>7.  appId （非推奨） | POST | 成功した応答は「204 No Content」になり、トークンが正常に作成され、authz フローで使用する準備が整ったことを示します。 | 204 — コンテンツなし   </br>400 — 無効なリクエスト |


| 入力パラメーター | 説明 |
| --- | --- |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| deviceId | デバイス ID バイト。 |
| mvpd | この操作が有効な MVPD ID です。 |
| deviceType | プロファイルリクエストを取得しようとしているAppleプラットフォーム。  次のいずれか **iOS** または **tvOS**. |
| SAMLResponse | Platform SSO から返される実際のプロファイル。 |
| _deviceUser_ | デバイスのユーザー ID。 |
| _appId_ | アプリケーション ID/名前。 |
