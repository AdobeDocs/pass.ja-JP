---
title: ヘッダー – AP デバイス識別子
description: REST API V2 - ヘッダー – AP デバイス識別子
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# ヘッダー – AP デバイス識別子 {#header-ap-device-identifier}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>AP-Device-Identifier</b> 要求ヘッダーには、クライアント アプリケーションによって作成されたストリーミング デバイスの識別子が含まれています。

## 構文 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
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

<b>&lt;type></b>

デバイス識別子のタイプ。

以下に示すように、サポートされているタイプは 1 つだけです。

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">タイプ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>指紋</td>
      <td>デバイス識別子は、不透明な識別子で構成され、クライアントアプリケーションによって作成されます。</td>
   </tr>
</table>


<b>&lt;identifier></b>

デバイス識別子の `Base64-encoded` 値。

## 例 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
