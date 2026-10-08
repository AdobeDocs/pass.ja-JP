---
title: プライバシーリクエストの作成方法
description: プライバシーリクエストの作成方法
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%
---
# プライバシーリクエストの作成方法 {#howto-make-privacy-request}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 識別子と名前空間 {#identifier-namespace}

アクセスまたはプライバシーの削除リクエストを送信する場合、顧客アプリケーションには次の識別子を含める必要があります。

* **mvpdID** - MVPDの一意のID。
* **userID** - プログラマーのアプリのユーザーを一意に識別しますが、MVPDから構成されます。 プログラマの概要の「ユーザーIDについて」を参照してください。
* **IMSOrgID** - Adobe Experience Cloudのお客様を一意に識別するAdobe Experience Cloud Identity Management サービス組織ID


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
>Adobe Pass認証のプライバシーリクエストを生成するには、ユーザーを認証する必要があります。 それ以外の場合、プログラマはMVPDのユーザーIDを抽出する他の手段を見つける必要があります。

## リクエストの種類 {#req-type}

Adobe Pass Authenticationでは、アクセス要求と削除要求をサポートしています。

### アクセス {#access-req}

アクセス要求の場合：

そのデータ主体に対して作成された認証リクエストと承認リクエストの合計数の概要を含むJSON ファイルを返します。
これらのイベントはすべて、顧客ごとにフィルタリングされます。


**サンプルをリクエスト**

データアクセスリクエストを送信するAdobe Pass認証識別子を含むJSONをアップロードする必要があります。 適切な形式のJSONの例を確認するには、次のサンプルを参照してください。

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

データ削除リクエストを送信するAdobe Pass認証識別子を含むJSONをアップロードする必要があります。 適切な形式のJSONの例を確認するには、次のサンプルを参照してください。

**サンプルをリクエスト**

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

Delete リクエストの場合：

* データが削除されたレシートのみを共有し、削除されたすべてのデータを含む集約ファイルは共有しません。
* 応答に含まれるレシートには、そのデータ主体に対して見つかった認証トークンと認証トークンの合計数の概要が含まれています。

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

## リクエストをトリガーする方法 {#trigger-req}

お客様がAdobeにプライバシーリクエストを送信する方法は2つあります。

* **手動** - [Privacy Service ユーザーインターフェイス ](#privacy-service-ui)を使用
* **自動** - [Privacy Service API](#privacy-service-api)を使用

### Privacy Service UIを使用して {#privacy-service-ui}

Privacy Service ユーザーインターフェイスにアクセスして使用する方法に関する[完全なチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)は、Adobe I/O サービスを通じてオンラインで利用できます。 さらに、このリンクを使用して、プライバシー規制に関するビデオや記事のライブラリにアクセスできます。 Adobe Experience CloudとGDPR メニューをクリックします。 これにより、多数のビデオが開きます。「GDPR UIの使い方」はその使用方法を説明しています。

UIでは、ユーザーは独自のIMSOrgIDと、各製品のGDPR リクエストの詳細を含むJSONを読み込む必要があります。

### Privacy Service APIを使用することで {#privacy-service-api}

Adobe Experience Platform Privacy Serviceは、プライベートデータに対するアクセス要求/削除要求およびオプトアウト要求を一元的に処理する共通の機能を提供します。

**Privacy Service API ドキュメント**&#x200B;では、Adobeのお客様がAdobe APIと統合する方法について詳しく説明しています。

**Postmanを使用したAPI呼び出しの視覚化（無料のサードパーティ製ソフトウェア）:**

* [GitHub上のPrivacy Service API Postman コレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Postman環境の作成に関するビデオガイド](https://video.tv.adobe.com/v/28832)
* [Postmanで環境とコレクションを読み込む手順](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API パス：**

* PLATFORM ゲートウェイ URL: `https://platform.adobe.io/`
* このAPIのベース パス：`/data/core/privacy/jobs`
* 完全なパスの例：`https://platform.adobe.io/data/core/privacy/jobs/ping`


**必要なヘッダー：**

* すべての呼び出しには、ヘッダー`Authorization`、`x-gw-ims-org-id`、`x-api-key`が必要です。 これらの値の取得方法について詳しくは、**認証チュートリアル**&#x200B;を参照してください。
* リクエスト本文にペイロードを含むすべてのリクエスト（POST、PUT、PATCH呼び出しなど）には、値`application/json`のヘッダー`Content-Type`を含める必要があります。

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
