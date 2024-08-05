---
title: 認証セッションの再開
description: REST API V2 – 認証セッションの再開
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 1%

---


# 認証セッションの再開 {#resume-authentication-session}

>[!NOTE]
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
      <td>/api/v2/{serviceProvider}/sessions/{code}</td>
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
      <td style="background-color: #DEEBFF;">コード</td>
      <td>ストリーミングデバイスで認証セッションを作成した後に取得した認証コード。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">本文パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。
        <br/><br/>
        ストリーミングデバイスプラットフォームで値の指定に制限がある場合、アプリケーションは認証セッションを再開し、有効な値を指定する必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        MVPD ログインを実行しているアプリケーションの発信元ドメイン。
        <br/><br/>
        ストリーミングデバイスプラットフォームで値の指定に制限がある場合、アプリケーションは認証セッションを再開し、有効な値を指定する必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPD の認証フローが完了したときにユーザーエージェントが移動する最後のリダイレクト URL。
        <br/><br/>
        値は URL エンコードする必要があります。
        <br/><br/>
        ストリーミングデバイスプラットフォームで値の指定に制限がある場合、アプリケーションは認証セッションを再開し、有効な値を指定する必要があります。
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
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         送信するリソースに使用できるメディアタイプ。
         <br/><br/>
         application/x-www-form-urlencoded である必要があります。
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
        応答本文には、認証の実行に必要な次のアクションに関する情報が含まれています。
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
      <th style="background-color: #EFF2F7; width: 15%;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         次の属性を持つ JSON オブジェクト。
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  認証フローを完了するためにストリーミングデバイスが実行する必要があるアクション。
                  <br/><br/>
                  使用可能な値は次のとおりです。
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">値</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">認証</td>
                        <td>ストリーミングデバイスまたは別のデバイスが、指定された URL をユーザーエージェントで開く必要があります。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">再試行</td>
                        <td>ストリーミングデバイスまたは別のデバイスが、不足しているパラメーターを指定し、コードを使用して認証セッションを再開する必要があります。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">承認する</td>
                        <td>ストリーミングデバイスは、決定フローを直接続行できます。</td>
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
                        <td style="background-color: #DEEBFF;">ダイレクト</td>
                        <td>フローは、クライアント実装で使用可能な HTTP クライアントを使用して、指定された URL への直接呼び出しを続行します。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">対話型</td>
                        <td>フローは、ユーザーエージェントを使用して、指定された URL へのナビゲーションを続行します。</td>
                     </tr>
                  </table>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>基本認証フローを完了するために指定する必要がある、不足しているパラメーター。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>クライアントアプリケーションの移動先の URL。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">コード</td>
               <td>認証セッションを再開するためにセカンダリ・アプリケーションで使用できる認証コード。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>ユーザーアクティビティの追跡に使用できる不透明な識別子。</td>
               <td><i>必須</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>オンボーディングプロセス中に ID プロバイダーに関連付けられた内部の一意の ID。</td>
               <td>optional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
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

### 1.必要なすべてのパラメーターに値を指定せずに認証セッションを再開する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```JSON
POST /api/v2/REF30/sessions/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB  応答 ]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "retry",
   "actionType" : "interactive",
   "url" : "/v2/REF30/sessions/8BLW4RW",
   "missingParameters" : ["redirectUrl"]
   "code" : "8BLW4RW",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
