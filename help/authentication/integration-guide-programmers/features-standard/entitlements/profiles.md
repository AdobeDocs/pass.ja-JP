---
title: プロファイル
description: プロファイル
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%
---
# プロファイル {#profiles}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

ユーザーが有料テレビ プロバイダー（MVPD）で正常に認証されたときに、Adobe Pass認証[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)によってプロファイルが作成されます。

プロファイルの種類は、使用する認証方法によって異なります。

* **標準**

  基本認証を通じて作成されます。

* **Apple SSO**

  Appleの動画購読者アカウントフレームワークを使用して、シングルサインオン（SSO）で作成されました。

* **Platform SSO**

  プラットフォーム IDを使用してシングルサインオン（SSO）で作成。

* **サービストークン SSO**

  サービストークンを使用してシングルサインオン（SSO）で作成。

プロファイルは、クライアントアプリケーションが次のことを実行できるようにする主要なデータを保存します。

* ユーザーの認証ステータスを決定します。
* 使用する認証方法を特定します。
* ID プロバイダーを特定します。
* [&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)にアクセスします。

プロファイルは、Adobe Pass認証のバックエンドに安全に保存され、リクエスト側のアプリケーション、デバイス、サービスプロバイダーのIDにリンクされます。 これらは、[認証の有効期間（TTL） &#x200B;](#authentication-ttl-management)で定義されているように、期間限定で有効です。

## 認証の有効期間（TTL）管理 {#authentication-ttl-management}

Authentication Time-to-Live （TTL）: ユーザーが再認証を必要とする前に、認証を受け続ける時間を定義します。 この期間は限られており、MVPDの担当者と合意する必要があります。 TTL値は、次の要素によって異なります。

* プラットフォームカテゴリ（例：デスクトップ、モバイル、TV接続デバイス）
* 特定のプラットフォーム（例：iOS、Android、tvOS、Roku、FireTV）

認証（authN） TTLは、Adobe Pass [TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)を通じて、組織の管理者またはAdobe Pass認証担当者が表示および変更できます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)のドキュメントを参照してください。

## REST API V2 {#rest-api-v2}

プロファイルは、次のAPIを使用して取得できます。

* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のmvpdのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

プロファイルの構造については、上記のAPIの&#x200B;**応答**&#x200B;および&#x200B;**サンプル**&#x200B;の節を参照してください。

上記のAPIを統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本的なプロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [認証フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
