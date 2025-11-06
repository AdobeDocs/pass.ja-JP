---
title: プライバシーリクエストの作成方法
description: プライバシーリクエストの作成方法
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# プライバシーリクエストの作成方法 {#howto-make-privacy-request}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 識別子と名前空間 {#identifier-namespace}

プライバシーリクエストのアクセスまたは削除を送信する場合、顧客アプリケーションには次の識別子を含める必要があります。

* **mvpdID** - MVPDの一意の ID。
* **userID** - プログラマーのアプリのユーザーを一意に識別しますが、MVPDから開始します。 プログラマーの概要のユーザー ID についてを参照してください。
* **IMSOrgID** - Adobe Experience Cloudでお客様を一意に識別するAdobe Experience Cloud Identity Management サービス組織 ID。


以下のサンプルを確認してください。

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Adobe Pass Authentication でプライバシーリクエストを生成するには、ユーザーの認証が必要です。 そうしないと、プログラマーは、MVPDのユーザー ID を抽出する他の方法を見つける必要があります。

## リクエストのタイプ {#req-type}

Adobe Pass認証は、アクセスリクエストと削除リクエストをサポートします。

### アクセス {#access-req}

アクセスリクエストの場合：

そのデータ主体に対して作成された認証および承認リクエストの合計数の概要を含む JSON ファイルを提供します。
これらのイベントはすべて、顧客ごとにフィルタリングされます。


**リクエストのサンプル**

データアクセスリクエストを送信するAdobe Pass認証識別子を含んだ JSON をアップロードする必要があります。 整形式の JSON を確認するには、次のサンプルを参照してください。

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**応答サンプル**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### 削除 {#delete-req}

データ削除リクエストを送信するAdobe Pass認証識別子を含んだ JSON をアップロードする必要があります。 整形式の JSON を確認するには、次のサンプルを参照してください。

**リクエストのサンプル**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**応答サンプル**

削除リクエストの場合：

* データが削除されたレシートのみを共有し、削除されたすべてのデータを含む集計ファイルは共有されません。
* 応答に含まれるレシートには、そのデータ主体で見つかった認証および認証トークンの合計数の概要が含まれます。

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## リクエストのトリガー方法 {#trigger-req}

お客様がAdobeにプライバシーリクエストを送信する方法は 2 つあります。

* **手動** - [Privacy Service ユーザーインターフェイスを使用する場合 &#x200B;](#privacy-service-ui)
* **自動で** - [Privacy Service API を使用 &#x200B;](#privacy-service-api)

### Privacy Service UI を使用 {#privacy-service-ui}

Privacy Service ユーザーインターフェイスへのアクセス方法と使用方法に関する [&#x200B; 完全なチュートリアル &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=ja#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) を、Adobe I/O サービスを通じてオンラインで利用できます。 さらに、このリンクを使用して、プライバシー規制に関するビデオや記事のライブラリにアクセスできます。 Adobe Experience Cloudと GDPR メニューをクリックします。 これにより、いくつかのビデオが開きます。「GDPR UI の使い方」でその使用方法を説明しています。

UI で、お客様は独自の IMSOrgID と、各製品の GDPR リクエストの詳細を含む JSON を読み込む必要があります。

### Privacy Service API を使用 {#privacy-service-api}

Adobe Experience Platform Privacy Serviceは、プライベートデータのアクセス/削除リクエストとオプトアウトリクエストを共通の一元化された方法で処理できるようにします。

Adobeのお客様がPrivacy Service API を統合する方法について詳しくは **&#x200B;**&#x200B;Adobe API ドキュメントを参照してください。

**Postman（無料のサードパーティソフトウェア）を使用した API 呼び出しの視覚化：**

* [GitHub のPrivacy Service API Postman コレクション &#x200B;](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Postman環境の作成に関するビデオガイド &#x200B;](https://video.tv.adobe.com/v/31656?captions=jpn)
* [Postmanで環境とコレクションを読み込む手順 &#x200B;](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API パス：**

* プラットフォーム ゲートウェイ URL: `https://platform.adobe.io/`
* この API のベース パス：`/data/core/privacy/jobs`
* 完全パスの例：`https://platform.adobe.io/data/core/privacy/jobs/ping`


**必須ヘッダー：**

* すべての呼び出しにはヘッダー `Authorization`、`x-gw-ims-org-id`、`x-api-key` が必要です。 これらの値の取得方法について詳しくは、**認証に関するチュートリアル** を参照してください。
* リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCHの呼び出しなど）には、値 `Content-Type` のヘッダー `application/json` を含める必要があります。

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=ja#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
