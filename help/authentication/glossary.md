---
title: 用語集
description: 用語集
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 用語集 {#glossary}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## AccessEnabler {#accessEnabler}

Adobe Pass Authentication のクライアントコンポーネント。 Adobe Pass認証では、サポートされる各プラットフォームに対して AccessEnabler ライブラリが提供されます。

## AuthN {#authn}

「AuthN トークン」や「AuthN フロー」など、「認証」の略記法として使用されます。


## AuthN トークン{#authn-token}

認証トークン。ユーザーが MVPD で正常に認証された後にAdobe Pass Authentication によって生成されます。 トークンは、プログラマーの統合プラットフォームに応じて、ユーザーのデバイスまたはAdobe Pass認証サーバーに保存されます。

## AuthZ {#authz}

「AuthZ トークン」や「AuthZ フロー」と同様に、「認証」の略記法として使用されます。

## AuthZ トークン {#authz-token}

認証トークン。保護されたコンテンツの表示がユーザーによって承認された後にAdobe Pass Authentication によって生成されます。 AuthZ トークンはAdobe Pass認証サーバーに保存され、 [短時間のみ有効なメディアトークン](#short-lived-token).

## チャネル ID （非推奨） {#channel_id}

以前のリソース ID の用語。

## クライアントレス API {#clientless-api}

AccessEnabler クライアントコンポーネントの代わりに Web サービスを使用するAdobe Pass認証統合ソリューション。

## デバイス ID {#device-id}

デバイス（電話、タブレットなど）を一意に識別する ( Adobe Pass Authentication 内 ) この ID は、プログラマーのアプリケーションによって取得/提供されます。


## 権利付与フロー{#entitlement_flow}

Adobe Pass認証に関するドキュメントで使用される用語は、Adobe Pass認証でのデバイス/ユーザーの登録、MVPD でのユーザーの認証、ユーザーのリソースの認証、ユーザーのログアウトのプロセス全体を指します。


## GUID {#guid}

詳しくは、 [ユーザー ID](#user-id).

## IdP {#idp}

Adobe Pass Authentication 統合での MVPD の役割のコンテキストで、プロバイダーを識別します。MVPD と同義です。 （お客様は、有料テレビプロバイダーのログインページを使用して、自分の ID を確認する必要があります。）

## メディアトークン検証ツール {#media-token-verifier}

エンタイトルメントフローが正常に完了した後にAdobe Pass Authentication で生成される短時間のみ有効なメディアトークンを検証するために、プログラマーが使用するAdobe提供のライブラリ。

## MVPD {#mvpd}

マルチチャンネルビデオプログラミングディストリビュータ。「Pay TV Provider」と同義です。

## MVPD ID {#mvpd-id}

詳しくは、 [ユーザー ID](#user-id).

## パートナー ID {#partner-id}

Adobeが MVPD に渡す識別子。Adobe Pass Authentication が認証をリクエストする代わりに、その識別子を使用して認証をリクエストします。 特定のプログラマーの UI を設定するために使用される場合もありますが、すべてのプログラマーで同じ場合もあります。MVPD のニーズによって異なります。

## 有料テレビプロバイダー {#pay-tv-provider}

と同義語 [MVPD](#mvpd).

## プログラマー {#programmer}

「コンテンツプロバイダー」、「アカウント」、「チャネル」、「サービスプロバイダー」、「ブランド」などと同義です。

## プロキシ MVPD {#proxy-mvpd}

他の MVPD 用の ID サービスを提供する MVPD。Adobe Pass Authentication と直接統合されます。

## プロキシ化 MVPD {#proxied-mvpd}

AdobeSP と直接統合されていないが、プロキシ MVPD を介して統合されている MVPD。

## 要求者 ID {#requestor-id}

を一意に識別する [プログラマー](#programmer) （アカウント、ブランド、チャネルまたはプロパティ）Adobe Pass Authentication 内で使用できます。 この ID は、アカウントの初期設定時に、プログラマーとAdobeの間で決定されます。 Web 上では、要求者 ID はホワイトリストに登録された一連のドメインに関連付けられます。外部ドメインからの ID を使用する呼び出しは拒否されます。 プログラマーは、Analytics で要求者 ID も使用します。 通常、プログラマーごとに 1 つの要求者 ID しかありません。 リクエスト元 ID に関連する追加の機能の 1 つは、setRequestor API 呼び出しは暗号化されたデータが送信され、Adobe Pass認証システムでプログラマーを認証するために使用されるので、プログラマーが公開証明書をAdobeに提供する必要があることです。

## リソース ID {#resource-id}

文字列または mRSS リソースで、 [プログラマー](#programmer) を MVPD に送信します。 これは、プログラマーと MVPD の間で合意されています。Adobe Pass認証は、Resource ID を未タッチで渡すので、すべての MVPD で同じである必要があります。 MVPD が各 ID が何を表すかを認識している限り、プログラマーは複数のリソース ID を使用できます。

## SessionGUID {#sessionGUID}

詳しくは、 [ユーザー ID](#user-id).

## 短時間のみ有効なメディアトークン {#short-lived-token}

このトークンは、特定のユーザーの使用権限プロセスが正常に完了すると、Adobe Pass Authentication によって生成されます。 トークンがプログラマーに渡され、プログラマーは、短時間のみ有効なメディアトークンでAdobe Pass認証トークン検証ツールを使用して、エンタイトルメントプロセスのセキュリティを検証します。

## スマートデバイス {#smart-device}

セットトップボックス、ゲームコンソール、スマートテレビを指すために、Adobe Pass認証に関するドキュメント全体で使用される用語です。 これらは、ネットワーク機能を持つが、Web ページをレンダリングできないデバイスです。

## SP{#sp}

サービスプロバイダー。これは通常、 *役割* SP の、Adobe Pass Authentication が再生し、プログラマーの代わりに、 [MVPD](#mvpd).

## 一時パス {#temp-pass}

プログラマーがコンテンツを支払うための一時的な無料アクセスを提供する機能です。 アクセスは、プログラマが指定した期間、リクエスト元ごとに行われます。

## TTL {#ttl}

有効期間。 トークンが有効である期間を指定します。

## TVE {#tve}

どこでもテレビ。

## ユーザー ID {#user-id}

プログラマーのアプリのユーザーを一意に識別しますが、MVPD から構成されます。 様々な使用例向けに様々なフォームで利用できます。 詳しくは、 [プログラマーの概要でのユーザー ID の理解](/help/authentication/programmer-overview.md#user-ids).

## 許可リスト {#whitelist}

Adobe Pass認証との通信の目的で正当と指定されたドメインのリスト。

## XSTS トークン {#xsts-token}

Xbox/Adobe Pass認証の統合で使用される、Xbox コンソールアプリの開発用にMicrosoftが発行したセキュリティトークン。
