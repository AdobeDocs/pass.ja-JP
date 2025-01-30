---
title: 認証の開始
description: 認証の開始
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# （レガシー）認証の開始 {#initiate-authentication}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

>[!NOTE]
>
> REST API の実装には、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) という制限があります。

## REST API エンドポイント {#clientless-endpoints}

&lt; レジストリ_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* 実稼動 – [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 説明 {#description}

MVPD選択イベントを通知して認証プロセスを開始します。 Adobe Pass認証データベースにレコードを作成します。これは、MVPDから応答が成功すると紐付けされます。



| エンドポイント | 呼び出 </br> 元 | 入力   </br> パラメーター | HTTP </br> メソッド | 応答 | HTTP </br>Response |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | AuthN モジュール | 1. requestor_id （必須） </br>2.  mso_id （必須） </br>3.  reg_code （必須） </br>4.  domain_name （必須） </br>5.  noflash=true - </br>    （必須、残差パラメーター） </br>6.  no_iframe=true （必須、残差パラメーター） </br>7.  追加のパラメーター（オプション） </br>8。  redirect_url （必須） | GET | ログイン Web アプリケーションは、MVPDのログインページにリダイレクトされます。 | 完全なリダイレクト実装用の 302 |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --- | --- |
| requestor_id | この操作が有効なプログラマ要求元。 |
| mso_id | この操作が有効なMVPD ID。 |
| reg_code | Reggie サービスによって生成される登録コード。 |
| domain_name | 元のドメイン。 |
| redirect_url | 認証完了後のログイン Web アプリケーション リダイレクト URL。 |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**重要：必須パラメーター –** クライアントサイドの実装に関係なく、上記のすべてのパラメーターは必須です。
>
>
>例：
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**重要：オプションパラメーター**
>
>呼び出しには、次のような他の機能を有効にするオプションのパラメーターも含めることができます。
>
> * generic\_data - [ プロモーション TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass) を使用できるようにします
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **備考** {#notes}

* `domain_name` パラメーターの値は、Adobe Pass Authentication に登録されているドメイン名のいずれかに設定されている必要があります。

* [/authenticate リクエストで「&amp;&#39;reg\_code」を使用するのを避ける（テクニカルノート）](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* `redirect_url` パラメーターは最後でなければなりません

* `redirect_url` パラメーターの値は URL エンコードする必要があります
