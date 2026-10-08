---
title: Header - Adobe-Subject-Token
description: REST API V2 - Header - Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%
---
# Header - Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>Adobe-Subject-Token</b> リクエストヘッダーには、Adobe Pass認証システム外で動作するID サービスまたはライブラリから取得した`JWS`または`JWE`という一意のプラットフォーム IDが含まれています。

このヘッダーは、Platform ID メソッドを活用したシングルサインオン（SSO）対応フローで使用するように設計されています。

Platform ID メソッドを使用したシングルサインオン（SSO）対応フローについて詳しくは、「[Platform ID フローを使用したシングルサインオン &#x200B;](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)」のドキュメントを参照してください。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>unique_platform_identifier</b>

一意のプラットフォーム ID情報を含む、署名済みまたは暗号化されたJSON Web トークン （`JWT`）であるJSON Web署名（`JWS`）またはJSON Web暗号化（`JWE`）。

これは、次のプラットフォームで使用できます。

* [Amazon SSO クックブック （REST API V2）](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## 例 {#examples}

次のプラットフォームについて説明されている例を参照してください。

* [Amazon SSO クックブック （REST API V2）](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
