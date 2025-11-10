---
title: 2 番目の画面の Web アプリによる事前認証済みリソースのリストの取得
description: 2 番目の画面の Web アプリによる事前認証済みリソースのリストの取得
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 1c357b918fa4f6d4b92a9055de018c55ee5861e0
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# （レガシー） 2 番目の画面の Web アプリによる事前承認済みリソースのリストの取得 {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

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

Adobe Pass Authentication に対するリクエストで、事前に許可されたリソースのリストを取得します。

API には、ストリーミングアプリまたはプログラマーサービス用の API セットと、2 番目の画面の web アプリ用の API セットの 2 つのセットがあります。 ここでは、AuthN アプリの API について説明します。


| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{registration code} | AuthN モジュール | （1）登録記号 </br>    （パスコンポーネント） </br>2.  要求者（必須） </br>3.  リソース （必須） | GET | 個々の事前認証の決定やエラーの詳細が含まれる XML または JSON。 以下のサンプルを参照してください。 | 200 - Success</br></br>400 - Bad request</br></br>401 - Unauthorized</br></br>405 - Method not allowed </br></br>412 - Precondition failed</br></br>500 – 内部サーバーエラー |



| 入力パラメーター | 説明 |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 登録コード | 認証フローの開始時にユーザーによって指定された登録コード値。 |
| 要求者 | この操作が有効なプログラマ requestorId です。 |
| resource | ユーザーがアクセス可能で、MVPD認証エンドポイントによって認識されるコンテンツを識別する resourceId のコンマ区切りリストを含む文字列。 |


### 応答のサンプル {#sample-response}

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
