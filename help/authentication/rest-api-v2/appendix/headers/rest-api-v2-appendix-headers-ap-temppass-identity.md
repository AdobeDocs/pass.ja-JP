---
title: ヘッダー – AP-TempPass-Identity
description: REST API V2 - ヘッダー – AP-TempPass-Identity
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 2%

---


# ヘッダー – AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>AP-TempPass-Identity</b> リクエストヘッダーには、プロモーション TempPass の実現に使用されるユーザー ID 情報が含まれています。

## 構文 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;user_identity_information&gt;</td>
   </tr>
   <tr>
      <td>ヘッダータイプ</td>
      <td>リクエストヘッダー</td>
   </tr>
   <tr>
      <td>標準</td>
      <td>不可</td>
   </tr>
</table>

## ディレクティブ {#directives}

<b>&lt;user_identity_information></b>

プロモーションの一時アクセスを付与する必要があるエンドユーザーに関連付けられたユーザー ID 情報の `Base64-encoded` 値。

## 例 {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
