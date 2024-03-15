---
title: スロットルメカニズム
description: Adobe Pass Authentication で使用されるスロットルメカニズムについて説明します。 このページでは、このメカニズムの概要を説明します。
source-git-commit: 4f81f39427d87e4274c27d8f1b4bd1eb366d9abb
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---


# スロットルメカニズム {#throttling-mechanism}

すべてのパス認証のお客様は、手順とビジネスケースに従って、各ユーザーのパス認証 API にアクセスできる必要があります。

パス認証では、お客様のユーザー間でリソースを確実に均等に配布するためのスロットルメカニズムが導入されています。

このメカニズムは、いくつかの理由で重要です。

- マルチテナントアーキテクチャの最も重要なルールの 1 つは、あるユーザーの行動が他のユーザーに影響を与えないことです。
- レート制限は、API との統合時に間違いを犯しやすいので、API にとって重要です。 API はプログラムによって提供されるので、意図したよりも多くのリクエストを誤って送信するのは比較的簡単です。

## メカニズムの概要 {#mechanism-overview}

### クライアントの実装

Pass Authentication は、API とやり取りするためのガイドラインと SDK を提供しますが、お客様による使用方法は制御しません。 実装によっては、基本的な実装があり、誤ってまたは間違っていても、サービスに不要な API リクエストが大量に送信される場合があり、他のユーザーが低速化や容量の問題を経験する可能性があります。

サービス自体が任意の合理的な能力を処理できる必要があります。 しかし、サービスの拡張性やパフォーマンスがどれほど高くても、常に制限があります。 したがって、サービスには、特定の期間内に許可される呼び出しの数に対して設定された制限が必要です。

### スロットルの導入

パス認証は、ユーザーの識別と、事前定義された値を使用したトークンバケットレート制限アルゴリズムに基づいており、アドビの API に対する各ユーザーのデバイスアクセスを制御します。

### デバイス識別メカニズム

提案されたスロットルメカニズムでは、「X-Forwarded-For」ヘッダーを利用して、識別されたデバイスを個別に使用します。 制限は、各デバイスで同じ方法で適用されます。

### 必要な更新

サーバー間実装は、「X-Forwarded-For」ヘッダーメカニズムを使用して、クライアントの IP アドレスを転送する必要があります。

X-Forwarded-For ヘッダーを渡す方法の詳細を確認できます [ここ](rest-api-cookbook-servertoserver.md).

### 実際の制限とエンドポイント

現在、デフォルトの制限では、1 秒あたり最大 1 個のリクエストを指定できます。最初のバーストは 3 件です（識別されたクライアントの最初の操作に対する 1 回限りの許容値で、初期化が正常に完了するようにします）。 これは、すべてのお客様の通常のビジネスケースに影響を与えません。

スロットルメカニズムは、次のエンドポイントで有効になります。

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDK 実装の曖昧さ回避

Adobe Pass Authentication が提供する SDK を使用するクライアントは、どのエンドポイントとも明示的にやり取りしていないので、この節では、既知の関数、スロットル応答が発生した際の動作、実行する必要があるアクションについて説明します。

#### setRequestor

次を使用してスロットル制限に達したとき： `setRequestor` 関数を呼び出すと、SDK は、 `errorHandler` コールバック。

#### getAuthorization

次を使用してスロットル制限に達したとき： `getAuthorization` 関数を呼び出すと、SDK は、 `errorHandler` コールバック。

#### checkPreauthorizedResources

次を使用してスロットル制限に達したとき： `checkPreauthorizedResources` 関数を呼び出すと、SDK は `errorHandler` コールバック。

#### getMetadata

次を使用してスロットル制限に達したとき： `getMetadata` 関数を呼び出すと、SDK は、 `setMetadataStatus` コールバック。

個々の実装の詳細については、特定の SDK ドキュメントを参照してください。

- [JavaScript SDK API リファレンス](javascript-sdk-api-reference.md)
- [Android SDK API リファレンス](android-sdk-api-reference.md)
- [iOS/tvOS API リファレンス](iostvos-sdk-api-reference.md)

### API 応答の変更と応答

制限が満たされていることを特定した場合、このリクエストを特定の応答ステータス (HTTP 429 Too Many Requests) でマークし、その間にユーザーデバイス（IP アドレス）に割り当てられたすべてのトークンを消費したことを示します。

スロットルは、最初の 429 応答の 1 秒後に期限切れになります。 429 応答を受け取る各アプリケーションは、少なくとも 1 秒待ってから新しいリクエストを生成する必要があります。

すべてのお客様のアプリケーションは、「429 Too Many Requests」の応答を適切に処理する必要があります。

以下に、429 応答メッセージの例を示します。

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## 影響と必要な変更

### X-Forwarded-For ヘッダーを渡す

Pass Authentication API とのやり取りにカスタム実装（サーバー間認証を含む）を使用しているお客様は、X-Forwarded-For ヘッダーを使用して、Pass Authentication API にユーザー IP アドレスを取得し、正しく転送できることを確認する必要があります。

詳しくは、 [ここ](rest-api-cookbook-servertoserver.md) を参照してください。

### 新しい応答コードへの対応

Pass Authentication API とのやり取りにカスタム実装（サーバー間認証を含む）を使用しているお客様は、429 Too Many リクエストの受信後におこなわれる以降の呼び出しには、最低 1 秒の待ち時間が含まれている必要があります。 この待機期間は、このメカニズムを変更し、有効なビジネス応答を取得する機会を確保します。

## スロットルのシナリオの例

| 最初のリクエストからの時間 | 受信した応答 | 説明 |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| 秒 0 | 呼び出しが成功ステータスコードを受け取る | 1 件の呼び出しが制限から消費されました |
| Second 0.3 | 呼び出しが成功ステータスコードを受け取る | 制限から消費された 1 件の呼び出しと、バーストとしてマークされた 1 件の呼び出し |
| Second 0.6 | 呼び出しが成功ステータスコードを受け取る | 制限から消費された 1 件の呼び出しと、バーストとしてマークされた 2 件の呼び出し |
| Second 0.9 | 呼び出しが成功ステータスコードを受け取る | 制限から消費された 1 件の呼び出しと、バーストとしてマークされた 3 件の呼び出し |
| 2 番目の 1.2 | 呼び出しが成功ステータスコードを受け取る | 制限から消費された 2 回の呼び出しと、バーストとしてマークされた 3 回の呼び出し |
| 2 番目の 1.4 | 呼び出しは 429 ステータスコードを受け取ります | 制限から消費された 2 回の呼び出しと、バーストとしてマークされた 3 回の呼び出し、1 回の呼び出しで「429 Too many requests」を受け取る |
| 2 番目の 1.6 | 呼び出しは 429 ステータスコードを受け取ります | 制限から消費された 2 回の呼び出しと、バーストとしてマークされた 3 回の呼び出し、2 回の呼び出しで「429 Too many requests」を受け取る |
| 2 番目の 1.8 | 呼び出しは 429 ステータスコードを受け取ります | 制限から消費された 2 回の呼び出しと、バーストとしてマークされた 3 回の呼び出し、3 回の呼び出しで「429 Too many requests」を受け取る |
| 2 番目の 2.1 | 呼び出しが成功ステータスコードを受け取る | 制限から消費された 3 つの呼び出しと、バーストとしてマークされた 3 つの呼び出し、および 3 つの呼び出しが「429 Too many requests」を受け取る |
