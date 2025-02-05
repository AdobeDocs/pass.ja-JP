---
title: 製品に関するお知らせ
description: 製品に関するお知らせ
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 9dcc649b4216cccc9be35cd6553308bfc345b5f4
workflow-type: tm+mt
source-wordcount: '859'
ht-degree: 1%

---

# 製品に関するお知らせ {#product-announcements}

<a href="https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements">![ ライブウェビナーシリーズ ](/help/authentication/assets/rest-api-v2/live-webinar-series-banner.png)</a>

Adobe Pass認証パートナーの皆様には、新しい REST API に関する次回のライブウェビナーにご招待します。 この新しい API は、エンドユーザーエクスペリエンスを強化しAdobe Pass サービスとの統合を簡素化するために、昨年公開されました。 

新しい API の概要、メリット、新しい API に移行することでアクティブ化できる追加のユースケースを提供するために、一連の 2 つのウェビナーを実施します。

あなたとあなたのチームにとって最適なウェビナーに登録してください。

* [ ウェビナー 1 - 2025 年 2 月 19 日 ](https://events.teams.microsoft.com/event/83c6ec1e-2522-4918-910f-c529b4fb0574@fa7b1b5a-7b34-4387-94ae-d2c178decee1)
* [ ウェビナー 2 - 2025 年 3 月 5 日 ](https://events.teams.microsoft.com/event/6f673689-e848-4c00-93d9-4f5d8f108977@fa7b1b5a-7b34-4387-94ae-d2c178decee1)

このセッションでは、次の内容について学習します。

* REST API v2 の概要と利点
* 基本フローのチュートリアル
* タイムラインと次の手順

このウェビナーは、次の場合に役立ちます。

* 新しい TVE アプリのローンチを計画する新しいお客様
* 新しい API への移行が必要な既存のお客様
* 顧客向け API を実装する実装パートナー

新しい API に関する技術ドキュメントは [ こちら ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md) で参照できます。 セッション中に話し合いたい質問があれば、[ こちら ](https://forms.office.com/r/sJea78tUy3) に記入することをお勧めします。

## 提供終了（EOL） {#eol}

この節では、廃止が予定されている特定のAdobe Pass認証機能と製品のサポート終了とサポート終了日について説明します。

廃止措置のタイムラインについて常に情報を提供し、以下に説明するアクションを実行する予定であることを確認してください。

| お知らせ | お知らせ日 | サポート終了日 | 提供終了日 | 説明 |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 のサポート終了 | 12/11/2024 | 該当なし | 01/08/2025 | Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 の提供終了は、**2025 年 1 月 8 日** に予定しています。 この日以降、AccessEnabler JavaScript SDK v3.5 で提供される機能およびサービスは、ユーザー認証および許可を含め、機能しなくなります。 |
| Adobe Pass認証の非 DCR セキュリティメカニズムのサポート終了 | 12/11/2024 | 該当なし | 01/20/2025 | Adobe Pass認証の非 DCR セキュリティメカニズムの提供終了は **2025 年 1 月 20 日** を予定しています。 このメカニズムは、次のAdobe Pass認証 API へのアクセスを保護するために使用されました。<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">Reset Temp Pass API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">API の低下 </a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md"> プロキシMVPD API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md"> 使用権限サービスの監視 API</a></li></ul>この日付以降、このセキュリティメカニズムを使用してアクセスされた上記の API で提供される機能およびサービスは機能しなくなります。</br></br> 継続的なサービスを確保するには、すべてのアプリケーションを「動的クライアント登録 [ メカニズムを使用するように移行する必要があ ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ます。 |
| Adobe Pass認証 REST API v1 のサポート終了 | 12/11/2024 | 12/31/2025 | 2026 年末（未定） | Adobe Pass認証 REST API v1 のサポートは、**2025 年 12 月 31 日** に廃止する予定です。 この期限を過ぎると、更新または修正は提供されなくなります。</br></br> 継続的なサポートを確保するには、すべてのアプリケーションを <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> に移行する必要があります。</br></br>Adobe Pass認証 REST API v1 のサポート終了は、**2026 年末** を予定しています。 この日以降、REST API v1 で提供される機能やサービスは、ユーザー認証や承認を含め、機能しなくなります。</br></br> 継続的なサービスを確保するには、すべてのアプリケーションを <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> に移行する必要があります。 |
| Adobe Pass Authentication AccessEnabler SDK のサポート終了 | 12/11/2024 | 05/31/2026 | 2026 年末（未定） | Adobe Pass Authentication AccessEnabler SDK のサポートは、**2026 年 5 月 31 日** に終了する予定です。 この期限を過ぎると、更新または修正は提供されなくなります。</br></br> 継続的なサポートを確保するには、すべてのアプリケーションを <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> に移行する必要があります。</br></br>Adobe Pass Authentication AccessEnabler SDK の提供終了は、**2026 年末** を予定しています。 この日付を過ぎると、AccessEnabler SDK で提供される機能およびサービスは、ユーザー認証および承認を含めて機能しなくなります。</br></br> 継続的なサービスを確保するには、すべてのアプリケーションを <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a> に移行する必要があります。 |

## 製品リリース {#product-releases}

この節では、Adobe Pass Authentication のリリース履歴への参照と対応するリリースノートをコンパイルします。

### 2024 {#product-releases-2024}

| リリースノート | 日付 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 3.0.3 リリースノート ](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Adobe Pass Authentication 3.0 リリースノート ](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Adobe Pass Authentication 2.70 リリースノート ](notes-releases/auth-rn-270.md) | 2024/4/23 - 2024/4/25 |
| [Adobe Pass Authentication 2.69 リリースノート ](notes-releases/auth-rn-269.md) | 2024/2/27 - 2024/2/29 |
| [Adobe Pass Authentication JavaScript SDK 4.7.0 リリースノート ](notes-releases/authn-rn-javascript-470.md) | 2024/2/27 - 2024/2/29 |
| [Adobe Pass認証iOS/tvOS SDK 3.9.2 リリースノート ](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Adobe Pass認証iOS/tvOS SDK 3.8.4 リリースノート ](notes-releases/authn-rn-ios-tvos-384.md) | 2024/1/26 |

### 2023 {#product-releases-2023}

| リリースノート | 日付 |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 2.68 リリースノート ](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Adobe Pass Authentication 2.67 リリースノート ](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Adobe Pass Authentication 2.66 リリースノート ](notes-releases/auth-rn-266.md) | 2023/7/11 - 2023/7/13 |
| [Adobe Pass Authentication 2.65.1 リリースノート ](notes-releases/auth-rn-2651.md) | 2023/6/20 - 2023/6/22 |
| [Adobe Pass Authentication 2.65 リリースノート ](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Adobe Pass Authentication 2.64.1 リリースノート ](notes-releases/auth-rn-2641.md) | 2023/1/31 - 2023/02/02 |
| [Adobe Pass認証iOS/tvOS SDK 3.8.3 リリースノート ](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Adobe Pass認証iOS/tvOS SDK 3.8.2 リリースノート ](notes-releases/authn-rn-ios-tvos-382.md) | 2023/02/10 |
| [Adobe Pass認証iOS/tvOS SDK 3.8.1 リリースノート ](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Adobe Pass Authentication Android SDK 3.7.3 リリースノート ](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| リリースノート | 日付 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 2.64 リリースノート ](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Adobe Pass Authentication 2.63 リリースノート ](notes-releases/auth-rn-263.md) | 2022/9/20 - 2022/9/22 |
| [Adobe Pass Authentication 2.62.1 リリースノート ](notes-releases/auth-rn-2621.md) | 2022/08/02 - 2022/08/04 |
| [Adobe Pass Authentication JavaScript SDK 4.6.0 リリースノート ](notes-releases/authn-rn-javascript-460.md) | 2022/9/20 - 2022/9/22 |

### 2021 {#product-releases-2021}

| リリースノート | 日付 |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Adobe Pass Authentication JavaScript SDK 4.4.0 リリースノート ](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Adobe Pass認証iOS/tvOS SDK 3.7.0 リリースノート ](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
