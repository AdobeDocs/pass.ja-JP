---
title: ヘッダー – AP-Visitor-Identifier
description: REST API V2 - ヘッダー – AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 1%
---
# ヘッダー – AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>AP-Visitor-Identifier</b> リクエストヘッダーには、Adobe Experience Cloud ソリューション全体で訪問者を一意に識別するためにクライアントアプリケーションで必要な`ECID`が含まれています。

Adobe Pass認証でのECIDの使用について詳しくは、「[Adobe Pass認証でのExperience Cloud IDの使用](../../../../features-premium/analytics/exp-cloud-id-authn.md)」のドキュメントを参照してください。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;visitor_identifier&gt;</td>
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

## 例 {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
