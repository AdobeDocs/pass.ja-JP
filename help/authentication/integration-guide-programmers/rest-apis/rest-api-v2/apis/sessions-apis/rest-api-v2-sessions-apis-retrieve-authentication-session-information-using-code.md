---
title: コードを使用した認証セッションの取得
description: REST API V2 - コードを使用した認証セッションの取得
exl-id: 5cc209eb-ee6b-4bb9-9c04-3444408844b7
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 3%
---
# コードを使用した認証セッションの取得 {#retrieve-authentication-session-using-code}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> REST API V2の実装は、[ スロットル メカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)のドキュメントによって制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)にもアクセスしてください。

## リクエスト {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/v2/{serviceProvider}/sessions/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部一意のID。</td>
      <td><i>必須</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">コード</td>
      <td>ストリーミングデバイスで認証セッションを作成した後に取得された認証コード。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンのペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         ストリーミングデバイスのIP アドレス。
         <br/><br/>
         特に、ストリーミングデバイスではなくプログラマーサービスによって呼び出しが行われる場合は、サーバーからサーバーへの実装に常に使用することを強くお勧めします。
         <br/><br/>
         クライアントからサーバーへの実装では、ストリーミングデバイスのIP アドレスが暗黙的に送信されます。
      </td> 
      <td>オプション</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        訪問者識別子ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a> ヘッダーのドキュメントを参照してください。
      <td>オプション</td>
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

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        応答本文は、認証セッションに関する情報を含む。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>不正なリクエスト</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して、もう一度試す必要があります。 応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a>のドキュメントに準拠するエラー情報が含まれる場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未承認</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得し、再試行する必要があります。 詳しくは、<a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">動的クライアント登録の概要</a>のドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>メソッドは許可されていません</td>
      <td>
        HTTP メソッドが無効です。クライアントは、リクエストされたリソースに対して許可されているHTTP メソッドを使用して、再試行する必要があります。 詳しくは、「<a href="#request"> リクエスト </a>」の節を参照してください。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバーサイドで問題が発生しました。 応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a>のドキュメントに準拠するエラー情報が含まれる場合があります。
      </td>
   </tr>
</table>

### 成功 {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>200</td>
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
               <td style="background-color: #DEEBFF;">existingParameters</td>
               <td>既に指定された既存のパラメーター。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>認証フローを完了するために指定する必要があるパラメーターがありません。</td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">デバイス</td>
               <td>実際のストリーミングデバイスに関連するデバイス情報。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>認証コードが無効になるまでのタイムスタンプ（ミリ秒単位）。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>認証コードが無効になるまでのタイムスタンプ（ミリ秒単位）。</td>
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
      <td>400, 401, 405, 500</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
            応答本文は、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a> ドキュメントに準拠する追加のエラー情報を提供する場合があります。
            <br/><br/>
            クライアントアプリケーションは、このAPIによって最も一般的に返されるエラーコードを適切に処理できるエラー処理メカニズムを実装する必要があります。
            <ul>
                <li>invalid_authentication_session</li>
                <li>invalid_parameter_code</li>
                <li>など。</li>
            </ul>
            上記のリストは網羅的ではありません。 クライアントアプリケーションは、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">公開ドキュメント </a>で定義されているすべての強化エラーコードを処理できる必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### &#x200B;1. パラメーターを欠落せずに認証セッションを取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{        
    "existingParameters": {
        "mvpd": "apassidp",
        "domain": "adobe.com"
        "redirectUrl": "https://www.adobe.com",
        "serviceProvider": "REF30"        
    },
    "device": {
        "type": "Desktop",
        "model": null,
        "version": {
            "major": 0,
            "minor": 0,
            "patch": 0,
            "profile": ""
        },
    "hardware": {
      "name": null,
      "vendor": "Apple",
      "version": {
        "major": 0,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "manufacturer": "Apple"
    },
    "operatingSystem": {
      "name": "macOS",
      "family": "macOS",
      "vendor": "Apple",
      "version": {
        "major": 10,
        "minor": 15,
        "patch": 7,
        "profile": ""
      }
    },
    "browser": {
      "name": "Chrome",
      "vendor": "Google",
      "version": {
        "major": 140,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36",
      "originalUserAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
    },
    "display": {
      "width": 0,
      "height": 0,
      "ppi": 0,
      "name": "DISPLAY",
      "vendor": null,
      "version": null,
      "diagonalSize": null
    },
    "applicationId": null,
    "connection": {
      "ipAddress": "...",
      "port": "55161",
      "secure": false,
      "type": null
    }
    }
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"    
}
```

>[!ENDTABS]

### &#x200B;1. パラメーターが欠落している認証セッションの取得

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
  "missingParameters": [
    "mvpd"
  ],
  "existingParameters": {
    "redirectUrl": "https://adobe.com",
    "domainName": "adobe.com",
    "serviceProvider": "REF30"
  },
  "device": {
    "type": "Desktop",
    "model": null,
    "version": {
      "major": 0,
      "minor": 0,
      "patch": 0,
      "profile": ""
    },
    "hardware": {
      "name": null,
      "vendor": "Apple",
      "version": {
        "major": 0,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "manufacturer": "Apple"
    },
    "operatingSystem": {
      "name": "macOS",
      "family": "macOS",
      "vendor": "Apple",
      "version": {
        "major": 10,
        "minor": 15,
        "patch": 7,
        "profile": ""
      }
    },
    "browser": {
      "name": "Chrome",
      "vendor": "Google",
      "version": {
        "major": 140,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36",
      "originalUserAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
    },
    "display": {
      "width": 0,
      "height": 0,
      "ppi": 0,
      "name": "DISPLAY",
      "vendor": null,
      "version": null,
      "diagonalSize": null
    },
    "applicationId": null,
    "connection": {
      "ipAddress": "...",
      "port": "3061",
      "secure": false,
      "type": null
    }
  },
  "notBefore": "1761299929958",
  "notAfter": "1761301729958"
}
```

>[!ENDTABS]
