---
title: ヘッダー – AP-TempPass-Identity
description: REST API V2 - ヘッダー – AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 3%

---


# ヘッダー – AP-TempPass-Identity {#header-ap-temppass-identity}

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
