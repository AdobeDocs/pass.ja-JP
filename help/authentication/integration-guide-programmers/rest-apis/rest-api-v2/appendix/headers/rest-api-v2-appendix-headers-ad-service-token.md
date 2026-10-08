---
title: ヘッダー – AD-Service-Token
description: REST API V2 - ヘッダー – AD-Service-Token
exl-id: 856f76fc-cde6-4b3f-81f7-deaa0df015dc
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%
---
# ヘッダー – AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>AD-Service-Token</b> リクエストヘッダーには、Adobe Pass認証システム外で実行されているID サービスから取得した`JWS`という一意のユーザーIDが含まれています。

このヘッダーは、サービストークン方式を利用したシングルサインオン（SSO）対応フローで使用するように設計されています。

サービストークン方式を使用したシングルサインオン（SSO）対応フローの詳細については、[&#x200B; サービストークンのフローを使用したシングルサインオン &#x200B;](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)のドキュメントを参照してください。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b>: &lt;unique_user_identifier&gt;</td>
   </tr>
   <tr>
      <td>ヘッダータイプ</td>
      <td>リクエストヘッダー</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>いいえ</td>
   </tr>
</table>

## 指令 {#directives}

<b>unique_user_identifier</b>

一意のユーザーID情報を含む署名済みJSON Web トークン （`JWT`）であるJSON Web署名（`JWS`）。

`JWT`には次の属性があります。

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">属性</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>シングルサインオン（SSO）を実現するための外部ID サービスをアプリケーションに提供するエンティティに関連付けられた一意のID。</td>
   </tr>
   <tr>
      <td>サブ</td>
      <td>外部ID サービスから返されたユーザーの一意のID。</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>オーディエンスは「Adobe」であるべきです。</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>現在のJWTのタイムスタンプで発行された値。</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>現在のJWTの有効期限タイムスタンプ。</td>
   </tr>
</table>

`JWT`は、`SHA256withRSA` アルゴリズムを使用して署名する必要があります。

`JWT`は、RSA秘密鍵のペアの一部である秘密鍵で署名する必要があります。これは、外部ID サービスによって管理される公開鍵です。

前述の秘密鍵で署名された`JWT` トークンを認識するには、そのペアの公開鍵をAdobe Pass Authenticationに引き渡す必要があります。

## 例 {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
