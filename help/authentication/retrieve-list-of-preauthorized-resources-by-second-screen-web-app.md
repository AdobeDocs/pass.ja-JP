---
title: 2 画面目の Web アプリで事前に認証されたリソースのリストを取得
description: 2 画面目の Web アプリで事前に認証されたリソースのリストを取得
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# 2 画面目の Web アプリで事前に認証されたリソースのリストを取得 {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 説明 {#description}

事前に認証されたリソースのリストを取得するためのAdobe Pass Authentication へのリクエスト。

API には 2 つのセットがあります。1 つはストリーミングアプリ用のセット、もしくはプログラマーサービス用のセット、もう 1 つは 2 つ目のスクリーン Web アプリ用のセットです。 このページでは、AuthN アプリの API について説明します。


| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/preauthorize/{registration code} | AuthN モジュール | (1) 登録番号  </br>    （パスコンポーネント）</br>2.  requestor （必須）</br>3.  リソースリスト（必須） | GET | 個々の事前認証の決定またはエラーの詳細を含む XML または JSON。 以下のサンプルを参照してください。 | 200 — 成功</br></br>400 — 無効なリクエスト</br></br>401 — 未認証</br></br>405 — 許可されていないメソッド  </br></br>412 — 事前条件に失敗しました</br></br>500 — 内部サーバーエラー |



| 入力パラメーター | 説明 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 登録コード | 認証フローの開始時にユーザーが提供する登録コード値。 |
| 要求者 | この操作が有効な ProgrammerRequestorId。 |
| リソースリスト | ユーザーがアクセス可能なコンテンツを識別し、MVPD 認証エンドポイントによって認識される resourceIds のコンマ区切りリストを含む文字列。 |


### レスポンスのサンプル {#sample-response}

**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
    <resource>
        <id>TestStream1</id>
        <authorized>true</authorized>
    </resource>
    <resource>
        <id>TestStream2</id>
        <authorized>false</authorized>  
        <error>
            <status>403</status>
            <code>authorization_denied_by_mvpd</code>
            <message>User not authorized</message>
            <details>Your subscription package does not include the "TestStream3" channel.</details>
            <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
            <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
            <action>none</action>
        </error>
    <resource>
</resources>
```

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
