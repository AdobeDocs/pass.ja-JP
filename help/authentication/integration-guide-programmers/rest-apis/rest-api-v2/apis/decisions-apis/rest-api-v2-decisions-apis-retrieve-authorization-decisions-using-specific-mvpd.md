---
title: 特定の mvpd を使用した認証決定の取得
description: REST API V2 – 特定の mvpd を使用した認証決定の取得
exl-id: e8889395-4434-4bec-a212-a8341bb9c310
source-git-commit: 7ac04991289c95ebb803d1fd804e9b497f821cda
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 1%

---

# 特定の mvpd を使用した認証決定の取得 {#retrieve-authorization-decisions-using-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/decisions/authorize/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">リソース</td>
      <td>再生する前にMVPDの決定が必要なリソースのリスト。</td>
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
         application/json にする必要があります。
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
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token<br/> または <br/>X-Roku-Reserved-Roku-Connect-Token</td>
      <td>
        Platform ID 方式のシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> / <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md">X-Roku-Reserved-Roku-Connect-Token</a> ヘッダードキュメントに記載されています。
        <br/><br/>
        プラットフォーム ID を使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> プラットフォーム ID フローを使用したシングルサインオン </a> ドキュメントを参照してください。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        サービストークンメソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> ヘッダーのドキュメントを参照してください。
        <br/><br/>
        サービストークンを使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> サービストークンフローを使用したシングルサインオン </a> ドキュメントを参照してください。
      </td>
      <td>optional</td>
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
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>ユーザー固有 ID ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a> ヘッダードキュメントを参照してください。</td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        訪問者識別子ペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a> ヘッダードキュメントを参照してください。
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
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに準拠したエラー情報が含まれている場合があります。
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
      <td>200</td>
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
      <td style="background-color: #DEEBFF;">決定</td>
      <td>
         要素のリストを含む JSON。各要素は次の属性を持ちます。
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
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
                  決定ソースに関する情報。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b>mvpd</b><br/>Decision は、MVPD Authorization エンドポイントによって発行されます。</li>
                    <li><b>degradation</b><br/>Decision は、アクセスが低下した結果として発行されます。</li>
                    <li><b>tempass</b><br/>Decision は、一時的なアクセスの結果として発行されます。</li>
                    <li><b>dummy</b><br/>Decision は、ダミー認証機能の結果として発行されます。</li>
                  </ul>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">トークン</td>
               <td>
                  メディアトークンに関する情報。
                  <br/><br/>
                  次の属性を持つ JSON オブジェクト。
                  <ul>
                    <li><b>notBefore</b><br/> メディアトークンが無効になる前のタイムスタンプ（ミリ秒単位）。</li>
                    <li><b>notAfter</b><br/> メディアトークンが無効になるまでのタイムスタンプ（ミリ秒単位）。</li>
                    <li><b>serializedToken</b><br/>Base64 エンコードされたメディアトークン。</li>
                  </ul>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>決定が無効になる前のタイムスタンプ（ミリ秒単位）。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>決定が無効になるまでのタイムスタンプ（ミリ秒単位）。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">エラー</td>
               <td>このエラーは、「拡張エラーコード <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> ドキュメントに従った「拒否」決定に関する追加情報を提供 </a> ます。</td>
               <td>optional</td>
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
      <td>
            応答本文には、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。
            <br/><br/>
            クライアントアプリケーションは、この API で最も一般的に返されるエラーコードを適切に処理できるエラー処理メカニズムを実装する必要があります。
            <ul>
                <li>authenticated_profile_missing</li>
                <li>authenticated_profile_expired</li>
                <li>authorization_denied_by_mvpd</li>
                <li>network_received_error</li>
                <li>等。</li>
            </ul>
            上記のリストは完全ではありません。 クライアントアプリケーションは、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md"> 公開ドキュメント </a> で定義されているすべての拡張エラーコードを処理できる必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### 1.決定が許可されているときに、特定の mvpd を使用して認証決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/decisions/authorize/Cablevision HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": true,
            "token": {
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            },
            "notBefore": 1697094207324,
            "notAfter": 1697098802367
        }
    ]
}
```

>[!ENDTABS]

### 2.決定が拒否の場合に、特定の mvpd を使用して認証決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/decisions/authorize/Cablevision HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": false,
            "error": {
                "action": "none",
                "status": 403,
                "code": "authorization_denied_by_mvpd",
                "message": "The MVPD has returned a "Deny" decision when requesting authorization for the specified resource",
                "details": "Your subscription package does not include the "Live" channel",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
            },
            "notBefore": 1697094207324,
            "notAfter": 1697098802367
        }
    ]
}
```

>[!ENDTABS]

### 3.特定の mvpd を使用して、劣化が適用されている場合に認証決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/REF30/decisions/authorize/${degradedMvpd} HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB  応答 – AuthNAll の低下 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697549918183
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697549918183
        }
    ]
}
```

>[!TAB  応答 – AuthZAll の低下 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
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
            "mvpd": "${degradedMvpd}",
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

>[!TAB  応答 – AuthZNone の低下 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]

### 4.基本的な TempPass を使用した認証決定の取得

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Response – 利用可能 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            },
            "notBefore": 1697094207324,
            "notAfter": 1697594802367
        }
    ]
}
```

>[!TAB  応答 – 期間の制限を超えました ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_duration_limit_exceeded",
                "message": "The temporary access duration limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB  応答 – 無効な設定 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 500,
                "code": "invalid_configuration_temporary_access",
                "message": "The temporary access configuration is invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "configuration"
            }
        }
    ]
}
```

>[!ENDTABS]

### 5.プロモーション TempPass を使用して承認の決定を取得する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Response – 利用可能 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

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
            },
            "notBefore": 1697543318183,
            "notAfter": 1697843918183
        }
    ]
}
```

>[!TAB  応答 – 期間の制限を超えました ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_duration_limit_exceeded",
                "message": "The temporary access duration limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB  応答 – リソースの制限を超えました ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_resources_limit_exceeded",
                "message": "The temporary access resources limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB  応答 – 無効な設定 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 500,
                "code": "invalid_configuration_temporary_access",
                "message": "The temporary access configuration is invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "configuration"
            }
        }
    ]
}
```

>[!TAB  応答 – 無効な ID]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 400,
                "code": "invalid_header_identity_for_temporary_access",
                "message": "The identity for temporary access header value is missing or invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
