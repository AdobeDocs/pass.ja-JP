---
title: アクセストークンの取得
description: Dynamic Client Registration API - アクセストークンの取得
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 1%
---
# アクセストークンの取得 {#retrieve-access-token}

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
      <td>/o/client/token</td>
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
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            クライアントアプリケーションのID文字列。
            <br/><br/>
            クライアント ID文字列の取得方法について詳しくは、<a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> クライアント資格情報の取得</a> API ドキュメントを参照してください。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            クライアントアプリケーションの秘密文字列。
            <br/><br/>
            クライアント秘密文字列の取得方法について詳しくは、<a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> クライアント資格情報の取得</a> API ドキュメントを参照してください。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            クライアントアプリケーションがクライアントトークンエンドポイントに使用できる付与タイプ文字列（例：「client_credentials」）。
            <br/><br/>
            付与タイプ文字列の取得方法について詳しくは、<a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> クライアント資格情報の取得</a> API ドキュメントを参照してください。
      </td>
      <td><i>必須</i></td>
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
         application/x-www-form-urlencodedである必要があります。
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
               <td style="background-color: #DEEBFF;">id</td>
               <td>ユーザー活動の追跡に使用できる不透明な識別子。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>クライアントアプリケーションが認証ヘッダーに使用する必要があるアクセストークン値。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>アクセストークンが発行された時間（ミリ秒単位）。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>アクセストークンの有効期限が切れるまでの時間（秒）。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>トークンタイプ（例：「ベアラー」）。</td>
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
                        <li>リクエストには、サポートされていないパラメーター値（付与タイプ以外）が含まれています。</li>
                        <li>リクエストはパラメーターを繰り返します。</li>
                        <li>リクエストには複数の資格情報が含まれています。</li>
                        <li>リクエストは、クライアントの認証に複数のメカニズムを使用します。</li>
                        <li>リクエストの形式が正しくありません。</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>クライアントの資格情報が無効です。クライアントは新しいクライアント資格情報を取得し、再試行する必要があります。 詳しくは、<a href="dynamic-client-registration-apis-retrieve-client-credentials.md"> クライアント資格情報の取得</a> API ドキュメントを参照してください。</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>クライアントアプリケーションは、この承認付与タイプを使用する権限がありません。</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unsupported_grant_type</td>
               <td>認証の付与タイプは、認証サーバーでサポートされていません。</td>
            </tr>
         </table>
      </td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### アクセストークンの取得 {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB 応答 – 成功]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1752148106221,
  "expires_in": 21600,
  "token_type": "bearer"
}
```

>[!TAB 応答 – エラー]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
