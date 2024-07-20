---
title: Dynamic Client Registration API
description: Dynamic Client Registration API
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 0%

---

# Dynamic Client Registration API {#dynamic-client-registration-api}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

現在、Adobe Pass認証でアプリケーションを識別して登録する方法は 2 つあります。

* ブラウザーベースのクライアントは、許可された [ ドメインリスト ](/help/authentication/programmer-overview.md) で登録されます
* iOSやAndroid アプリケーションなどのネイティブアプリケーションクライアントは、署名済みのリクエストメカニズムを使用して登録されます。

Adobe Pass認証は、アプリケーションを登録するための新しいメカニズムを提案します。 このメカニズムについては、次の段落で説明します。

## アプリケーション登録メカニズム {#appRegistrationMechanism}

### 技術的な理由 {#reasons}

Adobe Pass認証の認証機構は、セッション Cookie に依存していましたが、[Android Chromeのカスタムタブ ](https://developer.chrome.com/multidevice/android/customtabs){target=_blank}[Apple Safari ビューコントローラー ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank} により、この目標は達成できなくなりました。

これらの制限を考慮すると、Adobeでは、すべてのクライアントに対して新しい登録メカニズムを導入します。 OAuth 2.0 RFC に基づいており、次の要素で構成されています
（次の手順）

1. TVE からソフトウェア ステートメントを取得しています
Dashboard
1. クライアント資格情報の取得
1. アクセストークンの取得

### TVE ダッシュボードからソフトウェア ステートメントを取得しています {#softwareStatement}

リリースするアプリケーションごとに、ソフトウェア・ステートメントを取得する必要があります。 すべてのソフトウェア ステートメントは、アプリケーションが作成されると、TVE ダッシュボードを通じて提供されます。 ソフトウェアステートメントは、ユーザーのデバイス上のアプリケーションと共にデプロイする必要があります。

>[!IMPORTANT]
>
>ソフトウェアステートメントを使用する場合、署名済みリクエスター ID メカニズムは不要になります。

ソフトウェア ステートメントの作成方法の詳細については、[TVE Dashboard のクライアント登録 ](/help/authentication/dynamic-client-registration.md) を参照してください。

### クライアント資格情報の取得 {#clientCredentials}

TVE Dashboard からソフトウェア ステートメントを取得したら、Adobe Pass認証サーバにアプリケーションを登録する必要があります。 これを行うには、/register 呼び出しを実行し、一意のクライアント ID を取得します。

**リクエスト**

| HTTP コール |                    |
|-----------|--------------------|
| パス | /o/client/register |
| メソッド | POST |

| フィールド |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | TVE ダッシュボードで作成されたソフトウェア ステートメント。 | mandatory |
| redirect_uri | アプリケーションが認証フローを完了するために使用する URI です。 | optional |

| リクエストヘッダー |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | mandatory |
| X-Device-Info | デバイスおよび接続情報を渡すで定義されたデバイス情報 | mandatory |
| User-Agent | ユーザーエージェント | mandatory |

**応答**

| 応答ヘッダー |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | mandatory |

| 応答フィールド |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | 文字列 | mandatory |
| client_secret | 文字列 | mandatory |
| client_id_issued_at | long | mandatory |
| redirect_uri | 文字列のリスト | mandatory |
| grant_types | 文字列のリスト <br/> **許容値**<br/> `client_credentials`:Android SDK など、安全でないクライアントで使用されます。 | mandatory |
| エラー | **使用できる値**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unapproved_software_statement</li></ul> | エラーフローでは必須 |


#### エラーの応答 {#error-response}

エラーが発生した場合、登録サーバーは HTTP 400 （無効なリクエスト）ステータスコードで応答し、応答内に次のパラメーターを含めます。

| ステータスコード | 応答本文 | 説明 |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | リクエストに必須パラメーターがないか、サポートされていないパラメーター値が含まれている、パラメーターが繰り返されている、または形式が正しくありません。 |
| HTTP 400 | {&quot;エラー&quot;: &quot;invalid_redirect_uri&quot;} | 登録済みのアプリケーションに基づいて、このクライアントに redirect_uri を使用することはできません。 |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | ソフトウェア ステートメントが無効です。 |
| HTTP 400 | {&quot;エラー&quot;: &quot;unapproved_software_statement&quot;} | 構成に software_id が見つかりません。 |

#### クライアント資格情報の例 {#client-credentials-example}

**リクエスト：**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**応答：**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**エラー応答：**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### アクセストークンの取得 {#accessToken}

アプリケーションの一意のクライアント識別子（クライアント ID およびクライアント秘密鍵）を取得した後、アクセストークンを取得する必要があります。 アクセストークンは必須の OAuth 2.0 トークンで、Adobe Pass Authentication API の呼び出しに使用されます。

>[!NOTE]
>
>現在、アクセストークンの有効期間は 24 時間です。

**リクエスト**


| **HTTP コール** | |
| --- | --- |
| パス | `/o/client/token` |
| メソッド | POST |

| **リクエストパラメーター** | |
| --- | --- |
| `grant_type` | クライアント登録プロセスで受信しました。<br/> **許容値**<br/>`client_credentials`:Android SDK など、安全でないクライアントに使用されます。 |
| `client_id` | クライアント登録プロセスで取得されたクライアント識別子。 |
| `client_secret` | クライアント登録プロセスで取得されたクライアント識別子。 |

**応答**

| 応答フィールド | | |
| --- | --- | --- |
| `access_token` | Adobe Pass API の呼び出しに使用する必要があるアクセストークン値 | mandatory |
| `expires_in` | access_token の有効期限が切れるまでの時間（秒） | mandatory |
| `token_type` | トークンのタイプ **bearer** | mandatory |
| `created_at` | トークンの発行時間 | mandatory |
| **応答ヘッダー** | | |
| `Content-Type` | application/json | mandatory |

**エラー応答**

エラーが発生した場合、認証サーバーは HTTP 400 （無効なリクエスト）ステータスコードで応答し、応答内に次のパラメーターが含まれます。

| ステータスコード | 応答本文 | 説明 |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | 要求に必須パラメーターがないか、サポートされていないパラメーター値（許可タイプ以外）が含まれているか、パラメーターを繰り返しているか、複数の資格情報が含まれているか、クライアントの認証に複数のメカニズムが使用されているか、その他の形式が正しくありません。 |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK は、再度認証サーバーに登録する必要があります。 |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | 認証済みクライアントには、この認証付与タイプの使用が許可されていません。 |

#### アクセストークンの取得例： {#obt-access-token}

**リクエスト：**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**応答：**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**エラー応答：**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## 認証リクエストの実行 {#autheticationRequests}

アクセストークンを使用して、Adobe Pass[ 認証 API 呼び出し ](/help/authentication/initiate-authentication.md) を実行します。 これを行うには、次のいずれかの方法で、アクセストークンを API リクエストに追加する必要があります。

* リクエストに新しいクエリパラメーターを追加する。 この新しいパラメーターは **access_token** と呼ばれます。

* リクエスト：Authorization:Bearer に新しい HTTP ヘッダーを追加する。 クエリ文字列はサーバーログに表示される傾向があるので、HTTP ヘッダーを使用することをお勧めします。

エラーが発生した場合は、次のエラー応答が返されます。

| エラーの応答 |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | リクエストの形式が正しくありません。 |
| invalid_client | 403 | このクライアント ID は、要求の実行が許可されなくなりました。 新しいクライアント資格情報を生成する必要があります。 |
| access_denied | 401 | access_token が無効です（期限切れまたは無効）。 クライアントは新しい access_token を要求する必要があります。 |

### 認証リクエストの実行例：

**アクセストークンをリクエストパラメーターとして送信しています：**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**アクセストークンを HTTP ヘッダーとして送信：**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**応答本文としてのエラー応答：**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**URL パラメーターとしてのエラー応答：**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
