---
title: ヘッダー – X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 - ヘッダー – X-Roku-Reserved-Roku-Connect-Token
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# ヘッダー – X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>X-Roku-Reserved-Roku-Connect-Token</b> リクエストヘッダーには、Adobe Pass Authentication Systems の外部で動作する ID サービスまたはライブラリから取得した `JWS` または `JWE` として一意のプラットフォーム ID が含まれています。

このヘッダーは、Platform ID メソッドを活用するシングルサインオン（SSO）対応フローで使用するように設計されています。

Platform ID メソッドを活用したシングルサインオン（SSO）有効フローについて詳しくは、[ プラットフォーム ID フローを使用したシングルサインオン ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) ドキュメントを参照してください。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>:&lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

一意のプラットフォーム識別情報を含む、署名済みまたは暗号化された JSON Web トークン（`JWT`）である JSON Web 署名（`JWS`）または JSON Web 暗号化（`JWE`）。

これは、次のプラットフォームで使用できます。

* [Roku SSO クックブック（REST API V2）](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
