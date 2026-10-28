---
title: REST API V2 APIの概要
description: REST API V2 APIの概要
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 35c6ed6491cd47e6f85ee8974d72236c19fb8a6d
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%
---
# REST API V2 APIの概要 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

開始するには、以下のリストに興味のあるAPIの公開ドキュメントにアクセスしてください。

## ベース URL {#rest-api-v2-apis-base-urls}

REST API V2 リクエストには、以下の環境固有のベース URLを使用します。 追加
このベース URLに対する各APIに対してドキュメント化された相対`path`。

一部のAPI応答は、ストリーミングデバイスが開く必要があるインタラクティブ URLを返します
URLを貼り付けます。 例としては、次によって返される`url`属性が含まれます
[特定のmvpdのログアウトを開始](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
`Location` リダイレクトが返されました：
[&#x200B; ユーザーエージェント &#x200B;](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)で認証を実行します。

これらのインタラクティブ URLは、同じAdobe Pass サービスプロバイダー（SP）ホストを使用します。
返されたとおりに絶対インタラクティブ URLを開きます。 Adobe Passが
`/adobe-services/`で始まる相対インタラクティブ URLは、次に対して解決します
ターゲット環境のSP ホスト：

| 環境 | REST API V2とインタラクティブ URL ベース URL |
| --- | --- |
| 本番 | `https://sp.auth.adobe.com` |
| ステージング | `https://sp.auth-staging.adobe.com` |

## 設定 {#rest-api-v2-apis-configuration}

* [特定のサービスプロバイダーの設定の取得](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## セッション {#rest-api-v2-apis-sessions}

* [認証セッションの作成](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [認証セッションを再開](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [認証セッションの取得](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [ユーザーエージェントで認証を実行する](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## プロファイル {#rest-api-v2-apis-profiles}

* [プロファイルの取得](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のmvpdのプロファイルの取得](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## 決定 {#rest-api-v2-apis-decisions}

* [特定のmvpdを使用して認証の決定を取得する](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [特定のmvpdを使用して事前承認決定を取得する](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## ログアウト {#rest-api-v2-apis-logout}

* [特定のmvpdのログアウトを開始](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## パートナーシングルサインオン {#rest-api-v2-apis-partner-single-sign-on}

* [パートナー認証リクエストの取得](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [パートナー認証応答を使用したプロファイルの作成と取得](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## REST API V2を試してみますか？

[Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトから製品専用ページを介して、REST API V2を検索できるようになりました。
