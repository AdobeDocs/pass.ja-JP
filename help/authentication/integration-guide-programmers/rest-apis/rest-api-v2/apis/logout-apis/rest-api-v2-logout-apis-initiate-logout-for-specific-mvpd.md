---
title: 特定のmvpdのログアウトを開始
description: REST API V2 – 特定のmvpdのログアウトを開始する
exl-id: 2482de87-b3d4-4ea8-bd4a-25bf10017e01
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 2%
---
# 特定のmvpdのログアウトを開始 {#initiate-logout-for-specific-mvpd}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> REST API V2の実装は、[&#x200B; スロットル メカニズム &#x200B;](/help/authentication/integration-guide-programmers/throttling-mechanism.md)のドキュメントによって制限されています。

## リクエスト {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/v2/{serviceProvider}/logout/{mvpd}</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>オンボーディングプロセス中にID プロバイダーに関連付けられた内部一意のID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">クエリのパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPDのログアウトフローが完了したときにユーザーエージェントが移動する最後のリダイレクト URL。
        <br/><br/>
        値はURL エンコードする必要があります。
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
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token<br/>または<br/>X-Roku-Reserved-Roku-Connect-Token</td>
      <td>
        Platform ID メソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a> / <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md">X-Roku-Reserved-Roku-Connect-Token</a> ヘッダードキュメントを参照してください。
        <br/><br/>
        プラットフォーム IDを使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> プラットフォーム ID フローを使用したシングルサインオン </a>のドキュメントを参照してください。
      </td>
      <td>オプション</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        サービストークン メソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> ヘッダーのドキュメントを参照してください。
        <br/><br/>
        サービストークンを使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> サービストークンのフローを使用したシングルサインオン </a>のドキュメントを参照してください。
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
        応答本文には、削除された各プロファイルのログアウトフローを完了するために、クライアントが実行する必要があるアクションのリストが含まれます。
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
      <td style="background-color: #DEEBFF;">ログアウト</td>
      <td>
         キー、値のペアのマップを含むJSON。
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
               <td>オンボーディングプロセス中にID プロバイダーに関連付けられた内部一意のID。</td>
               <td><i>必須</i></td>
         </table>
         value エレメントは、次の属性で定義されます。
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  ログアウトフローを完了するためにストリーミングデバイスが実行する必要があるアクション。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <ul>
                    <li><b> ログアウト </b><br/> ストリーミングデバイスは、ユーザーエージェントで指定されたURLを開く必要があります。<br/>このアクションは、次のシナリオに適用されます。ログアウトエンドポイントを使用してMVPDからログアウトします。</li>
                    <li><b>partner_logout</b><br/> ストリーミングデバイスは、パートナー（システム）レベルからもログアウトするようにユーザーに通知する必要があります。<br/>このアクションは、次のシナリオに適用されます。プロファイルの種類が「appleSSO」の場合、MVPDからログアウトします。</li>
                    <li><b>complete</b><br/> ストリーミングデバイスは、その後のアクションを実行する必要はありません。<br/>このアクションは、ログアウトエンドポイント（ダミーログアウト機能）を使用せずにMVPDからログアウトする、低下したアクセス中にログアウトする、一時的なアクセス中にログアウトする、などのシナリオに適用されます。</li>
                    <li><b>無効</b><br/> ストリーミングデバイスは、後続のアクションを実行する必要はありません。<br/>このアクションは、次のシナリオに適用されます。有効なプロファイルが見つからないときにMVPDからログアウトします。</li>
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
                    <li><b>interactive</b><br/>このタイプは、'actionName'属性<b> ログアウト </b>の次の値に適用されます。</li>
                    <li><b>partner_interactive</b><br/>このタイプは、「actionName」属性<b>partner_logout</b>の次の値に適用されます。</li>
                    <li><b>none</b><br/>このタイプは、「actionName」属性の次の値に適用されます：<b>complete</b>、<b>invalid</b>。</li>
                  </ul>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>オンボーディングプロセス中にID プロバイダーに関連付けられた内部一意のID。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  MVPD エンドポイントでログアウトフローを実行するために使用されるURL。
                  <br/><br/>
                  これは、「actionName」属性の次の値には存在しません。
                  <ul>
                    <li><b>完了</b></li>
                    <li><b>無効</b></li>
                  </ul>
               </td>
               <td>オプション</td>
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
      <td>応答本文は、<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">拡張エラーコード </a> ドキュメントに準拠する追加のエラー情報を提供する場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples}

### &#x200B;1. ログアウトエンドポイントを持つ特定のmvpdのログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/REF30/logout/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)  
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "Cablevision": {
            "actionName": "logout",
            "actionType": "interactive",
            "mvpd": "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### &#x200B;2. ログアウトエンドポイントを使用せずに、特定のmvpdのログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/REF30/logout/Dish?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "Dish": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "Dish"
       }
    }
}
```

>[!ENDTABS]

### &#x200B;3. サービストークン方式を使用してシングルサインオンで取得したプロファイルを含む、特定のmvpdに対してシングルログアウトを開始します

>[!IMPORTANT]
>
> 前提条件
>
> <br/>
>
> * MVPDにはログアウトエンドポイントがあります。

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/REF30/logout/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Spectrum": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Spectrum",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Spectrum&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### &#x200B;4. Platform ID メソッドを使用してシングルサインオンで取得したプロファイルを含む、特定のmvpdに対してシングルログアウトを開始します

>[!IMPORTANT]
>
> 前提条件
>
> <br/>
>
> * MVPDにはログアウトエンドポイントがあります。

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/REF30/logout/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Comcast_SSO": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Comcast_SSO",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Comcast_SSO&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### &#x200B;5. パートナー（Apple）を使用してシングルサインオンで取得したプロファイルを含む、特定のmvpdのログアウトを開始します

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/REF30/logout/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "logouts": {
      "Cablevision": {
         "actionName": "partner_logout",
         "actionType": "partner_interactive",
         "mvpd": "Cablevision"
      }
   }
}
```

>[!ENDTABS]

### &#x200B;6. 劣化が適用されている間に、特定のmvpdのログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/REF30/logout/${degradedMvpd}?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "ATTOTT": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "${degradedMvpd}",
        }
    }
}
```

>[!ENDTABS]

### &#x200B;7. 基本またはプロモーション用のTempPassのログアウトを開始する（必須ではありません）

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "logouts": {
        "TempPass_5mins": {
            "actionName": "complete",
            "actionType": "none",
            "mvpd": "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]
