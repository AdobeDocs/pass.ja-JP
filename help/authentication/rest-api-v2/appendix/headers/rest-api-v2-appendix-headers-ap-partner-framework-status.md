---
title: ヘッダー – AP-Partner-Framework-Status
description: REST API V2 - ヘッダー – AP-Partner-Framework-Status
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---


# ヘッダー – AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>AP-Partner-Framework-Status</b> 要求ヘッダーには、シングル サインオン （SSO）を実現するためにパートナーフレームワークから取得した状態情報が含まれています。

## 構文 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

次の属性を含む JSON 要素の `Base64-encoded` 値：

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">属性</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         これは必須の属性です。
         <br/><br/>
         パートナーフレームワークによって返され、アプリケーションによって処理されるユーザー権限ステータス情報。
         <br/><br/>
         これは、次の属性を持つ JSON 要素です。
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">属性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  これは必須の属性です。
                  <br/><br/>
                  これは、次の値を含む列挙です。
                  <br/>
                  <ul>
                     <li>承認済み – ユーザーはアプリケーションが購読情報にアクセスできるようにしました。</li>
                     <li>拒否 – ユーザーが、購読情報にアクセスするためのアプリケーションを拒否しました。</li>
                     <li>保留中 – アプリケーションが購読情報にアクセスできるようにするためのオプションがユーザーによってまだ選択されていません。</li>
                     <li>notDetermined - アプリケーションが購読情報にアクセスできません。</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>エラー</td>
               <td>
                  これはオプションの属性です。
                  <br/><br/>
                  これは、ユーザー権限のステータス情報のクエリ中にパートナーフレームワークエラーがトリガーされた場合に、パートナーフレームワークエラーを渡すために使用できます。
                  <br/><br/>
                  これは、次の属性を持つ JSON 要素です。
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>コード</td>
                        <td>パートナーフレームワークによって定義されたエラーを一意に識別する文字列。</td>
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
         これは必須の属性です。
         <br/><br/>
         パートナーフレームワークによって返され、アプリケーションによって処理される、プロバイダーのログインステータス情報。
         <br/><br/>
         これは、次の属性を持つ JSON 要素です。
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">属性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  これは必須の属性です。
                  <br/><br/>
                  これは、パートナーフレームワークレベルでの認証フロー中に使用される MVPD を識別する mappingId です。
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  これは必須の属性です。
                  <br/><br/>
                  ユーザーがパートナーフレームワークレベルでサポートされている MVPD を使用して正常にログに記録された場合、これは、認証済みユーザープロファイルの有効期限です。
               </td>
            </tr>
            <tr>
               <td>エラー</td>
               <td>
                  これはオプションの属性です。
                  <br/><br/>
                  これは、プロバイダーログインステータス情報のクエリ中にパートナーフレームワークエラーがトリガーされた場合に、パートナーフレームワークエラーを渡すために使用できます。
                  <br/><br/>
                  これは、次の属性を持つ JSON 要素です。
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>コード</td>
                        <td>パートナーフレームワークによって定義されたエラーを一意に識別する文字列。</td>
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
