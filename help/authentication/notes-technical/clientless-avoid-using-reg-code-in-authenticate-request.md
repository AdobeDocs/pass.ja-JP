---
title: /authenticate 要求で'&'reg_code を使用しない
description: /authenticate 要求で'&'reg_code を使用しない
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# /authenticate 要求で&#39;&amp;&#39;reg_code を使用しない {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>



## 問題

IE 9 ブラウザーは「\&amp;reg」を特別なコマンドとして解釈し、® に変換します。

## 説明

`/authenticate` リクエストが次のように構成されている場合…


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


IE ブラウザーによって以下のように解釈され、Adobeに次のフォーマットで送信されます。


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


&#39;&amp;&#39;がないため、要求者\_id は univision®\_code=EKAFMFI と解釈され、Adobeはトークンを関連付ける `regCode` のパラメーターを見つけることができません。  AuthN トークンがまったく作成されない可能性があり、その場合、`/checkauthn` 呼び出しでトークンが見つからない可能性があります。



## 解決策

この問題は、次のいずれかのオプションで解決できます。

1. 他のクエリ文字列パラメーターの間に `&reg_code` パラメーターを使用しないでください。  代わりに、リクエスト URL の最初のクエリ文字列パラメーターに移動し、リクエスト URL を次のようになります。


       &lt;FQDN>authenticate?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   これにより、`&reg` パラメーターが正しく解釈されなくなります。

1. `&amp;reg_code` を使用して `&reg_code` を正規化します。

1. Adobeは、AuthN トークンの作成に失敗した場合、認証呼び出しに応じてエラーコードを 2 番目の画面に送り返す新機能を導入できます。
