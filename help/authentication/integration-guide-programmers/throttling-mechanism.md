---
title: スロットルメカニズム
description: Adobe Pass認証で使用されるスロットルメカニズムについて説明します。 このページでは、このメカニズムの概要について説明します。
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1162'
ht-degree: 0%

---

# スロットルメカニズム {#throttling-mechanism}

すべてのパス認証のお客様は、手順とビジネスケースに従って、各ユーザーのパス認証 API にアクセスできる必要があります。

パス認証では、お客様のユーザー間でリソースを公平に分配するためのスロットルメカニズムが導入されています。

このメカニズムが重要な理由は次のとおりです。

- マルチテナントアーキテクチャの優れたルールの 1 つは、あるユーザーの行動が他のユーザーに影響を与えないということです。
- レート制限は、API と統合する際に間違いが生じやすいため、API にとって重要です。 API はプログラムで使用されるので、意図したよりも多くのリクエストを誤って送信することは比較的簡単です。

## メカニズムの概要 {#mechanism-overview}

### クライアントの実装

パス認証は、API を操作するためのガイドラインとSDKを提供しますが、お客様による API の使用方法は制御しません。 一部の実装には基本的な実装が含まれている場合があり、誤ってもしなくても、サービスに不要な API リクエストが殺到する可能性があります。これは、他のユーザーに速度の低下や容量の問題が発生することを意味する場合があります。

サービス自体は、合理的な容量を処理できる必要があります。 ただし、サービスの拡張性やパフォーマンスに関係なく、常に制限があります。 そのため、サービスには、特定の期間内で許可される呼び出しの数に対する制限が設定されている必要があります。

### スロットルの概要

パス認証は、ユーザー識別と、API に対する各ユーザーのデバイスアクセスを制御するための事前定義済みの値を持つトークンバケットレート制限アルゴリズムに基づいています。

### デバイスの識別メカニズム

提案されているスロットルメカニズムは、「X-Forwarded-For」ヘッダーを使用して、識別されたデバイスを個別に使用します。 制限は、各デバイスに対して同じ方法で適用されます。

### 必要な更新

サーバー間の実装では、「X-Forwarded-For」ヘッダーメカニズムを使用してクライアントの IP アドレスを転送する必要があります。

X-Forwarded-For ヘッダーの渡し方の詳細については、[ こちら ](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md) をご覧ください。

### 実際の上限とエンドポイント {#throttling-mechanism-limits}

現在、デフォルトの制限では、1 秒あたり最大 1 件のリクエストが許可され、初期バーストは 10 件のリクエストが許可されます（識別されたクライアントの最初のインタラクションで 1 回の許可が必要です。これにより、初期化が正常に終了します）。 これは、すべての顧客の通常のビジネスケースには影響しません。

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
- /api/v1/identity
- /adobe-services/config/
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDKの実装の曖昧さ

Adobe Pass認証が提供する SDK を使用するクライアントは明示的にはエンドポイントとやり取りしていないため、この節では、既知の関数、スロットル応答が発生した場合の動作、実行する必要があるアクションについて説明します。

#### setRequestor

SDKの関数を使用してスロットル制限 `setRequestor` 到達すると、SDKはコールバックを通じて CFG429 エラーコード `errorHandler` 返します。

#### getAuthorization

SDKの関数を使用してスロットル制限 `getAuthorization` 到達すると、SDKはコールバックを通じて Z100 エラーコード `errorHandler` 返します。

#### checkPreauthorizedResources

SDKの関数を使用してスロットル制限 `checkPreauthorizedResources` 到達すると、SDKはコールバックを通じて P100 エラーコード `errorHandler` 返します。

#### getMetadata

SDKの関数を使用してスロットル制限 `getMetadata` 到達すると、SDKはコールバックを通じて空の応答 `setMetadataStatus` 返します。

実装の詳細については、それぞれのSDK ドキュメントを参照してください。

- [JavaScript SDK API リファレンス](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Android SDK API リファレンス](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [iOS/tvOS API リファレンス](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### API 応答の変更と応答 {#throttling-mechanism-response}

制限に違反していると判断した場合、このリクエストを特定の応答ステータス（HTTP 429 Too Many Requests）でマークし、ユーザーデバイスに割り当てられているすべてのトークン（IP アドレス）を時間間隔で消費するように指示します。

スロットルは、最初の 429 応答のうち 1 秒後に有効期限が切れます。 429 応答を受信する各アプリケーションは、新しいリクエストを生成するまで、少なくとも 1 秒待つ必要があります。

すべての顧客アプリケーションは、「429 Too Many Requests」応答を適切に処理する必要があります。

次に、429 応答メッセージのサンプルを示します。

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

カスタム実装（サーバー間を含む）を使用して Pass Authentication API とやり取りするお客様は、ユーザー IP アドレスを確実に取得し、X-Forwarded-For ヘッダーを使用して正しく転送する必要があります。

詳しくは [ こちら ](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md) を参照してください。

### 新しい応答コードへの対応

カスタム実装（サーバー間リクエストを含む）を使用してパス認証 API とやり取りするお客様は、429 リクエストが多すぎる後に行われる後続の呼び出しに 1 秒以上の待機期間を含める必要があります。 この待機期間により、このメカニズムを変更し、有効なビジネスレスポンスを取得する機会が確保されます。

## スロットルのシナリオの例

| 最初のリクエストからの経過時間 | 返信受信済み | 説明 |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| 2 番目 0 | 呼び出しが成功ステータスコードを受信します | 制限から消費された呼び出しは 1 回です |
| 秒 0.3 | 呼び出しが成功ステータスコードを受信します | 1 件の呼び出しが制限から消費され、1 件の呼び出しがバーストとしてマークされました |
| 2 番目 0.6 | 呼び出しが成功ステータスコードを受信します | 1 件の呼び出しが制限から消費され、2 件の呼び出しがバーストとしてマークされました |
| 秒 0.9 | 呼び出しが成功ステータスコードを受信します | 1 件の呼び出しが制限から消費され、3 件の呼び出しがバーストとしてマークされました |
| Second 1.2 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 3 回 |
| Second 1.3 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 4 回 |
| Second 1.4 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 5 回 |
| Second 1.5 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 6 回 |
| Second 1.6 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 7 回 |
| Second 1.7 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 8 回 |
| Second 1.8 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 2 回、バーストとしてマークされた呼び出しが 9 回 |
| Second 2.1 | 呼び出しが成功ステータスコードを受信します | 制限から使用された呼び出しが 3 回、バーストとしてマークされた呼び出しが 9 回 |
| Second 2.2 | 呼び出しが成功ステータスコードを受信します | 制限から消費された呼び出しが 3 回、バーストとしてマークされた呼び出しが 10 回 |
| Second 2.4 | コールが 429 ステータス コードを受け取る | 制限から消費された呼び出しが 3 件、バーストとしてマークされた呼び出しが 10 件、「429 Too many request」が 1 件の呼び出しを受け取ります |
| Second 2.6 | コールが 429 ステータス コードを受け取る | 制限から消費された呼び出しが 3 件、バーストとしてマークされた呼び出しが 10 件、「429 Too many request」が 2 件の呼び出しを受け取ります |
| Second 2.8 | コールが 429 ステータス コードを受け取る | 制限から消費された呼び出しが 3 件、バーストとしてマークされた呼び出しが 10 件、「429 Too many requests」が 3 件の呼び出しを受け取ります |
| Second 3.1 | 呼び出しが成功ステータスコードを受信します | 制限から消費された呼び出しが 4 件、バーストとしてマークされた呼び出しが 10 件、「429 Too many request」が 3 件の呼び出しを受け取ります |
