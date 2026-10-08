---
title: /authenticate リクエストで「&」 reg_codeを使用しない
description: /authenticate リクエストで「&」 reg_codeを使用しない
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%
---
# （レガシー） /authenticate リクエストで「&amp;」 reg_codeを使用しない {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>



## イシュー

IE 9 ブラウザーは&#39;\&amp;reg&#39;を特殊コマンドとして解釈し、それを®に変換します。

## 説明

`/authenticate` リクエストが次のように構成されている場合…


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...以下のようなIE ブラウザーによって解釈され、次のフォーマットでAdobeに送信されます。


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


「&amp;」がないので、依頼者\_idはunivision®\_code=EKAFMFIと解釈され、Adobeはトークンを関連付ける`regCode` パラメーターを見つけません。  AuthN トークンがまったく作成されない可能性があります。この場合、`/checkauthn`呼び出しではトークンが見つかりません。



## Solution

次のいずれかのオプションで、この問題を解決する必要があります。

1. 他のクエリ文字列パラメーター間で`&reg_code` パラメーターを使用しないでください。  代わりに、リクエスト URLの最初のクエリ文字列パラメーターに移動し、リクエスト URLを次のように設定します。


       &lt;FQDN>authenticate?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   この方法では、`&reg` パラメーターが正しく解釈されません。

1. `&reg_code`を`&amp;reg_code`を使用するように正規化します。

1. Adobeでは、AuthN トークンの作成に失敗した場合、認証呼び出しに応じてエラーコードを2番目の画面に送り返す新機能を導入できました。
