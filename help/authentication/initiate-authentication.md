---
title: 認証の開始
description: 認証の開始
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# 認証の開始 {#initiate-authentication}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

>[!NOTE]
>
> REST API 実装は、 [スロットルメカニズム](/help/authentication/throttling-mechanism.md)

## REST API エンドポイント {#clientless-endpoints}

&lt;reggie_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* 実稼動 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* ステージング — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 説明 {#description}

MVPD 選択イベントを通知することで、認証プロセスを開始します。 MVPD から正常な応答を受け取った場合に紐付けされる、Adobe Pass Authentication データベースにレコードを作成します。



| エンドポイント | 呼び出し済み  </br>作成者 | 入力   </br>パラメーター | HTTP  </br>メソッド | 応答 | HTTP  </br>応答 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | AuthN モジュール | 1. requestor_id（必須）</br>2.  mso_id（必須）</br>3.  reg_code（必須）</br>4.  domain_name（必須）</br>5.  noflash=true -  </br>    （必須、残余パラメータ）</br>6.  no_iframe=true（必須、残差パラメータ）</br>7.  追加のパラメーター（オプション）</br>8.  redirect_url（必須） | GET | Login Web App が MVPD ログインページにリダイレクトされます。 | 完全なリダイレクト実装の場合は 302 |

{style="table-layout:auto"}


| 入力パラメーター | 説明 |
| --- | --- |
| requestor_id | この操作が有効なプログラマー要求元。 |
| mso_id | この操作が有効な MVPD ID です。 |
| reg_code | Reggie サービスによって生成された登録コード。 |
| domain_name | 元のドメイン。 |
| redirect_url | 認証完了後のログイン Web アプリのリダイレクト URL。 |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**重要：必須のパラメーター —** クライアント側の実装に関係なく、上記のすべてのパラメーターは必須です。
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
>**重要：オプションのパラメーター**
>
>また、呼び出しには、次のような他の機能を有効にするオプションのパラメーターを含めることができます。
>
> * generic\_data - [プロモーション TempPass](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **メモ** {#notes}

* の値 `domain_name` パラメーターは、Adobe Pass Authentication に登録されているドメイン名の 1 つに設定する必要があります。 詳しくは、 [登録と初期化](/help/authentication/programmer-overview.md).

* [/authenticate request で&#39;&amp;&#39;reg\_code を使用しない（テクニカルノート）](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* The `redirect_url` パラメーターは、順序の最後のパラメーターである必要があります

* の値 `redirect_url` パラメーターは URL エンコードされている必要があります
