---
title: ヘッダー – AD-Service-Token
description: REST API V2 - ヘッダー – AD-Service-Token
exl-id: 856f76fc-cde6-4b3f-81f7-deaa0df015dc
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# ヘッダー – AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>AD-Service-Token</b> リクエストヘッダーには、Adobe Pass Authentication Systems の外部で動作する ID サービスから取得した一意のユーザー ID`JWS` 含まれています。

このヘッダーは、サービストークンメソッドを活用するシングルサインオン（SSO）対応フローで使用するように設計されています。

サービストークンメソッドを活用したシングルサインオン（SSO）有効フローについて詳しくは、[ サービストークンフローを使用したシングルサインオン ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md) ドキュメントを参照してください。

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
      <td>標準</td>
      <td>不可</td>
   </tr>
</table>

## ディレクティブ {#directives}

<b>unique_user_identifier</b>

JSON web 署名（`JWS`）：一意のユーザー識別情報を含む署名済み JSON web トークン（`JWT`）です。

`JWT` には次の属性があります。

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">属性</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>が</td>
      <td>シングルサインオン（SSO）を実現するためにアプリケーションに外部 ID サービスを提供するエンティティに関連付けられた一意の ID。</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>外部 ID サービスから返されるユーザーの一意の ID。</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>オーディエンス（「Adobe」にする必要があります）。</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>現在の JWT のタイムスタンプ時に発行されたもの。</td>
   </tr>
   <tr>
      <td>費用</td>
      <td>現在の JWT の有効期限タイムスタンプ。</td>
   </tr>
</table>

`JWT` は、アルゴリズムを使用して署名する必要 `SHA256withRSA` あります。

`JWT` は、RSA 秘密鍵のペアの一部である秘密鍵で署名する必要があります。秘密鍵 – 外部 ID サービスによって管理される公開鍵。

そのペアの公開鍵をAdobe Pass認証に引き継いで、前述の秘密鍵で署名されたトークン `JWT` 認識できるようにする必要があります。

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
