---
title: REST API V2の概要
description: REST API V2の概要
exl-id: a5595193-82c4-4033-bd98-596b4908b401
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%
---
# REST API V2の概要 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

TVE アプリケーションのコスト効率を改善しますか？

複数のプラットフォームでTVE アプリケーションをサポートするために必要な開発時間とリソースを削減しますか？

プラットフォーム全体で一貫したユーザーエクスペリエンスを実現したい場合、

メンテナンスの手間を減らし、アップデート、バグ修正、改良の提供を簡素化しますか？

## REST API V2の概要 {#rest-api-v2-introduction}

Adobe Pass Authenticationは、ユーザーエクスペリエンスを向上させ、Pass サービスとの統合を簡素化するように設計されたREST API V2のリリースを発表できることを嬉しく思います。

「当社のプラットフォームにとって大きな前進であり、新機能やアプリケーションフローの扉を開く新しいREST APIの可能性に期待しています。

## 最新情報は何でしょう？ {#rest-api-v2-whats-new}

すべてのプラットフォームに独自の実装を実施

顧客のアプリケーションは、プラットフォームをまたいで同じ実装を使用できるようになり、新機能のローンチやライブアプリの維持が簡単になりました。

### クロスデバイス SSO {#rest-api-v2-cross-device-sso}

REST API V2を使用すると、認証セッションを異なるデバイス間で安全に渡すことができます。 デバイス間でセッションを渡すだけで、利用者は再認証することなく、モバイルデバイスで認証を行い、TV接続デバイスで動画をストリーミングすることができます。

### 複数のアクティブな認証セッション {#rest-api-v2-multiple-active-authentication-sessions}

アクティブなMVPD セッションが異なるようになり、必要に応じてTempPassと通常のMVPD統合を切り替えることができます。

### 強化されたセキュリティメカニズム {#rest-api-v2-enhanced-security-mechanism}

動的クライアント登録は、すべてのフローと機能で使用されるセキュリティメカニズムです。 これにより、顧客のアプリケーションをより安全かつ詳細に制御でき、あらゆるプラットフォームにアプリケーションを登録できます。

### 応答時間を短縮するためのパフォーマンスの向上 {#rest-api-v2-improved-performance}

強化されたキャッシングメカニズムにより、MVPDへのトラフィックを減らし、応答時間を短縮し、待ち時間を短縮できます。 全体として、ビデオが開始されるまでAPI呼び出しの数が減ります。

### すべてのフローのエラーコードを強化 {#rest-api-v2-enhanced-error-codes}

高度なエラーコードが、すべてのPass フローで同じ形式で使用できるようになりました。追加の詳細により、アプリケーション全体のユーザーエクスペリエンスを向上させることができます。

### すべての認証セッションの制御を改善 {#rest-api-v2-improved-control}

新しいREST API V2では、同時に複数の認証済みセッションでアクションを実行できます。

### メンテナンスコストを削減 {#rest-api-v2-reduce-maintenance-costs}

すべての応答とエラー情報が正規化されました。

## 次のステップ？ {#rest-api-v2-whats-next}

現在、SDKまたはREST呼び出しを通じてAPIを使用しているすべてのお客様は、2025年末までサポートを提供し続ける予定です。

ただし、今後のすべての開発はREST API V2上に構築されます。 最新のAdobe Pass機能を活用するために、移行プロセスを開始することを強くお勧めします。

## Adobe Experience Platform Data Governanceについて詳しくは、 {#rest-api-v2-want-to-learn-more}

Adobe Experience Manager Sitesの実装を始めるには、次の公開ドキュメントをご覧ください。

- [ウェビナー](#rest-api-v2-webinar)
- [用語集](rest-api-v2-glossary.md)
- [チェックリスト](rest-api-v2-checklist.md)
- [AI ルール](rest-api-v2-ai-rules.md)
- [FAQ](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [フロー](flows/rest-api-v2-flows-overview.md)
- Cookbooks
- 付録
- [最小必要システム構成](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

また、専任のサポートチームが、お客様の質問や技術的なサポートをサポートします。

### ウェビナー {#rest-api-v2-webinar}

新しいREST API V2に関するウェビナーでは、新機能の概要と、REST API V2の実際のデモを紹介しました。

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## REST API V2を試してみますか？ {#rest-api-v2-want-to-try}

[Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトから製品専用ページを介して、REST API V2を検索できるようになりました。
