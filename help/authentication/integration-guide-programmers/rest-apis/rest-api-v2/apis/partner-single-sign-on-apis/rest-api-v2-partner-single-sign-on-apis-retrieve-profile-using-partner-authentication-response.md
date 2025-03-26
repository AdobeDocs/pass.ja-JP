---
title: パートナー認証応答を使用したプロファイルの作成と取得
description: REST API V2 - パートナー認証応答を使用したプロファイルの作成と取得
exl-id: cae260ff-a229-4df7-bbf9-4cdf300c0f9a
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 1%

---

# パートナー認証応答を使用したプロファイルの作成と取得 {#create-and-retrieve-profile-using-partner-authentication-response}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

## リクエスト {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/v2/{serviceProvider}/profiles/sso/{partner}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">パートナー</td>
      <td>Adobe Pass認証フローと統合されたシングルサインオンフレームワークを提供するパートナー（例：Apple）の名前。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">SAMLResponse</td>
      <td>
        パートナープロファイルの作成と保存に必要なユーザーメタデータが含まれるパートナー認証応答。
        <br/><br/>
        値は Base64 でエンコードし、その後 URL エンコードする必要があります。
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
      <td>ベアラートークンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         送信するリソースに使用できるメディアタイプ。
         <br/><br/>
         application/x-www-form-urlencoded である必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>デバイス識別子ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> ヘッダードキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         デバイス情報ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> ヘッダーのドキュメントを参照してください。
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        パートナーメソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a> ヘッダードキュメントを参照してください。
        <br/><br/>
        パートナーを使用したシングルサインオン有効フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md"> パートナーフローを使用したシングルサインオン </a> ドキュメントを参照してください。</td>
      <td>optional</td>
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

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>201</td>
      <td>作成日時</td>
      <td>
        応答本文には、有効なプロファイルのマップが含まれています。このマップは空の場合があります。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="../../../rest-api-dcr/dynamic-client-registration-overview.md"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
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
        サーバー側で問題が発生しました。 応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
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
      <td>201</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">プロファイル</td>
      <td>
         キーと値のペアのマップを含む JSON。
         <br/><br/>
         キー要素は、次の値で定義されます。
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">値</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
               <td><i>必須</i></td>
            </tr>
         </table>
         value 要素は、次の属性で定義されます。
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>プロファイルが無効になる前のタイムスタンプ。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>プロファイルが無効になった後のタイムスタンプ。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">発行者</td>
               <td>
                  プロファイルを所有するエンティティ。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b>Apple</b><br/> プロファイルは次の結果として作成されました：パートナーAppleを使用したシングル サインオン。</li>
                  </ul>
               </td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">タイプ</td>
               <td>
                  プロファイルのタイプ。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b>appleSSO</b><br/> プロファイルは次の結果として作成されました：パートナーのAppleを使用したシングルサインオン。</li>
                  </ul>
               </td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">属性</td>
               <td>
                    キーと値のペアのマップを含む JSON。
                    <br/><br/>
                    キー要素は、ユーザーメタデータ属性で定義され、次のいずれかになります。
                    <ul>
                        <li>必須（「userID」など）</li>
                        <li>非必須（「zip」、「householdID」、「maxRating」など）。</li>
                    </ul>
                    属性の値には次の種類があります。
                    <ul>
                        <li>シンプル</li>
                        <li>list</li>
                        <li>マップ</li>
                    </ul>
                    ユーザーメタデータは、認証フローが完了すると使用可能になりますが、MVPDと問題の特定のメタデータ属性に応じて、特定のメタデータ属性が認証フロー中に更新される場合があります。
               </td>
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
      <td>400、401、405、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### 1. パートナー認証応答を使用したプロファイルの作成と取得

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiQ2FibGV2aXNpb24iLAogICAgICAiZXhwaXJhdGlvbkRhdGUiIDogIjIwMjU0MzA2MzYwMDAiCiAgICB9Cn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "Cablevision": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Apple",
            "type": "appleSSO",
            "attributes": {
                "userID": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                },
                "householdID": {
                    "value": "BASE64_value_householdId",
                    "state": "plain"
                },
                "zip": {
                    "value": "BASE64_value_zip",
                    "state": "enc"
                }       
            }
        }
     }
}  
```

>[!ENDTABS]

### 2. パートナー認証応答を使用してプロファイルを作成および取得するが、低下が適用される

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAiYWNjZXNzU3RhdHVzIjogImdyYW50ZWQiCiAgICB9LAogICAgImZyYW1ld29ya1Byb3ZpZGVySW5mbyIgOiB7CiAgICAgICJpZCIgOiAiJHtkZWdyYWRlZE12cGR9IiwKICAgICAgImV4cGlyYXRpb25EYXRlIiA6ICIyMDI1NDMwNjM2MDAwIgogICAgfQp9
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "${degradedMvpd}": {
            "notBefore": 1706636062704,
            "notAfter": 1706696062704,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes": {
                "userID": {
                    "value": "95cf93bcd183214ac9e4433153cb8a9d180a463128c0a5d26f202e8c",
                    "state": "plain"
                }
            }
        }
   }
}
```

>[!ENDTABS]
