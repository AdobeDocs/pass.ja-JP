---
title: ヘッダー – AP-Visitor-Identifier
description: REST API V2 - ヘッダー – AP-Visitor-Identifier
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# ヘッダー – AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>AP-Visitor-Identifier</b> リクエストヘッダーには、Adobe Experience Cloud ソリューション全体で訪問者を一意に識別するためにクライアントアプリケーションで必要な `ECID` が含まれています。

Adobe Pass認証での ECID の使用について詳しくは、[Adobe Pass認証でのExperience Cloud ID の使用 &#x200B;](../../../../features-premium/analytics/exp-cloud-id-authn.md) ドキュメントを参照してください。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>:&lt;visitor_identifier&gt;</td>
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

## 例 {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
