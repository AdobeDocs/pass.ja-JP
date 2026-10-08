---
title: パートナー認証リクエストの取得
description: REST API V2 - パートナー認証リクエストの取得
exl-id: 52d8a8e9-c176-410f-92bc-e83449278943
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 1%
---
# パートナー認証リクエストの取得 {#retrieve-partner-authentication-request}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> REST API V2の実装は、[ スロットル メカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md)のドキュメントによって制限されています。

## リクエスト {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/v2/{serviceProvider}/sessions/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>投稿する</td>
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
      <td style="background-color: #DEEBFF;">パートナー</td>
      <td>Adobe Pass認証フローと統合されたシングルサインオンフレームワークを提供するパートナー（Appleなど）の名前。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Body パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        MVPD ログインを実行するアプリケーションの元のドメイン。
        <br/><br/>
        ストリーミングデバイスプラットフォームで値の提供に制限がある場合、アプリケーションは認証セッションを再開し、有効な値を提供する必要があります。
        <br/><br/>
        これは、応答がストリーミングアプリケーションが基本認証フローを進める必要があることを示すフォールバックシナリオの場合に使用されます。
      </td>
      <td><i>必須</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPDの認証フローが完了したときにユーザーエージェントが移動する最後のリダイレクト URL。
        <br/><br/>
        値はURL エンコードする必要があります。
        <br/><br/>
        ストリーミングデバイスプラットフォームで値の提供に制限がある場合、アプリケーションは認証セッションを再開し、有効な値を提供する必要があります。
        <br/><br/>
        これは、応答がストリーミングアプリケーションが基本認証フローを進める必要があることを示すフォールバックシナリオの場合に使用されます。
      </td>
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
      <td style="background-color: #DEEBFF;">コンテンツタイプ</td>
      <td>
         送信されるリソースの許可されたメディアタイプ。
         <br/><br/>
         application/x-www-form-urlencodedである必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>デバイス識別子ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         デバイス情報ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> ヘッダーのドキュメントを参照してください。
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        パートナーメソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> ヘッダーのドキュメントを参照してください。
        <br/><br/>
        パートナーを使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md"> パートナーフローを使用したシングルサインオン </a>のドキュメントを参照してください。</td>
      <td>オプション</td>
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
        応答本文には、認証の実行に必要な次のアクションに関する情報が含まれます。
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
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  認証フローを完了するためにストリーミングデバイスが実行する必要があるアクション。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b>partner_profile</b><br/> ストリーミングデバイスは、提供されたパートナー認証要求を使用して、プロファイルの取得に利用できるパートナー認証応答を取得できます。</li>
                    <li><b>認証</b><br/> パートナーのシングル サインオン フローを続行できない場合、ストリーミング デバイスは基本認証フローにフォールバックできます。<br/> ストリーミングデバイスまたは別のデバイスは、ユーザーエージェントで指定されたURLを開く必要があります。</li>
                    <li><b>再開</b><br/> パートナーのシングル サインオン フローを続行できない場合、ストリーミング デバイスは基本認証フローにフォールバックできます。<br/> ストリーミングデバイスまたは別のデバイスは、不足しているパラメーターを指定し、コードを使用して認証セッションを再開する必要があります。</li>
                    <li><b>authorize</b><br/> ストリーミングデバイスは、決定フローを直接進めることができます。</li>
                  </ul>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  「actionName」属性で指定されたアクションでフローを続行するために、ストリーミングデバイスが実行するインタラクションのタイプ。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b> インタラクティブ </b><br/> ユーザーエージェントを使用して、指定されたURLへのナビゲーションを続行します。</li>
                    <li><b>direct</b><br/> フローは、クライアント実装で使用可能なHTTP クライアントを使用して、指定されたURLへの直接呼び出しを続けます。</li>
                  </ul>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">reasonType</td>
               <td>
                  「actionName」を説明する理由のタイプ。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b>none</b><br/>認証を続行するには、クライアントアプリケーションが必要です。</li>
                    <li><b>認証済み</b><br/> クライアントアプリケーションは、基本的なアクセスフローを通じて既に認証されています。</li>
                    <li><b>劣化</b><br/> クライアントアプリケーションは、劣化したアクセスフローを通じて既に認証されています。</li>
                    <li><b>authenticatedSSO</b><br/> クライアントアプリケーションは、既にシングルサインオンアクセスフローを通じて認証されています。</li>
                    <li><b>pfs_fallback</b><br/>AP-Partner-Framework-Status</a> ヘッダー値が見つからないか無効なため、クライアントアプリケーションは基本認証フローにフォールバックする必要があります。<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md"></li>
                    <li><b>configuration_fallback</b><br/>Adobe Pass バックエンドのパートナーのシングルサインオン設定により、クライアントアプリケーションは基本認証フローにフォールバックする必要があります。</li>
                    <li><b>missing_parameters_fallback</b><br /> パラメーターが見つからないか無効なため、クライアントアプリケーションを再開フローに戻す必要があります。</li>
                  </ul>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    基本認証フローを完了するために指定する必要があるパラメーターがありません。
                    <br/><br/>
                    このフィールドは、パートナーのシングルサインオンフローを続行できない場合に表示されます。
               </td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>クライアントアプリケーションがナビゲートする必要があるURL。</td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">コード</td>
               <td>
                    認証セッションを再開するためにセカンダリアプリケーションで使用できる認証コード。
                    <br/><br/>
                    このフィールドは、パートナーのシングルサインオンフローを続行できない場合に表示されます。
               </td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    Adobe Pass認証システム外のパートナーとの認証フローで使用されるパートナー認証リクエスト。
                    <br/><br/>
                    このフィールドは、パートナーのシングルサインオンフローが続行できる場合に表示されます。
                    <br/><br/>
                    次の属性を持つJSON オブジェクト：
                    <ul>
                        <li><b>type</b><br/>MVPDでサポートされているプロトコルの種類を示します（SAMLのみ）。</li>
                        <li><b> リクエスト </b><br/>SAML リクエスト。</li>
                        <li><b>attributesNames</b><br/>SAML リクエスト属性。</li>
                    </ul>
               </td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>ユーザー活動の追跡に使用できる不透明な識別子。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>オンボーディングプロセス中にID プロバイダーに関連付けられた内部一意のID。</td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部一意のID。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>認証コードが無効になるまでのタイムスタンプ（ミリ秒単位）。</td>
               <td>オプション</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>認証コードが無効になるまでのタイムスタンプ（ミリ秒単位）。</td>
               <td>オプション</td>
            </tr>
         </table>
      </td>
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
      <td>応答本文は、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a> ドキュメントに準拠する追加のエラー情報を提供する場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### &#x200B;1. パートナー認証リクエストの取得

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "partner_profile",
    "actionType": "direct",
    "reasonType": "none",
    "url": "/api/v2/REF30/profiles/sso/Apple",
    "sessionId": "83c046be-ea4b-4581-b5f2-13e56e69dee9",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "authenticationRequest": {
        "type": "saml",
        "request": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG....",
        "attributesNames": ["uid", "NameID", "uniqueId"]
   }
}    
```

>[!ENDTABS]

### &#x200B;2. パートナー認証リクエストを取得しますが、劣化が適用されます

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiJHtkZWdyYWRlZE12cGR9IiwKICAgICAgImV4cGlyYXRpb25EYXRlIiA6ICIyMDI1NDMwNjM2MDAwIgogICAgfQp9
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "degraded",
    "url": "/api/v2/REF30/decisions/authorize/${degradedMvpd}",
    "sessionId": "14d4f239-e3b1-4a4a-b8b3-6395b968a260",
    "mvpd": "${degradedMvpd}",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]

### &#x200B;3. パートナー認証リクエストを取得しますが、AP-Partner-Framework-Status ヘッダー値が欠落または無効なため、基本認証フローにフォールバックします

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImRlbmllZCIKICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHt9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "pfs_fallback",
    "url": "/api/v2/authenticate/REF30/OKTWW2W",
    "code": "OKTWW2W",
    "sessionId": "748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### &#x200B;4. Adobe Pass バックエンドのパートナーシングルサインオン設定により、パートナー認証リクエストを取得しますが、基本認証フローにフォールバックします

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "configuration_fallback",
    "url": "/api/v2/authenticate/REF30/OKTWW2W",
    "code": "OKTWW2W",
    "sessionId": "748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### &#x200B;5. パートナー認証リクエストを取得しますが、パラメーターが不足しているため、基本認証フローにフォールバックします

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/sessions/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK  

Content-Type: application/json;charset=UTF-8

{
    "actionName": "resume",
    "actionType": "direct",
    "reasonType": "missing_parameters_fallback",
    "missingParameters": [
          "redirectUrl"
    ],
    "url": "/api/v2/REF30/sessions/SB7ZRIO",
    "code": "SB7ZRIO",
    "sessionId": "1476173f-5088-43b8-b7c3-8cf3a185de0a",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]
