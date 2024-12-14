---
title: 登録ページ
description: 登録ページ
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 0%

---

# （レガシー）登録ページ {#registration-page}

## REST API エンドポイント {#clientless-endpoints}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## 説明 {#create-reg-code-svc}

ランダムに生成された登録コードとログインページ URI を返します。

| エンドポイント | 呼び出 <br> 元 | 入力   <br> パラメーター | HTTP <br> メソッド | 応答 | HTTP <br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br> 例：<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | ストリーミングアプリ <br> プログラマ <br> サービス | 1.依頼者 <br>    （パスコンポーネント） <br>2.  deviceId （ハッシュ化）   <br>    （必須） <br>3.  device_info/X-Device-Info （必須） <br>4.  mvpd （オプション） <br>5.  ttl （オプション） <br> | POST | 失敗した場合に登録コードおよび情報またはエラーの詳細を含む XML または JSON。 以下のサンプルを参照してください。 | 201 |

{style="table-layout:auto"}

| 入力パラメーター | タイプ | 説明 |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 認証 | Header <br> Value: Bearer &lt;access_token> | DCR アクセストークン |
| 承諾 | Header <br> Value: application/json | クライアントが理解できるコンテンツタイプを示します |
| 要求者 | クエリパラメーター | この操作が有効なプログラマ requestorId です。 |
| deviceId | クエリパラメーター | デバイス ID のバイト。 |
| device_info/<br>X-Device-Info | device_info: Body <br> X-Device-Info: ヘッダー | ストリーミングデバイス情報。<br>**注意**：これは device_info を URL パラメーターとして渡す場合がありますが、このパラメーターの潜在的なサイズとGET URL の長さに関する制限により、http ヘッダーで X-Device-Info として渡す必要があります。 <br> 詳しくは、「デバイスと接続情報の受け渡し [ を参照してください ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。 |
| mvpd | クエリパラメーター | この操作が有効なMVPD ID。 |
| ttl | クエリパラメーター | このリグレコードの有効期間（秒）。<br>**メモ**:ttl に許可されている最大値は 36000 秒（10 時間）です。 値を大きくすると、400 HTTP 応答（無効なリクエスト）が返されます。 `ttl` を空のままにすると、Adobe Pass Authentication はデフォルト値の 30 分を設定します。 |
| _deviceType_ | クエリパラメーター | 非推奨（廃止予定）です。使用しないでください。 |
| _deviceUser_ | クエリパラメーター | 非推奨（廃止予定）です。使用しないでください。 |
| _appId_ | クエリパラメーター | 非推奨（廃止予定）です。使用しないでください。 |

{style="table-layout:auto"}

>[!CAUTION]
>
>**ストリーミングデバイスの IP アドレス**
><br>
>クライアントからサーバーへの実装の場合、ストリーミングデバイスの IP アドレスは、この呼び出しで暗黙的に送信されます。  **regcode** 呼び出しが、ストリーミングデバイスではなくプログラマーサービスとして行われるサーバー間実装の場合、ストリーミングデバイスの IP アドレスを渡すには次のヘッダーが必要です。
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>ここで、`<streaming\_device\_ip>` はストリーミングデバイスのパブリック IP アドレスです。
><br><br>
>例：<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### 応答 JSON


#### 登録コードの JSON サンプル

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| 要素名 | 説明 |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | 登録コードサービスで生成された UUID |
| コード | 登録コードサービスで生成された登録コード |
| 要求者 | 要求者 ID |
| mvpd | Mvpd ID |
| 生成日時 | 登録コード作成タイムスタンプ（1970 年 1 月 1 日（PT）からのミリ秒単位） |
| expires | 登録コードの有効期限が切れる際のタイムスタンプ（1970 年 1 月 1 日（GMT）からのミリ秒単位） |
| deviceId | Base64 一意のデバイス ID |
| 情報：deviceId | Base64 デバイスタイプ |
| 情報：deviceInfo | User-Agent、X-Device-Info、または device_info から受信した情報に基づいて構築された Base64 Normalized Device Information |
| 情報：userAgent | アプリケーションから送信されたユーザーエージェント |
| 情報：originalUserAgent | アプリケーションから送信されたユーザーエージェント |
| 情報：authorizationType | DCR を使用した呼び出しの OAUTH2 |
| 情報：sourceApplicationInformation | DCR で設定されたアプリケーション情報 |

{style="table-layout:auto"}


<br>

### エラーメッセージ JSON 応答のサンプル（#error-sample-response）

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

