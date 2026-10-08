---
title: スロットル機構
description: Adobe Pass Authenticationで使用されるスロットリングメカニズムについて説明します。 このページでは、このメカニズムの概要を説明します。
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1162'
ht-degree: 0%
---
# スロットル機構 {#throttling-mechanism}

すべてのPass認証のお客様は、手順とビジネスケースに従って、各ユーザーのPass認証APIにアクセスできる必要があります。

Pass Authenticationでは、お客様のユーザー間でのリソースの公平な配布を保証するためのスロットリングメカニズムが導入されています。

このメカニズムは、次のような理由で重要です。

- マルチテナントアーキテクチャの黄金律のひとつは、あるユーザーの行動が他のユーザーに影響を与えてはならないということです。
- レート制限は、APIと統合する際にミスが発生しやすいため、APIにとって重要です。 APIはプログラム的に動作するため、意図したよりも多くのリクエストを誤って送信することは比較的簡単です。

## メカニズムの概要 {#mechanism-overview}

### クライアント実装

Pass Authenticationは、APIを操作するためのガイドラインとSDKを提供しますが、お客様による使用方法は制御しません。 一部の実装には初歩的な実装が含まれている場合があり、誤ってAPI リクエストが発生したり、他のユーザーに遅延や容量の問題が発生したりする可能性があります。

サービス自体は、合理的な容量を処理できる必要があります。 しかし、サービスの拡張性やパフォーマンスにかかわらず、常に制限があります。 そのため、サービスには、特定の時間間隔で受け入れられる呼び出し数に対して設定された制限が必要です。

### スロットルの導入

パス認証は、ユーザー識別と、事前に定義された値を持つトークンバケット率制限アルゴリズムに基づいて行われ、各ユーザーのAPIへのアクセスを制御します。

### デバイス識別機構

提案されたスロットリングメカニズムは、「X-Forwarded-For」ヘッダーの助けを借りて、特定されたデバイスを個別に使用します。 制限は、各デバイスに対して同じように適用されます。

### 必要な更新

サーバー間の実装では、「X-Forwarded-For」ヘッダーメカニズムを使用して、クライアントのIP アドレスを転送する必要があります。

X-Forwarded-For ヘッダー[を渡す方法について詳しくは、こちらを参照してください](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)。

### 実際の限界値とエンドポイント {#throttling-mechanism-limits}

現在、デフォルトの制限では、1秒あたり最大1 リクエストを許可し、最初のバーストは10 リクエストです（特定されたクライアントの最初のインタラクションに対する1回限りの許可により、初期化が正常に終了する必要があります）。 これは、すべての顧客で通常のビジネスケースに影響を与えることはありません。

スロットル メカニズムは、次のエンドポイントで有効になります。

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
- /adobe-services/config/
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### SDKの実装の曖昧さ

Adobe Pass Authenticationを使用するクライアントは、SDKが提供するエンドポイントと明示的に相互作用しないため、この節では、既知の関数、スロットル応答が発生した場合の動作、実行すべきアクションについて説明します。

#### setRequestor

SDKの`setRequestor`関数を使用してスロットル制限に達すると、SDKは`errorHandler` コールバックを通じてCFG429 エラーコードを返します。

#### getAuthorization

SDKの`getAuthorization`関数を使用してスロットル制限に達すると、SDKは`errorHandler` コールバックを通じてZ100 エラーコードを返します。

#### checkPreauthorizedResources

SDKの`checkPreauthorizedResources`関数を使用してスロットル制限に達すると、SDKは`errorHandler` コールバックを通じてP100 エラーコードを返します。

#### getMetadata

SDKの`getMetadata`関数を使用してスロットル制限に達すると、SDKは`setMetadataStatus` コールバックを通じて空の応答を返します。

それぞれの実装の詳細については、SDKの具体的なドキュメントを参照してください。

- [JavaScript SDK API リファレンス](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Android SDK API リファレンス](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [iOS/tvOS API リファレンス](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### API応答の変更と応答 {#throttling-mechanism-response}

制限に違反していることを特定すると、このリクエストに特定の応答ステータス（HTTP 429 Too Many Requests）を付けて、時間間隔でユーザーデバイス（IP アドレス）に割り当てられたすべてのトークンを消費したことを示します。

スロットリングは、最初の429回の応答の1秒後に期限切れになります。 429応答を受信する各アプリケーションは、新しいリクエストを生成する前に、少なくとも1秒間待つ必要があります。

すべての顧客アプリケーションは、「429 Too Many Requests」応答を適切に処理する必要があります。

429応答メッセージの例を次に示します。

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

### X-Forwarded-For ヘッダーを渡しています

カスタム実装（サーバー間の実装を含む）を使用してPass Authentication APIを操作する場合は、X-Forwarded-For ヘッダーを使用してPass Authentication APIにさらに進め、ユーザーのIP アドレスを取得し、正しく転送できるようにする必要があります。

詳しくは、[こちら](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)を参照してください。

### 新しい応答コードへの対応

カスタム実装（サーバー間の実装を含む）を使用してPass Authentication APIを操作する場合は、429 Too Many Requestを受信した後に行われた後続の呼び出しに、最低1秒間の待機期間が含まれていることを確認する必要があります。 この待機期間は、このメカニズムを変更し、有効なビジネス応答を取得する機会を保証します。

## スロットルのシナリオ例

| 最初のリクエストからの時間 | 応答を受信しました | 説明 |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| 2番目0 | 呼び出しは成功ステータスコードを受信します | 制限から1回の呼び出しが消費されました |
| 2番目0.3 | 呼び出しは成功ステータスコードを受信します | 制限から1回の通話が消費され、バーストとしてマークされた1回の通話が消費されました |
| 2番目0.6 | 呼び出しは成功ステータスコードを受信します | 制限から1回の通話が消費され、バーストとしてマークされた2回の通話が消費されました |
| 2番目0.9 | 呼び出しは成功ステータスコードを受信します | 制限から1回の通話が消費され、バーストとしてマークされた3回の通話が消費されました |
| 2番目の1.2 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2回の呼び出しと、バーストとしてマークされた3回の呼び出し |
| 2番目1.3 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2件の通話と、バーストとしてマークされた4件の通話 |
| 2番目1.4 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2件の通話と、バーストとしてマークされた5件の通話 |
| 2番目の1.5 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2件の通話と、バーストとしてマークされた6件の通話 |
| 2番目の1.6 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2件の通話と、バーストとしてマークされた7件の通話 |
| 2番目の1.7 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2回の呼び出しと、バーストとしてマークされた8回の呼び出し |
| 2番目の1.8 | 呼び出しは成功ステータスコードを受信します | 制限から消費された2件の通話と、バーストとしてマークされた9件の通話 |
| 2番目2.1 | 呼び出しは成功ステータスコードを受信します | 制限から3回の通話が消費され、バーストとしてマークされた9回の通話が消費されました |
| 2番目2.2 | 呼び出しは成功ステータスコードを受信します | 制限から3回の通話が消費され、バーストとしてマークされた10回の通話が消費されました |
| 2番目2.4 | 呼び出しは429 ステータスコードを受信します | 制限から3回の呼び出しが消費され、バーストとしてマークされた10回の呼び出しと1回の呼び出しは「429 Too many requests」を受信します |
| 2番目2.6 | 呼び出しは429 ステータスコードを受信します | 制限から3回の呼び出しが消費され、バーストとしてマークされた10回の呼び出しと2回の呼び出しは「429 Too many requests」を受信します |
| 2番目2.8 | 呼び出しは429 ステータスコードを受信します | 制限から3回の呼び出しが消費され、バーストとしてマークされた10回の呼び出しと3回の呼び出しは「429 Too many requests」を受信します |
| 2番目3.1 | 呼び出しは成功ステータスコードを受信します | 制限から4回の呼び出しが消費され、バーストとしてマークされた10回の呼び出しと3回の呼び出しは「429 Too many requests」を受信します |
