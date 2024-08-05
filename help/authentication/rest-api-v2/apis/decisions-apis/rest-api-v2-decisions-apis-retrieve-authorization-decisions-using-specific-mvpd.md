---
title: 特定の mvpd を使用した認証決定の取得
description: REST API V2 – 特定の mvpd を使用した認証決定の取得
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 0%

---


# 特定の mvpd を使用した認証決定の取得 {#retrieve-authorization-decisions-using-specific-mvpd}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## リクエスト {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/v2/{serviceProvider}/decisions/authorize/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">本文パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">リソース</td>
      <td>再生する前に MVPD 決定を必要とするリソースのリスト。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="../../../dynamic-client-registration-api.md"> 動的クライアント登録 </a> ドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         送信するリソースに使用できるメディアタイプ。
         <br/><br/>
         application/json にする必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>デバイス識別子ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ドキュメントに記載されています。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         デバイス情報ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> ドキュメントを参照してください。
         <br/><br/>
         アプリケーションのデバイスプラットフォームで有効な値を明示的に指定できる場合は、常に使用することを強くお勧めします。
         <br/><br/>
         指定した場合、Adobe Pass認証バックエンドは、明示的に設定された値を、抽出された値と暗黙的に（デフォルトで）結合します。
         <br/><br/>
         指定しない場合、Adobe Pass認証バックエンドでは、抽出された値が暗黙的に（デフォルトで）使用されます。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         ストリーミングデバイスの IP アドレス。
         <br/><br/>
         サーバーからサーバーへの実装には常に使用することを強くお勧めします。特に、呼び出しがストリーミングデバイスではなくプログラマーサービスによって行われる場合に強くお勧めします。
         <br/><br/>
         クライアントからサーバーへの実装の場合、ストリーミングデバイスの IP アドレスは暗黙的に送信されます。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe件名トークン</td>
      <td>
        Platform ID 方式のシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobeの件名のトークン </a> ドキュメントに記載されています。
        <br/><br/>
        プラットフォーム ID を使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> プラットフォーム ID フローを使用したシングルサインオン </a> ドキュメントを参照してください。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        サービストークンメソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> ドキュメントに記載されています。
        <br/><br/>
        サービストークンを使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md"> サービストークンフローを使用したシングルサインオン </a> ドキュメントを参照してください。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        パートナーメソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> ドキュメントを参照してください。
        <br/><br/>
        パートナーを使用したシングルサインオン有効フローについて詳しくは、<a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md"> パートナーフローを使用したシングルサインオン </a> ドキュメントを参照してください。</td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>ユーザー固有 ID ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a> ドキュメントに記載されています。</td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承諾</td>
      <td>
         クライアントアプリケーションによって受け入れられるメディアタイプ。
         <br/><br/>
         指定する場合は、application/json にする必要があります。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>optional</td>
   </tr>
</table>

## 応答 {#response}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">コード</th>
      <th style="background-color: #EFF2F7; width: 20%;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        応答本文には、決定のリストと追加情報が含まれます。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="../../../enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに準拠したエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="../../../dynamic-client-registration-api.md"> 動的クライアント登録 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>許可されていないメソッド</td>
      <td>
        HTTP メソッドが無効です。クライアントは、リクエストされたリソースに許可されている HTTP メソッドを使用し、再試行する必要があります。 詳しくは、<a href="#request"> リクエスト </a> の節を参照してください。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバー側で問題が発生しました。 応答本文には、<a href="../../../enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
</table>

### 成功 {#success}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>200</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">決定</td>
      <td>
         要素のリストを含む JSON。各要素は次の属性を持ちます。
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">resource</td>
                <td>認証決定が返されるリソースの識別子。</td>
                <td><i>必須</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">serviceProvider</td>
                <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
                <td><i>必須</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpd</td>
                <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
                <td><i>必須</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">authorized</td>
                <td>リソースの決定ステータス。「true」または「false」のいずれかです。</td>
                <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">ソース</td>
               <td>
                  決定ソースに関する情報：
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">値</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd</td>
                        <td>決定は、MVPD 認証エンドポイントによって発行されます。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">分解</td>
                        <td>アクセスが低下した結果、決定が発行されます。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">temppass</td>
                        <td>決定は一時的なアクセスの結果として発行されます。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">代替</td>
                        <td>決定は、ダミーの認証機能の結果として発行されます。</td>
                     </tr>
                  </table>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">トークン</td>
               <td>
                  メディアトークンに関する情報：
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">notBefore</td>
                        <td>メディアトークンが無効となる前のタイムスタンプ。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">notAfter</td>
                        <td>メディアトークンが無効になった後のタイムスタンプ。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">serializedToken</td>
                        <td>Base64 にエンコードされたメディアトークン。</td>
                     </tr>
                  </table>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>決定が無効になる前のタイムスタンプ。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>決定が無効になった後のタイムスタンプ。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">エラー</td>
               <td>このエラーは、「拡張エラーコード <a href="../../../enhanced-error-codes.md"> ドキュメントに従った「拒否」決定に関する追加情報を提供 </a> ます。</td>
               <td>optional</td>
            </tr>
         </table>
      </td>
      <td><i>必須</i></td>
</table>

### エラー {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、401、405、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">エラー</td>
      <td>このエラーは、<a href="../../../enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従った追加情報を提供します。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### 1.通常の mvpd を使用して認証決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
POST /api/v2/REF30/decisions/authorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Response – 利用可能 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!ENDTABS]

### 2.一時パスを使用して認証決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Response – 利用可能 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB  応答 – 開始 ]

```JSON
HTTP/1.1 200 OK
Content-Type: applicationjson
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1695360527896,
                "notBefore": 1695360527896,
                "notAfter": 1695360707896,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB  応答 – 期限切れ ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                 "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB  応答 – 無効な設定 ]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 3.プロモーションの一時パスを使用して承認の決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Response – 利用可能 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB  応答 – 開始 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB  応答 – 期限切れ ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Response - Consumed]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_max_resources_exceeded",
                "message": "Flexible TempPass maximum resources exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB  応答 – 無効な設定 ]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8      
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB  応答 – 無効な ID]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8 
 
{
    "status": 400,
    "code": "temppass_invalid_identity",
    "message": "TempPass is not available for the specified identity.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 4.劣化した mvpd を使用して認証決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
POST /api/v2/REF30/decisions/authorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB  応答 – AuthNAll の低下 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**メモ：** この場合、AuthZAll ルールには「channel」 : 「apastest1」があります

>[!TAB  応答 – AuthZAll の低下 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**メモ：** この場合、AuthZAll ルールには「channel」 : 「apastest1」があります

>[!TAB  応答 – AuthZNone の低下 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**メモ：** この場合、AuthZNone ルールには「channel」 : 「apastest1」があります

>[!TAB  応答 – 期限切れの劣化ルール ]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
