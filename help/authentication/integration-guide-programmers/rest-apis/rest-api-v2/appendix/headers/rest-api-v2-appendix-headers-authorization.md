---
title: ヘッダー – 認証
description: REST API V2 - ヘッダー – 認証
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# ヘッダー – 認証 {#header-authorization}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>Authorization</b> リクエストヘッダーには、Adobe Passで保護された API にアクセスするためにクライアントアプリケーションで必要な `Bearer` アクセストークンが含まれています。

Adobe Passで保護された API へのアクセスの仕組みについて詳しくは、[Dynamic Client Registration Overview](../../../rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## 構文 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Authorization</b>: ベアラー &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>ヘッダータイプ</td>
      <td>リクエストヘッダー</td>
   </tr>
   <tr>
      <td>標準</td>
      <td>はい</td>
   </tr>
</table>

## ディレクティブ {#directives}

<b>&lt;access_token></b>

アクセストークン値は、有効期限が限られている opaque 値（24 時間など）で、[ アクセストークンの取得 ](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントに記載されているように、Adobe Passから取得する必要があります。

## 例 {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
