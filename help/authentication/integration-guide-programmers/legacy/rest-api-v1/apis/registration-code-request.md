---
title: 登録ページ
description: 登録ページ
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# （レガシー）登録ページ {#registration-page}

## REST API エンドポイント {#clientless-endpoints}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

>[!NOTE]
>
> REST APIの実装は[&#x200B; スロットル メカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)によって制限されています

&lt;REGGIE_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## 説明 {#create-reg-code-svc}

ランダムに生成された登録コードとログインページ URIを返します。

| エンドポイント | <br>様に呼び出されました | 入力   <br> パラメーター | HTTP <br> メソッド | 応答 | HTTP <br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>例：<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | ストリーミングアプリ <br>または<br> プログラマーサービス | &#x200B;1.  依頼者<br>    （パスコンポーネント） <br>2。  deviceId （ハッシュ化）   <br>    （必須） <br>3.  device_info/X-Device-Info （必須） <br>4.  mvpd （オプション） <br>5。  ttl （オプション） <br> | 投稿する | 登録コードと情報またはエラーの詳細を含むXMLまたはJSONが失敗した場合。 以下のサンプルを参照してください。 | 201 |

{style="table-layout:auto"}

| 入力パラメーター | タイプ | 説明 |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 認証 | ヘッダー<br>の値：Bearer &lt;access_token> | DCR アクセストークン |
| 承認 | ヘッダー<br>値：application/json | 顧客が理解できる必要があるコンテンツタイプを示します |
| 依頼者 | クエリパラメーター | この操作が有効なプログラマの依頼者Id。 |
| deviceId | クエリパラメーター | デバイス ID バイト。 |
| device_info/<br>X-Device-Info | device_info: Body <br> X-Device-Info: Header | ストリーミングデバイス情報。<br>**注**：これはdevice_infoをURL パラメーターとして渡すことができますが、このパラメーターの潜在的なサイズとGET URLの長さに制限があるため、http ヘッダーでX-Device-Infoとして渡す必要があります。 <br>詳細については、[&#x200B; デバイスと接続情報の受け渡し](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)を参照してください。 |
| mvpd | クエリパラメーター | この操作が有効なMVPD ID。 |
| ttl | クエリパラメーター | このregcodeの有効期間を秒単位で指定します。<br>**注**: ttlに許可される最大値は36000秒（10時間）です。 値が大きいほど、400 HTTP応答（不正なリクエスト）が返されます。 `ttl`が空のままの場合、Adobe Pass Authenticationはデフォルト値を30分に設定します。 |
| _deviceType_ | クエリパラメーター | 非推奨です。使用しないでください。 |
| _deviceUser_ | クエリパラメーター | 非推奨です。使用しないでください。 |
| _appId_ | クエリパラメーター | 非推奨です。使用しないでください。 |

{style="table-layout:auto"}

>[!CAUTION]
>
>**ストリーミングデバイス IP アドレス**
><br>>クライアント間の実装の場合、ストリーミングデバイスのIP アドレスは、この呼び出しで暗黙的に送信されます。  サーバー間の実装では、**regcode**&#x200B;呼び出しがストリーミングデバイスではなくプログラマーサービスとして行われます。ストリーミングデバイスのIP アドレスを渡すには、次のヘッダーが必要です。
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>ここで、`<streaming\_device\_ip>`はストリーミングデバイスのパブリック IP アドレスです。
><br><br>>例：<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
><br>

### 応答JSON


#### 登録コード JSON サンプル

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
| id | 登録コードサービスによって生成されたUUID |
| コード | 登録コードサービスによって生成された登録コード |
| 依頼者 | 依頼者ID |
| mvpd | Mvpd ID |
| 生成日 | 登録コード作成タイムスタンプ（1970年1月1日GMTからのミリ秒単位） |
| 期限切れ | 登録コードの有効期限が切れるタイムスタンプ（1970年1月1日GMTからのミリ秒単位） |
| deviceId | Base64一意のデバイス ID |
| 情報:deviceId | Base64 デバイスタイプ |
| 情報:deviceInfo | Base64 Normalized Device Information build on information received from User-Agent, X-Device-Info or device_info |
| 情報:userAgent | アプリケーションから送信されたユーザーエージェント |
| 情報:originalUserAgent | アプリケーションから送信されたユーザーエージェント |
| 情報:authorizationType | DCRを使用した呼び出しのOAUTH2 |
| 情報:sourceApplicationInformation | DCRで設定されたアプリケーション情報 |

{style="table-layout:auto"}


<br>

### エラーメッセージ JSON応答サンプル （#error-sample-response）

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

