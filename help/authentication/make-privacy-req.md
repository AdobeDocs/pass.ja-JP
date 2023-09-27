---
title: プライバシーリクエストの作成方法
description: プライバシーリクエストの作成方法
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# プライバシーリクエストの作成方法 {#howto-make-privacy-request}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 識別子と名前空間 {#identifier-namespace}

プライバシーリクエストに対するアクセスまたは削除の要求を送信する場合、顧客アプリケーションには次の識別子を含める必要があります。

* **mvpdID** - MVPD の一意の識別子。
* **userID**  — プログラマーのアプリのユーザーを一意に識別しますが、MVPD から構成されます。 「プログラマーの概要」の「ユーザー ID について」を参照してください。
* **IMSOrgID** - Adobe Experience Cloudで顧客を一意に識別するAdobe Experience Cloud Identity Management Service 組織 ID


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
>Adobe Pass Authentication のプライバシーリクエストを生成するには、ユーザーを認証する必要があります。 そうでない場合、プログラマは MVPD userID を抽出する他の方法を見つけ出す必要があります。

## リクエストのタイプ {#req-type}

Adobe Pass認証は、アクセスリクエストと削除リクエストをサポートします。

### アクセス {#access-req}

アクセス要求の場合：

データ主体に対して作成された認証および承認要求の合計数の概要を含む JSON ファイルが返されます。
これらのイベントはすべて、お客様ごとにフィルタリングされます。


**リクエストのサンプル**

送信するデータアクセスリクエストのAdobe Pass認証 ID を含む JSON をアップロードする必要があります。 整形式の JSON がどのように表示されるかを確認するには、次のサンプルを参照してください。

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

**応答のサンプル**

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

データ削除リクエストを送信するAdobe Pass認証 ID を含む JSON をアップロードする必要があります。 整形式の JSON がどのように表示されるかを確認するには、次のサンプルを参照してください。

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

**応答のサンプル**

削除リクエストの場合：

* 共有するのは、データが削除されたことを示す受信だけで、削除されたすべてのデータを集計したファイルではありません。
* 応答に含まれる受信には、そのデータ主体に対して検出された認証および認証トークンの合計数の概要が含まれます。

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

お客様がプライバシーリクエストをAdobeに送信する方法は 2 つあります。

* **手動**  — を使用 [Privacy Serviceユーザーインターフェイス](#privacy-service-ui)
* **自動的に**  — を使用 [Privacy ServiceAPI](#privacy-service-api)

### Privacy ServiceUI を使用 {#privacy-service-ui}

A [完全なチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) Privacy Service・ユーザ・インタフェースにアクセスして使用する方法に関する情報は、Adobe I/O・サービスを通じてオンラインで入手できます。 また、このリンクを使用して、プライバシー規制に関するビデオや記事のライブラリにアクセスできます。 「 Adobe Experience Cloudと GDPR 」メニューをクリックします。 これにより、多くのビデオが開きます。「GDPR UI の使い方」では、その使い方を説明しています。

UI では、各製品の GDPR 要求の詳細を含む独自の IMSOrgID と JSON を読み込む必要があります。

### Privacy ServiceAPI を使用 {#privacy-service-api}

Adobe Experience Platform Privacy Serviceは、非公開データに対するアクセス/削除リクエストと販売のオプトアウトリクエストを一元化して共通の手順を提供します。

The **Privacy ServiceAPI ドキュメント** では、顧客がAdobeAPI と統合する方法について詳しくAdobeしています。

**Postman（無料のサードパーティ製ソフトウェア）で API 呼び出しを視覚化します。**

* [GitHub でのPrivacy ServiceAPI Postmanコレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Postman環境を作成するためのビデオガイド](https://video.tv.adobe.com/v/28832)
* [Postmanで環境とコレクションを読み込む手順](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API パス：**

* PLATFORM ゲートウェイ URL: `https://platform.adobe.io/`
* この API のベースパス： `/data/core/privacy/jobs`
* 完全パスの例： `https://platform.adobe.io/data/core/privacy/jobs/ping`


**必須ヘッダー：**

* すべての呼び出しにヘッダーが必要です `Authorization`, `x-gw-ims-org-id`、および `x-api-key`. これらの値の取得方法の詳細については、 **認証チュートリアル**.
* リクエスト本文にペイロードを持つすべてのリクエスト (POST、PUT、PATCH呼び出しなど ) には、ヘッダーが含まれている必要があります `Content-Type` 値を持つ `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
