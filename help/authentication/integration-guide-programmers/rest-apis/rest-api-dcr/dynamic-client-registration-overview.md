---
title: Dynamic Client Registration の概要
description: Dynamic Client Registration の概要
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# Dynamic Client Registration の概要 {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

動的クライアント登録は、[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) で定義されている認証メカニズムを表し、[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) で説明されている OAuth 2.0 認証フレームワークに基づいています。

Adobe Passは、次の保護された API へのアクセスを可能にする動的なクライアント登録サービスを提供します。

* Adobe Pass Authentication Management API:
   * [Temp Pass API をリセット](../../features-premium/temporary-access/temp-pass-feature.md)
   * [API の低下](../../features-premium/degraded-access/degradation-api-overview.md)
   * [プロキシMVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [使用権限サービスモニタリング API](../../features-premium/esm/entitlement-service-monitoring-api.md)
* Adobe Pass認証 REST API:
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [（レガシー） REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* Adobe Pass認証 SDK:
   * [（従来の）JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [（従来の）iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [（従来の）Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [（従来の） FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> 動的なクライアント登録承認メカニズムは、廃止される可能性がある古いAdobe Pass認証ソリューションに代わるものです。
>
> * 署名済み要求者 ID メカニズム。
> * ドメインリストのメカニズム。
> * API キーのメカニズム。

動的なクライアント登録の採用に伴う主なメリットは次のとおりです。

* セキュリティの強化。
* プラットフォーム間の統合モデル。
* アプリケーションのライフサイクルをきめ細かく制御します。

動的クライアント登録の管理および使用方法について詳しくは、次の節を参照してください。

## 動的なクライアント登録管理 {#dynamic-client-registration-management}

動的なクライアント登録管理プロセスにより、特定のプラットフォームで動作し、特定のAdobe Pass認証 API へのアクセスを必要とするクライアントアプリケーションは、[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) を使用して登録できます。

Adobe Pass TVE ダッシュボードは、Adobe Pass認証のお客様（プログラマー）が設定とデータを管理するためのツールです。 このセルフサービスダッシュボードにより、[Adobe Pass TVE ダッシュボードユーザーガイド ](../../../user-guide-tve-dashboard/tve-dashboard-overview.md) ドキュメントに記載されている様々な機能が有効になります。

[Adobe Pass TVE ダッシュボード ](https://experience.adobe.com/#/pass/authentication) にアクセスできる場合は、以下の節の手順に従って、登録されたアプリケーションを作成し、ソフトウェアのステートメントをダウンロードします。

### 登録済みアプリケーションの管理 {#manage-registered-applications}

>[!IMPORTANT]
>
> Adobe Pass TVE Dashboard にアクセスできない場合は、[Zendesk](https://adobeprimetime.zendesk.com) を通じてチケットを作成し、テクニカルアカウントマネージャー（TAM）に登録されたアプリを作成してソフトウェアのステートメントを共有するように依頼します。

登録済みアプリケーションを作成する方法は 2 つあります。

* **プログラマーレベル**

  プログラマーレベルの登録プロセスを使用すると、使用可能なすべてのチャネルまたは選択したチャネルサブセットにリンクされた登録済みアプリケーションを作成できます。 詳しくは、プログラマー向け [TVE ダッシュボードユーザーガイド ](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) ドキュメントを参照してください。


* **チャネルレベル**

  チャネルレベルの登録プロセスでは、現在選択されているチャネルにのみリンクされた登録済みアプリケーションを作成できます。 詳しくは、チャネル用 [TVE ダッシュボードユーザーガイド ](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) ドキュメントを参照してください。

>[!IMPORTANT]
>
> セキュリティを強化し、不正アクセスを防ぐために、より具体的で制限された権限を持つ登録済みアプリケーションを作成することをお勧めします。 したがって、登録済みアプリケーションを作成する場合は、割り当てられた `channels`、`platforms`、`scopes` に対して、より狭いオプションを使用することを検討してください。
>
> クライアントアプリケーションのライフサイクルと使用状況を管理するには、クライアントアプリケーションのメジャーアップデートごとに新しい登録アプリケーションを作成することをお勧めします。 必要に応じて、アドビの [Zendesk](https://adobeprimetime.zendesk.com) を通じてチケットを作成し、テクニカルアカウントマネージャー（TAM）に依頼して、特定のクライアントアプリケーションバージョンの機能をブロックするために、登録されたアプリケーションを失効させます。

### ソフトウェア明細書の管理 {#manage-software-statements}

>[!IMPORTANT]
>
> Adobe Pass TVE Dashboard にアクセスできない場合は、[Zendesk](https://adobeprimetime.zendesk.com) を通じてチケットを作成し、テクニカルアカウントマネージャー（TAM）に登録されたアプリを作成してソフトウェアのステートメントを共有するように依頼します。

ソフトウェアのステートメントをダウンロードする前に、[ 登録アプリケーションの管理 ](#manage-registered-applications) の節で説明されているように、クライアントアプリケーションの要件を満たす登録アプリケーションが作成されていることを確認します。

登録済みアプリケーションが作成されたレベルに応じて、ソフトウェア・ステートメントをダウンロードする方法は 2 つあります。

* **プログラマーレベル**

  詳しくは、プログラマー向け [TVE ダッシュボードユーザーガイド ](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md) ドキュメントを参照してください。

* **チャネルレベル**

  詳しくは、チャネル用 [TVE ダッシュボードユーザーガイド ](../../../user-guide-tve-dashboard/tve-dashboard-channels.md) ドキュメントを参照してください。

software ステートメントは、クライアントアプリケーションソフトウェアに関する情報をバンドルとして含む JSON web トークン（`JWT`）です。 [ クライアント資格情報の取得 ](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API に提示されると、ソフトウェアステートメントは、JSON web 署名（`JWS`）を使用してデジタル署名されます。

ソフトウェア ステートメントとその動作の詳細については、[RFC 7591](https://tools.ietf.org/html/rfc7591) ドキュメントを参照してください。

## 動的なクライアント登録フロー  {#dynamic-client-registration-flow}

要約すると、動的なクライアント登録認証メカニズムには、次のようないくつかのステップが含まれます。

**管理**

* クライアント担当者は、[ 登録アプリケーションの管理 ](#manage-registered-applications) の節で説明しているように、登録アプリケーションを作成する必要があります。
* クライアント担当者は、「ソフトウェア明細書の管理 [ の節で説明しているように、ソフトウェア明細書をダウンロードして埋め込む必要が ](#manage-software-statements) ります。

**フロー**

* クライアントアプリケーションは、[ クライアント資格情報の取得 ](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメントの説明に従って、クライアント資格情報を取得する必要があります。
* クライアントアプリケーションは、[ アクセストークンの取得 ](apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、アクセストークンを取得する必要があります。

Adobe Passで保護された API にアクセスする方法について詳しくは、[ 動的クライアント登録フロー ](flows/dynamic-client-registration-flow.md) ドキュメントを参照してください。 さらに、この [ ウェビナー ](https://my.adobeconnect.com/pzkp8ujrigg1/) 録画をご覧いただくこともできます。この録画では、より多くのコンテキストを提供し、デモも含まれています。
