---
title: ユーザーエージェントでの認証の実行
description: REST API V2 - ユーザーエージェントで認証を実行します
exl-id: d615dde0-71a8-4b6c-a12e-1e3b5e20728c
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 3%

---

# ユーザーエージェントでの認証の実行 {#perform-authentication-in-user-agent}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

>[!MORELIKETHIS]
>
> また、[REST API V2 の FAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general) も必ず参照してください。

## リクエスト {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/v2/authenticate/{serviceProvider}/{code}</td>
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
      <td>オンボーディングプロセス中にサービスプロバイダーに関連付けられた内部の一意の ID。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">コード</td>
      <td>ストリーミングデバイスで認証セッションを作成した後に取得した認証コード。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td>302</td>
      <td>件見つかりました</td>
      <td>
        応答本文には、MVPDのログインページに到達するまでフローを続行するための場所リダイレクトが含まれています
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。
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
        サーバー側で問題が発生しました。
      </td>
   </tr>
</table>

### 成功 {#success}

正常な応答は、MVPDのログインページに到達するまでの一連の 1 つまたは複数のリダイレクトです。

### エラー {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、405、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>text/html</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">-</td>
      <td>-</td>
      <td>-</td>
   </tr>
</table>

## サンプル {#samples}

### 1. ユーザーエージェントで認証を実行する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/authenticate/REF30/8KHP9RW HTTP/1.1

    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 302 Found

Location :  https://sp.auth.adobe.com/adobe-services/authenticate/saml?noflash=true&mso_id=Cablevision&requestor_id=REF30&no_iframe=false&domain_name=adobe.com&redirect_url=http%3A%2F%2Fadobe.com%2Fapitest%2Flive.html
```

>[!ENDTABS]
