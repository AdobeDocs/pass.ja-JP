---
title: 動的なクライアント登録の概要
description: 動的なクライアント登録の概要
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 0%
---
# 動的なクライアント登録の概要 {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

動的クライアント登録は、[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)によって定義された認証メカニズムを表し、[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)によって記述されているOAuth 2.0認証フレームワークに基づいています。

Adobe Passでは、次の保護されたAPIへのアクセスを可能にする動的なクライアント登録サービスを提供しています。

* Adobe Pass Authentication Management API:
  * [Temp Pass APIのリセット](../../features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
  * [劣化API](../../features-premium/degraded-access/degradation-feature.md#degradation-api-access)
  * [Proxy MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
  * [使用権限サービス監視API](../../features-premium/esm/entitlement-service-monitoring-api.md)
* Adobe Pass Authentication REST API:
  * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
  * [（レガシー） REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* Adobe Pass Authentication SDK:
  * [（レガシー） JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
  * [（レガシー） iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
  * [（レガシー） Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
  * [（レガシー） FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> Dynamic client registration authorization mechanismは、廃止される可能性のある古いAdobe Pass Authentication ソリューションに代わるものです。
>
> * 署名済み依頼者ID メカニズム。
> * ドメインリストメカニズム。
> * API キーの仕組み。

動的な顧客登録の導入により、主な利点は次のとおりです。

* セキュリティ機能の強化：
* プラットフォームをまたいだ統合モデル。
* アプリケーションのライフサイクルをきめ細かく制御。

動的クライアント登録の管理と使用方法について詳しくは、次の節を参照してください。

## 動的なクライアント登録管理 {#dynamic-client-registration-management}

動的なクライアント登録管理プロセスにより、特定のプラットフォームで動作し、特定のAdobe Pass認証APIへのアクセスを必要とするクライアントアプリケーションが[Adobe Pass TVE ダッシュボード ](https://experience.adobe.com/#/pass/authentication)を通じて登録できるようになります。

Adobe Pass TVE ダッシュボードは、Adobe Pass認証のお客様（プログラマー）が設定とデータを管理するためのツールです。 このセルフサービスダッシュボードを使用すると、[Adobe Pass TVE ダッシュボードユーザーガイド ](../../../user-guide-tve-dashboard/tve-dashboard-overview.md)のドキュメントに記載されている様々な機能を利用できます。

[Adobe Pass TVE ダッシュボード ](https://experience.adobe.com/#/pass/authentication)にアクセスできる場合は、以下のセクションの手順に従って、登録済みアプリケーションを作成し、ソフトウェアステートメントをダウンロードしてください。

### 登録済みアプリケーションの管理 {#manage-registered-applications}

>[!IMPORTANT]
>
> Adobe Pass TVE ダッシュボードにアクセスできない場合は、[Zendesk](https://adobeprimetime.zendesk.com)でチケットを作成し、テクニカルアカウントマネージャー（TAM）に登録アプリケーションの作成とソフトウェアに関する声明の共有を依頼します。

登録済みアプリケーションを作成するには、次の2つの方法があります。

* **プログラマーレベル**

  プログラマーレベルの登録プロセスを使用すると、利用可能なすべてのチャネルまたは選択したチャネルのサブセットにリンクされた登録済みアプリケーションを作成できます。 詳しくは、「[TVE Dashboard User Guide for Programmers](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md)」のドキュメントを参照してください。


* **チャネルレベル**

  チャネルレベルの登録プロセスを使用すると、現在選択されているチャネルにのみリンクされた登録済みアプリケーションを作成できます。 詳しくは、「[TVE Dashboard User Guide for Channels](../../../user-guide-tve-dashboard/tve-dashboard-channels.md)」のドキュメントを参照してください。

>[!IMPORTANT]
>
> セキュリティを強化し、不正アクセスを防止するために、より具体的で制限された権限を持つ登録アプリケーションを作成することをお勧めします。 したがって、登録アプリケーションを作成する際には、割り当てられた`channels`、`platforms`、`scopes`に対して絞り込みオプションを使用することを検討してください。
>
> クライアントアプリケーションのライフサイクルと使用状況を管理するために、クライアントアプリケーションのメジャーアップデートごとに新しい登録アプリケーションを作成することをお勧めします。 必要に応じて、[Zendesk](https://adobeprimetime.zendesk.com)でチケットを作成し、特定のクライアントアプリケーションバージョンの機能をブロックするために、登録されたアプリケーションを取り消すようにテクニカルアカウントマネージャー（TAM）に依頼します。

### ソフトウェアステートメントの管理 {#manage-software-statements}

>[!IMPORTANT]
>
> Adobe Pass TVE ダッシュボードにアクセスできない場合は、[Zendesk](https://adobeprimetime.zendesk.com)でチケットを作成し、テクニカルアカウントマネージャー（TAM）に登録アプリケーションの作成とソフトウェアに関する声明の共有を依頼します。

ソフトウェア ステートメントをダウンロードする前に、[登録アプリケーションの管理](#manage-registered-applications) セクションの説明に従って、クライアント アプリケーションの要件を満たす登録アプリケーションが作成されていることを確認してください。

登録アプリケーションが作成されたレベルに基づいて、ソフトウェアステートメントをダウンロードするには、次の2つの方法があります。

* **プログラマーレベル**

  詳しくは、「[TVE Dashboard User Guide for Programmers](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md)」のドキュメントを参照してください。

* **チャネルレベル**

  詳しくは、「[TVE Dashboard User Guide for Channels](../../../user-guide-tve-dashboard/tve-dashboard-channels.md)」のドキュメントを参照してください。

software ステートメントは、クライアントアプリケーションソフトウェアに関する情報をバンドルとして含むJSON Web トークン （`JWT`）です。 [ クライアント資格情報を取得](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) APIに提示すると、ソフトウェア文はJSON Web署名（`JWS`）を使用してデジタル署名されます。

ソフトウェアステートメントとその仕組みについて詳しくは、[RFC 7591](https://tools.ietf.org/html/rfc7591)のドキュメントを参照してください。

## 動的なクライアント登録フロー {#dynamic-client-registration-flow}

要約すると、動的なクライアント登録認証メカニズムにはいくつかの手順があります。

**管理**

* クライアント担当者は、[登録アプリケーションの管理](#manage-registered-applications) セクションの説明に従って、登録アプリケーションを作成する必要があります。
* クライアントの担当者は、[ ソフトウェアステートメントの管理](#manage-software-statements) セクションの説明に従って、ソフトウェアステートメントをダウンロードして埋め込む必要があります。

**フロー**

* クライアントアプリケーションは、[ クライアント資格情報の取得](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメントの説明に従って、クライアント資格情報を取得する必要があります。
* クライアントアプリケーションは、[ アクセストークンの取得](apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、アクセストークンを取得する必要があります。

Adobe Passで保護されたAPIへのアクセス方法をより詳細に理解するには、[Dynamic Client Registration Flow](flows/dynamic-client-registration-flow.md) ドキュメントを参照してください。 さらに、この[ ウェビナー](https://my.adobeconnect.com/pzkp8ujrigg1/)の録画も視聴できます。この録画では、より多くのコンテキストが提供され、デモも含まれています。
