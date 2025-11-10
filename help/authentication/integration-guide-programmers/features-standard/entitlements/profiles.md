---
title: プロファイル
description: プロファイル
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# プロファイル {#profiles}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

プロファイルは、ユーザーが有料テレビプロバイダー（MVPD[&#x200B; で正常に認証されると、Adobe Pass認証 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)REST API V2）によって作成されます。

プロファイルのタイプは、使用する認証方法によって異なります。

* **標準**

  基本認証で作成されます。

* **Apple SSO**

  Appleのビデオ購読者アカウントフレームワークを使用して、シングルサインオン（SSO）を通じて作成されます。

* **Platform SSO**

  プラットフォーム ID を使用したシングルサインオン（SSO）を介して作成されます。

* **サービストークン SSO**

  サービストークンを使用したシングルサインオン（SSO）を通じて作成されます。

プロファイルには、クライアントアプリケーションが次の操作をおこなうための主要なデータが格納されます。

* ユーザーの認証ステータスを判断します。
* 使用する認証方法を識別します。
* ID プロバイダーを識別します。
* [&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) にアクセスします。

プロファイルは、Adobe Pass Authentication のバックエンドに安全に保存され、要求元のアプリケーション、デバイス、サービスプロバイダーの ID にリンクされます。 [&#x200B; 認証有効期間（TTL） &#x200B;](#authentication-ttl-management) で定義されている限られた期間、有効なままです。

## 認証有効期間（TTL）の管理 {#authentication-ttl-management}

認証有効期間（TTL）は、ユーザーが認証を維持してから再認証が必要になるまでの期間を定義します。 この期間は制限されており、MVPDの担当者と合意する必要があります。 TTL 値は、以下に基づいて変化する可能性があります。

* プラットフォームカテゴリ（デスクトップ、モバイル、TV 接続デバイスなど）
* 特定のプラットフォーム（iOS、Android、tvOS、Roku、FireTV など）

認証（authN） TTL の表示や変更は、Adobe Pass[TVE ダッシュボード &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) を通じて、組織管理者の 1 人が、またはAdobe Pass認証担当者が代理で行うことができます。

詳しくは、[TVE ダッシュボード統合ユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows) ドキュメントを参照してください。

## REST API V2 {#rest-api-v2}

プロファイルは、次の API を使用して取得できます。

* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の mvpd のプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

プロファイルの構造については、上記の API の **Response** および **Samples** の節を参照してください。

上記の API を統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [&#x200B; 認証フェーズに関するよくある質問 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
