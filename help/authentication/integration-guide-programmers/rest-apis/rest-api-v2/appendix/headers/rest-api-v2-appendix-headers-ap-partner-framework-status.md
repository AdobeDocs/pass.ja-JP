---
title: ヘッダー – AP-Partner-Framework-Status
description: REST API V2 - ヘッダー – AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%
---
# ヘッダー – AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>AP-Partner-Framework-Status</b> リクエストヘッダーには、シングルサインオン （SSO）を実現するためにパートナーフレームワークから取得したステータス情報が含まれています。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

次の属性を含むJSON要素の`Base64-encoded`値：

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">属性</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         これは必須属性です。
         <br/><br/>
         パートナーフレームワークによって返され、アプリケーションによって処理されるユーザー権限のステータス情報。
         <br/><br/>
         これは、次の属性を持つJSON要素です。
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">属性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  これは必須属性です。
                  <br/><br/>
                  これは、次の値を持つ列挙です。
                  <br/>
                  <ul>
                     <li><b>付与</b><br/> ユーザーはアプリケーションにサブスクリプション情報へのアクセスを許可しました。</li>
                     <li><b>拒否</b><br/> ユーザーは、サブスクリプション情報にアクセスするアプリケーションを拒否しました。</li>
                     <li><b>制限付き</b><br/> アプリケーションはサブスクリプション情報にアクセスできません。</li>
                     <li><b>notDetermined</b><br/> アプリケーションによるサブスクリプション情報へのアクセスを許可するかどうかをユーザーが選択していません。</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>エラー</td>
               <td>
                  これはオプションの属性です。
                  <br/><br/>
                  これは、ユーザー権限のステータス情報のクエリ中にトリガーされる場合にパートナーフレームワークエラーを渡すために使用できます。
                  <br/><br/>
                  これは、次の属性を持つJSON要素です。
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>コード</td>
                        <td>パートナーフレームワークで定義されたエラーを一意に識別する文字列。</td>
                     </tr>
                     <tr>
                        <td>メッセージ</td>
                        <td>パートナーフレームワークで定義されたエラーの説明を含む文字列。</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         これは必須属性です。
         <br/><br/>
         パートナーフレームワークによって返され、アプリケーションによって処理されるプロバイダーログインステータス情報。
         <br/><br/>
         これは、次の属性を持つJSON要素です。
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">属性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  これは必須属性です。
                  <br/><br/>
                  これは、パートナーフレームワークレベルで認証フロー中に使用されるMVPDを識別するmappingIdです。
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  これは必須属性です。
                  <br/><br/>
                  パートナーフレームワークレベルでサポートされているMVPDを使用してユーザーが正常にログに記録された場合、これは認証済みユーザープロファイルの有効期限です。
                  <br/><br/>
                  これは、文字列として表されるUnixのエポック（例えば「1735689600000」）からミリ秒単位のタイムスタンプである必要があります。
               </td>
            </tr>
            <tr>
               <td>エラー</td>
               <td>
                  これはオプションの属性です。
                  <br/><br/>
                  これは、プロバイダーログインステータス情報のクエリ中にトリガーされる場合にパートナーフレームワークエラーを渡すために使用できます。
                  <br/><br/>
                  これは、次の属性を持つJSON要素です。
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>コード</td>
                        <td>パートナーフレームワークで定義されたエラーを一意に識別する文字列。</td>
                     </tr>
                     <tr>
                        <td>メッセージ</td>
                        <td>パートナーフレームワークで定義されたエラーの説明を含む文字列。</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## 例 {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
