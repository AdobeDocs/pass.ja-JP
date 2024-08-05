---
title: 特定のコードのプロファイルの取得
description: REST API V2 – 特定のコードのプロファイルを取得
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 1%

---


# 特定のコードのプロファイルの取得 {#retrieve-profile-for-specific-code}

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
      <td>/api/v2/{serviceProvider}/profiles/{code}</td>
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
      <td style="background-color: #DEEBFF;">コード</td>
      <td>ストリーミングデバイスで認証セッションを作成した後に取得した認証コード。</td>
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
        応答本文には、有効なプロファイルのマップが含まれています。このマップは空の場合があります。
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
      <td style="background-color: #DEEBFF;">プロファイル</td>
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
            </tr>
         </table>
         value 要素は、次の属性で定義されます。
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
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
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">値</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd<br/><br/> 例：Spectrum、Cablevision など</td>
                        <td>
                            プロファイルは次の結果として作成されました。
                            <ul>
                                <li>基本認証</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">Adobe</td>
                        <td>
                            プロファイルは次の結果として作成されました。
                            <ul>
                                <li>アクセスが低下しました</li>
                                <li>一時アクセス</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">タイプ</td>
               <td>
                  プロファイルのタイプ。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">値</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">標準</td>
                        <td>
                            プロファイルは次の結果として作成されました。
                            <ul>
                                <li>基本認証</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">機能低下</td>
                        <td>
                            プロファイルは次の結果として作成されました。
                            <ul>
                                <li>アクセスが低下しました</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">一時的</td>
                        <td>
                            プロファイルは次の結果として作成されました。
                            <ul>
                                <li>一時アクセス</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">属性</td>
               <td>
                    ユーザーメタデータ属性のリスト。
                    <br/><br/>
                    次の属性があります。
                    <ul>
                        <li>必須（「userId」など）</li>
                        <li>非必須（「zip」、「householdId」、「maxRating」など）。</li>
                    </ul>
                    属性の値には次の種類があります。
                    <ul>
                        <li>シンプル</li>
                        <li>list</li>
                        <li>マップ</li>
                    </ul>
               </td>
               <td><i>必須</i></td>
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

### 1.基本認証の実行後、セカンダリデバイス上の既存の有効な認証済みプロファイルを取得します

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
GET /api/v2/REF30/profiles/Cablevision/XTC98W
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Cablevision",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]
