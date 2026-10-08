---
title: クライアント資格情報の取得
description: Dynamic Client Registration API - クライアント資格情報の取得
exl-id: 0b39768b-25b8-47b9-8080-59c56fb829fb
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%
---
# クライアント資格情報の取得 {#retrieve-client-credentials}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> 動的クライアント登録APIの実装は、[&#x200B; スロットル メカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)のドキュメントによって制限されています。

## リクエスト {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/o/client/register</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>投稿する</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Body パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">software_statement</td>
      <td>
            登録済みアプリケーションに関連付けられているソフトウェアステートメントが、<a href="https://experience.adobe.com/#/pass/authentication">Adobe Pass TVE Dashboard</a>から作成およびダウンロードされました。
            <br/><br/>
            登録アプリケーションの管理については、<a href="../dynamic-client-registration-overview.md">動的クライアント登録の概要</a>のドキュメントを参照してください。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirect_uri</td>
      <td>認証フローが完了したときにユーザーエージェントが移動する場所に関連付けられたリダイレクト URI。</td>
      <td>オプション</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">コンテンツタイプ</td>
      <td>
         送信されるリソースの許可されたメディアタイプ。
         <br/><br/>
         application/json;charset=utf-8である必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         デバイス情報ペイロードの生成については、<a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> ドキュメントを参照してください。
         <br/><br/>
         アプリケーションのデバイスプラットフォームで有効な値の明示的なプロビジョニングが可能な場合は、必ず使用することを強くお勧めします。
         <br/><br/>
         指定すると、Adobe Pass認証バックエンドは、明示的に設定された値と抽出された値を暗黙的に（デフォルトで）マージします。
         <br/><br/>
         指定しない場合、Adobe Pass認証バックエンドは、抽出された値を暗黙的に（デフォルトで）使用します。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承認</td>
      <td>
         クライアントアプリケーションが受け入れたメディアタイプ。
         <br/><br/>
         指定する場合は、application/json;charset=utf-8である必要があります。
      </td>
      <td>オプション</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>オプション</td>
   </tr>
</table>

## 応答 {#response}

### 成功 {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>201</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">コンテンツタイプ</td>
      <td>アプリケーション/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         次の属性を持つJSON オブジェクト：
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id</td>
               <td>クライアントアプリケーションのID文字列。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_secret</td>
               <td>クライアントアプリケーションの秘密文字列。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">client_id_issued_at</td>
               <td>クライアント アプリケーション IDが発行された時刻。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">redirect_uris</td>
               <td>クライアントアプリケーションがリダイレクトベースのフローで使用できるリダイレクト URI文字列の配列。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">grant_types</td>
               <td>クライアントアプリケーションがクライアントトークンエンドポイントに使用できる付与タイプ文字列。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">範囲</td>
               <td>クライアントアプリケーションが使用できるAdobe Pass認証APIを定義するスコープ文字列。</td>
               <td><i>必須</i></td>
            </tr>
         </table>
      </td>
      <td><i>必須</i></td>
</table>

### エラー {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">コンテンツタイプ</td>
      <td>アプリケーション/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">エラー</td>
      <td>
        使用可能な値は次のとおりです。
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">値</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    次のいずれかの理由により、リクエストは無効です：
                    <ul>
                        <li>リクエストに必要なパラメーターが不足しています。</li>
                        <li>リクエストには、サポートされていないパラメーター値が含まれています。</li>
                        <li>リクエストはパラメーターを繰り返します。</li>
                        <li>リクエストの形式が正しくありません。</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_redirect_uri</td>
               <td>リクエストには、無効なリダイレクト URIの値が含まれています。</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_software_statement</td>
               <td>リクエストには、無効なソフトウェアステートメントの値が含まれています。</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unapproved_software_statement</td>
               <td>このリクエストには、Adobe Pass Authentication Serverによる使用が承認されていないソフトウェアステートメントの値が含まれています。</td>
            </tr>
         </table>
      </td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### クライアント資格情報の取得 {#samples-retrieve-client-credentials}

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /o/client/register HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/json
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

{
    "software_statement": "eyJhbGciOiJSUzI1NiJ9.
        eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
        bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
        aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
        GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
        zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
        5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
        fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
        U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
        IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
    "redirect_uri": "adobepass://com.programmer"  
 }
```

>[!TAB 応答 – 成功]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "client_id": "s6BhdRkqt3",
    "client_secret": "t7AkePiru4",
    "redirect_uris": [
        "app://com.programmer.adobe#sdasdsadas"
    ],
    "grant_types": [
        "client_credentials"
    ],
    "scopes": [
        "api:client:v2"
    ],
    "client_id_issued_at": 1723227212
}
```

>[!TAB 応答 – エラー]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
