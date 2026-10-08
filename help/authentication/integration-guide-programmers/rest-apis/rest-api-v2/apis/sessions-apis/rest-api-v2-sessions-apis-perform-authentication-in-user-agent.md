---
title: ユーザーエージェントで認証を実行する
description: REST API V2 - ユーザーエージェントで認証を実行する
exl-id: d615dde0-71a8-4b6c-a12e-1e3b5e20728c
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 5%
---
# ユーザーエージェントで認証を実行する {#perform-authentication-in-user-agent}

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
      <td>302</td>
      <td>見つかりました</td>
      <td>
        レスポンス本文には、MVPD ログインページに到達するまでフローを続行する場所リダイレクトが含まれています
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>不正なリクエスト</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して、もう一度試す必要があります。
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
        サーバーサイドで問題が発生しました。
      </td>
   </tr>
</table>

### 成功 {#success}

正常な応答は、MVPD ログインページに到達するまでの一連の1つまたは複数のリダイレクトです。

### エラー {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400, 405, 500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">コンテンツタイプ</td>
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

### &#x200B;1. ユーザーエージェントで認証を実行する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/v2/authenticate/REF30/8KHP9RW HTTP/1.1

    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 応答]

```HTTPS
HTTP/1.1 302 Found

Location :  https://sp.auth.adobe.com/adobe-services/authenticate/saml?noflash=true&mso_id=Cablevision&requestor_id=REF30&no_iframe=false&domain_name=adobe.com&redirect_url=http%3A%2F%2Fadobe.com%2Fapitest%2Flive.html
```

>[!ENDTABS]
