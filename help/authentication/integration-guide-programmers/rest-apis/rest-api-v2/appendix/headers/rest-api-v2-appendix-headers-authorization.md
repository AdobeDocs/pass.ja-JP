---
title: ヘッダー – 認証
description: REST API V2 - ヘッダー – 認証
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '121'
ht-degree: 1%
---

# ヘッダー – 認証 {#header-authorization}

>[!NOTE]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>Authorization</b> リクエストヘッダーには、クライアントアプリケーションがAdobe Passで保護されたAPIにアクセスするために必要な`Bearer` アクセストークンが含まれています。

Adobe Passで保護されたAPIにアクセスする仕組みについて詳しくは、[Dynamic Client Registration Overview](../../../rest-api-dcr/dynamic-client-registration-overview.md)のドキュメントを参照してください。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>認証</b>: ベアラー&lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>ヘッダータイプ</td>
      <td>リクエストヘッダー</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>はい</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;access_token></b>

アクセストークンの値は、制限された有効期間（24時間など）を持つ不透明な値で、[&#x200B; アクセストークンの取得](../../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントで説明されているように、Adobe Passから取得する必要があります。

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
