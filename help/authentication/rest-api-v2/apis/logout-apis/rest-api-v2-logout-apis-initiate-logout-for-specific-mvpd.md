---
title: 特定の mvpd に対するログアウトの開始
description: REST API V2 – 特定の mvpd のログアウトの開始
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 0%

---


# 特定の mvpd に対するログアウトの開始 {#initiate-logout-for-specific-mvpd}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) のドキュメントで制限されています。

## リクエスト {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">クエリパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPD のログアウトフローが完了したときにユーザーエージェントが移動する最後のリダイレクト URL。
        <br/><br/>
        値は URL エンコードする必要があります。
      </td>
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
        プラットフォーム ID を使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md"> プラットフォーム ID フローを使用したシングルサインオン </a> ドキュメントを参照してください。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        サービストークンメソッドのシングルサインオンペイロードの生成については、<a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a> ドキュメントに記載されています。
        <br/><br/>
        サービストークンを使用したシングルサインオン対応フローについて詳しくは、<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md"> サービストークンフローを使用したシングルサインオン </a> ドキュメントを参照してください。
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
        応答本文には、削除された各プロファイルのログアウトフローを完了するためにクライアントが実行する必要があるアクションのリストが含まれています。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="../../../enhanced-error-codes.md"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
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
      <td style="background-color: #DEEBFF;">ログアウト</td>
      <td>
         キーと値のペアのマップを含む JSON。
         <br/><br/>
         キー要素は、次の値で定義されます。
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">値</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
               <td><i>必須</i></td>
         </table>
         value 要素は、次の属性で定義されます。
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  ログアウトフローを完了するためにストリーミングデバイスで実行する必要があるアクション。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">値</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">ログアウト</td>
                        <td>
                            ストリーミングデバイスは、指定された URL をユーザーエージェントで開く必要があります。
                            <br/><br/>
                            このアクションは、次のシナリオに適用されます。
                            <ul>
                                <li>ログアウトエンドポイントを使用して MVPD からログアウトします。</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">完了</td>
                        <td>
                            ストリーミングデバイスは後続のアクションを実行する必要はありません。
                            <br/><br/>
                            このアクションは、次のシナリオに適用されます。
                            <ul>\
                                <li>ログアウトエンドポイントを使用せずに MVPD からログアウトする（ダミーログアウト機能）</li>
                                <li>縮退アクセス中にログアウトする</li>
                                <li>一時的アクセス中にログアウト</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">無効</td>
                        <td>
                            ストリーミングデバイスは後続のアクションを実行する必要はありません。
                            <br/><br/>
                            このアクションは、次のシナリオに適用されます。
                            <ul>
                                <li>有効なプロファイルが見つからないときに MVPD からログアウトする</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  「actionName」属性で指定されたアクションでフローを続行するために、ストリーミングデバイスが実行する必要があるインタラクションのタイプ。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">値</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">対話型</td>
                        <td>
                            このタイプは、「actionName」属性の次の値に適用されます。
                            <ul>
                                <li>ログアウト</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">なし</td>
                        <td>
                            このタイプは、「actionName」属性の次の値に適用されます。
                            <ul>
                                <li>完了</li>
                                <li>無効</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  MVPD エンドポイントでログアウト フローを実行するために使用される URL。
                  <br/><br/>
                  これは、「actionName」属性の次の値には存在しません。
                  <ul>
                    <li>完了</li>
                    <li>無効</li>
                  </ul>
               </td>
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

### 1. ログアウトフローをサポートする mvpd の基本ログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/logout/REF30/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)  
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Cablevision" : {
            "actionName" : "logout",
            "actionType" : "interactive",
            "mvpd" : "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### 2. ログアウトフローをサポートしない mvpd の基本ログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/logout/REF30/Dish?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Dish" : {
            "actionName" : "complete",
            "actionType" : "none",
            "mvpd" : "Dish"
       }
    }
}
```

>[!ENDTABS]

### 3. Platform ID 方法を使用したシングルサインオンで取得した認証済みプロファイルを含むログアウトを開始します

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/logout/REF30/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
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

### 4. サービストークン方式を使用したシングルサインオンにより取得した、認証済みプロファイルを含むログアウトを開始します

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/logout/REF30/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
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

### 5. （プロモーション）一時パスのログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "TempPass_5mins" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]

### 6.縮退した mvpd のログアウトを開始する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/logout/REF30/ATTOTT?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "ATTOTT" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "ATTOTT",
        }
    }
}
```

>[!ENDTABS]
