---
title: Header - Adobeの件名トークン
description: REST API V2 - ヘッダー – Adobe – 件名 – トークン
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# Header - Adobeの件名トークン {#header-adobe-subject-token}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>Platform-Subject-Token</b> リクエスト ヘッダーには、`JWS` または `JWE` として一意のAdobe ID が含まれています。この ID は、Adobe Pass認証システムの外部で動作している ID サービスまたはライブラリから取得されたものです。

このヘッダーは、Platform ID メソッドを活用するシングルサインオン（SSO）対応フローで使用するように設計されています。

Platform ID メソッドを活用したシングルサインオン（SSO）有効フローについて詳しくは、[ プラットフォーム ID フローを使用したシングルサインオン ](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) ドキュメントを参照してください。

## 構文 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe – 件名 – トークン </b>: &lt;unique_platform_identifier&gt;</td>
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

* [Amazon SSO クックブック（REST API V2）](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## 例 {#examples}

次のプラットフォームについて説明した例を参照してください。

* [Amazon SSO クックブック（REST API V2）](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
