---
title: 動的クライアント登録 API
description: 動的クライアント登録 API
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# 動的クライアント登録 API {#dynamic-client-registration-api}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

現在、Adobe Pass Authentication がアプリケーションを識別して登録する方法は 2 つあります。

* ブラウザーベースのクライアントは、許可されたを介して登録されます。 [ドメインリスト](/help/authentication/programmer-overview.md)
* iOSや Android アプリケーションなどのネイティブアプリケーションクライアントは、署名付きの要求者メカニズムを通じて登録されます。

Adobe Pass Authentication は、アプリケーションを登録するための新しいメカニズムを提案します。 このメカニズムについては、次の段落で説明します。

## 出願登録機構 {#appRegistrationMechanism}

### 技術的な理由 {#reasons}

Adobe Pass Authentication の認証メカニズムはセッション cookie に依存していましたが、原因は次のとおりです。 [Android Chrome カスタムタブ](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}に設定した場合、この目標はもはや達成できません。

これらの制限を考慮し、Adobeでは、すべてのクライアントに新しい登録メカニズムが導入されました。 OAuth 2.0 RFC に基づいており、次の手順で構成されています。

1. TVE ダッシュボードからのソフトウェア文の取得
1. クライアント資格情報の取得
1. アクセストークンの取得

### TVE ダッシュボードからのソフトウェア文の取得 {#softwareStatement}

リリースするアプリケーションごとに、ソフトウェア文を取得する必要があります。 すべてのソフトウェア文は、アプリケーションが作成されると、TVE ダッシュボードを通じて提供されます。 ソフトウェア文は、ユーザーのデバイス上のアプリケーションと共にデプロイする必要があります。

>[!IMPORTANT]
>
>ソフトウェア文を使用する場合、署名された要求者 ID メカニズムは不要になります。

ソフトウェア文の作成方法の詳細については、 [TVE ダッシュボードでのクライアント登録](/help/authentication/dynamic-client-registration.md).

### クライアント資格情報の取得 {#clientCredentials}

TVE ダッシュボードからソフトウェア文を取得したら、アプリケーションをAdobe Pass認証サーバーに登録する必要があります。 これを行うには、 /register 呼び出しを実行し、一意のクライアント識別子を取得します。

**リクエスト**

| HTTP 呼び出し |                    |
|-----------|--------------------|
| パス | /o/client/register |
| メソッド | POST |

| フィールド |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | TVE ダッシュボードで作成されたソフトウェア文。 | 必須 |
| redirect_uri | アプリケーションが認証フローの完了に使用する URI。 | オプション |

| リクエストヘッダー |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | 必須 |
| X-Device-Info | 「デバイスと接続情報を渡す」で定義されるデバイス情報 | 必須 |
| User-Agent | ユーザーエージェント | 必須 |

**応答**

| 応答ヘッダー |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | 必須 |

| 応答フィールド |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | 文字列 | 必須 |
| client_secret | 文字列 | 必須 |
| client_id_issued_at | long | 必須 |
| redirect_uris | 文字列のリスト | 必須 |
| grant_types | 文字列のリスト<br/> **受け入れられた値**<br/> `client_credentials`:Android SDK など、安全でないクライアントで使用されます。 | 必須 |
| エラー | **受け入れられた値**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unapproved_software_statement</li></ul> | エラーフローで必須 |


#### エラー応答 {#error-response}

エラーが発生した場合、登録サーバーは HTTP 400（無効なリクエスト）ステータスコードで応答し、応答内に次のパラメーターを含めます。

| ステータスコード | 応答本文 | 説明 |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | リクエストに必須のパラメーターがない、サポートされていないパラメーター値が含まれている、パラメーターを繰り返す、またはその他の形式ではありません。 |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | redirect_uri は、登録済みのアプリケーションに基づいて、このクライアントでは許可されていません。 |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | ソフトウェア文が無効です。 |
| HTTP 400 | {&quot;error&quot;: &quot;unapproved_software_statement&quot;} | software_id が設定に見つかりません。 |

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

アプリケーションの一意のクライアント識別子（クライアント ID とクライアント秘密鍵）を取得したら、アクセストークンを取得する必要があります。 アクセストークンは必須の OAuth 2.0 トークンで、Adobe Pass Authentication API の呼び出しに使用されます。

>[!NOTE]
>
>現在、アクセストークンの有効期間は 24 時間です。

**リクエスト**


| **HTTP 呼び出し** | |
| --- | --- |
| パス | `/o/client/token` |
| メソッド | POST |

| **リクエストパラメーター** | |
| --- | --- |
| `grant_type` | クライアント登録プロセスで受け取りました。<br/> **許可された値**<br/>`client_credentials`:Android SDK など、安全でないクライアントに使用されます。 |
| `client_id` | クライアント登録プロセスで取得されたクライアント識別子。 |
| `client_secret` | クライアント登録プロセスで取得されたクライアント識別子。 |

**応答**

| 応答フィールド | | |
| --- | --- | --- |
| `access_token` | Adobe Pass API の呼び出しに使用する必要があるアクセストークンの値 | 必須 |
| `expires_in` | access_token の有効期限が切れるまでの時間（秒） | 必須 |
| `token_type` | トークンのタイプ **無記名者** | 必須 |
| `created_at` | トークンの発行時間 | 必須 |
| **応答ヘッダー** | | |
| `Content-Type` | application/json | 必須 |

**エラー応答**

エラーが発生した場合、認証サーバーは HTTP 400（無効なリクエスト）ステータスコードで応答し、応答内に次のパラメーターを含めます。

| ステータスコード | 応答本文 | 説明 |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | リクエストに必須のパラメーターがない、サポートされていないパラメーター値（付与タイプ以外）が含まれている、パラメーターを繰り返す、複数の資格情報を含む、クライアントを認証する複数のメカニズムを利用する、またはその他の形式が正しくありません。 |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK は、認証サーバーに再度登録する必要があります。 |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | 認証済みのクライアントは、この認証付与タイプの使用を許可されていません。 |

#### アクセストークンの取得の例： {#obt-access-token}

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

アクセストークンを使用したAdobe Passの実行 [認証 API 呼び出し](/help/authentication/initiate-authentication.md). これをおこなうには、次のいずれかの方法で、アクセストークンを API リクエストに追加する必要があります。

* 新しいクエリパラメーターをリクエストに追加することによって。 この新しいパラメーターは、 **access_token**.

* 新しい HTTP ヘッダーをリクエストに追加します (Authorization: Bearer)。 クエリー文字列はサーバーログに表示されるので、HTTP ヘッダーを使用することをお勧めします。

エラーの場合、次のエラー応答が返される可能性があります。

| エラー応答 |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | リクエストの形式が正しくありません。 |
| invalid_client | 403 | クライアント ID は、要求を実行できなくなりました。 新しいクライアント資格情報が生成される必要があります。 |
| access_denied | 401 | access_token が無効です（期限切れまたは無効です）。 クライアントは新しい access_token を MUST リクエストします。 |

### 認証リクエストの実行の例：

**リクエストパラメーターとしてアクセストークンを送信中：**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**HTTP ヘッダーとしてアクセストークンを送信しています。**

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
