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
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## AccessEnabler {#accessEnabler}

Adobe Pass認証のクライアントコンポーネント。 Adobe Pass認証では、サポートされている各プラットフォームに対して AccessEnabler ライブラリを提供します。

## AuthN {#authn}

「AuthN トークン」、「AuthN フロー」など、「認証」の略記法として使用されます。


## AuthN トークン{#authn-token}

認証トークン：ユーザーが MVPD で正常に認証された後にAdobe Pass Authentication によって生成されます。 プログラマーのプラットフォームに応じて、トークンはユーザーのデバイスまたはAdobe Pass認証サーバーに保存されます。

## AuthZ {#authz}

「認証」の略記法として使用されます（「AuthZ トークン」、「AuthZ フロー」など）。

## AuthZ トークン {#authz-token}

保護されたコンテンツの表示がユーザーに許可された後に、Adobe Pass Authentication によって生成される認証トークン。 AuthZ トークンは、Adobe Pass認証サーバーに保存され、（短時間のみ有効なメディアトークン [ の生成に使用され ](#short-lived-token) す。

## チャネル ID （非推奨） {#channel_id}

リソース ID を表す以前の用語。

## クライアントレス API {#clientless-api}

AccessEnabler クライアント・コンポーネントの代わりに Web サービスを使用するAdobe Pass認証統合ソリューション。

## デバイス ID {#device-id}

デバイス（電話、タブレットなど）を一意に識別します Adobe Pass認証内で行います。 この ID は、プログラマーのアプリケーションによって取得/提供されます。


## 使用権限フロー{#entitlement_flow}

Adobe Pass認証ドキュメントで使用される用語は、Adobe Pass認証を使用したデバイス/ユーザーの登録、MVPD を使用したユーザーの認証、ユーザー用のリソースの認証、ユーザーのログアウトのプロセス全体を指します。


## GUID {#guid}

[ ユーザー ID](#user-id) を参照してください。

## IdP {#idp}

プロバイダーを識別します。Adobe Pass Authentication Integration における MVPD のロールのコンテキストでは、MVPD と同義です。 （お客様は、有料テレビ事業者のログインページで本人確認が必要です。）

## メディアトークン検証子 {#media-token-verifier}

資格フローが正常に完了した後に、Adobe PassAdobeによって生成された短時間のみ有効なメディアトークンを検証するために、プログラマーが使用する、認証提供のライブラリ。

## MVPD {#mvpd}

マルチチャネルビデオプログラミング販売業者。「有料テレビプロバイダー」と同義。

## MVPD ID {#mvpd-id}

[ ユーザー ID](#user-id) を参照してください。

## パートナー ID {#partner-id}

Adobeが MVPD に渡す識別子。MVPD は、Adobe Pass Authentication が認証をリクエストする人物を識別するために使用します。 特定のプログラマー向けの UI の設定に使用される場合もあれば、すべてのプログラマーで同じ場合もあります。MVPD のニーズに応じて異なります。

## 有料テレビ放送事業者 {#pay-tv-provider}

[MVPD](#mvpd) と同義。

## プログラマ {#programmer}

「コンテンツプロバイダー」、「アカウント」、「チャネル」、「サービスプロバイダー」、「ブランド」などと同義。

## プロキシ MVPD {#proxy-mvpd}

他の MVPD に ID サービスを提供する MVPD。Adobe Pass Authentication と直接統合されています。

## プロキシ化された MVPD {#proxied-mvpd}

プロキシ SP と直接統合されていないが、AdobeMVPD を介して統合されている MVPD。

## 要求者 ID {#requestor-id}

Adobe Pass Authentication 内の [ プログラマー ](#programmer) （アカウント、ブランド、チャネル、プロパティ）を一意に識別します。 この ID は、アカウントの初期設定時に、プログラマーとAdobeの間で決定されます。 Web では、リクエスター ID は、許可リストに登録された一連のドメインに関連付けられています。外部ドメインからの ID を使用した呼び出しは拒否されます。 プログラマーは、分析のリクエスター ID も使用します。 通常、プログラマーごとにリクエスター ID は 1 つのみです。 setRequestor API 呼び出しは、暗号化されたデータが送信され、Adobe PassAdobeシステムでのプログラマーの認証に使用されることを想定しているので、プログラマーが公開証明書を提供する必要がある点が、リクエスター ID に関連する追加機能の 1 つです。

## リソース ID {#resource-id}

MVPD に対して [ プログラマ ](#programmer) を識別する文字列または mRSS リソース。 プログラマと MVPD の間で合意されています。Adobe Pass Authentication はリソース ID をそのままに渡すので、すべての MVPD で同じでなければなりません。 MVPD が各 ID が何を表すかを認識している限り、プログラマは複数のリソース ID を使用できます。

## SessionGUID {#sessionGUID}

[ ユーザー ID](#user-id) を参照してください。

## 短時間のみ有効なメディアトークン {#short-lived-token}

このトークンは、特定のユーザーの使用権限プロセスが正常に完了すると、Adobe Pass認証によって生成されます。 トークンはプログラマーに渡され、プログラマーは短時間のみ有効なメディアトークンのAdobe Pass認証トークンベリファイアを使用して、使用権プロセスのセキュリティを確認します。

## スマートデバイス {#smart-device}

Adobe Pass認証ドキュメント全体で使用されている用語で、セットトップボックス、ゲーム機、スマート TV を指します。 ネットワーク機能はあるが、web ページをレンダリングできないデバイスです。

## SP{#sp}

サービスプロバイダー。これは通常、Adobe Pass Authentication が実行し、[MVPD *との統合においてプログラマーの代わりに機能する SP の* ロール ](#mvpd) を参照します。

## 一時パス {#temp-pass}

コンテンツを支払うための一時的な無料アクセスをプログラマーが提供できる機能。 アクセスは、プログラマが指定した期間、要求者ごとに行われます。

## TTL {#ttl}

有効期間。 これは、トークンが有効である、指定された期間です。

## TVE {#tve}

どこでもテレビ。

## ユーザー ID {#user-id}

プログラマーのアプリのユーザーを一意に識別しますが、MVPD から開始します。 様々なユースケース向けに、様々なフォームで使用できます。 [ プログラマの概要のユーザー ID について ](/help/authentication/programmer-overview.md#user-ids) を参照してください。

## 許可リスト {#whitelist}

Adobe Pass Authentication との通信の目的で正当と指定されるドメインのリスト。

## XSTS トークン {#xsts-token}

Xbox コンソール アプリ開発用のMicrosoftによって発行されたセキュリティ トークンで、Xbox/Adobe Pass認証の統合で使用されます。
