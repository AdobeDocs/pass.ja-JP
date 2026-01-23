---
title: REST API V2 の概要
description: REST API V2 の概要
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: 63dc9636f74f8eee1af6205c4d31a01df4503050
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---

# REST API V2 の概要 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

TVE アプリケーションのコスト効率を向上させたいとお考えですか？

複数のプラットフォームで TVE アプリケーションをサポートするために必要な開発時間とリソースを削減したいですか？

プラットフォーム間で一貫したユーザーエクスペリエンスを確保しますか？

メンテナンス作業を軽減し、更新、バグ修正、改善の提供を簡素化したいですか？

## REST API V2 の概要 {#rest-api-v2-introduction}

Adobe Pass Authentication は、ユーザーエクスペリエンスを強化し、パスサービスとの統合を簡素化するように設計された REST API V2 のリリースを発表しました。

プラットフォームにとって大きな前進となり、新機能とアプリケーションフローの扉を開く、新しい REST API の可能性に興奮しています。

## 新着情報 {#rest-api-v2-whats-new}

すべてのプラットフォームで一意の実装

顧客のアプリケーションは、プラットフォーム間で同じ実装を使用できるようになり、新機能の起動やライブアプリの管理が容易になりました。

### クロスデバイス SSO {#rest-api-v2-cross-device-sso}

REST API V2 を使用すると、異なるデバイス間で認証セッションを安全に渡すことができます。 デバイス間でセッションを受け渡すだけで、ユーザーは自身のモバイルデバイスで認証を行い、再認証を行わずにテレビに接続されたデバイスでビデオをストリーミングすることができます。

### 複数のアクティブな認証セッション {#rest-api-v2-multiple-active-authentication-sessions}

様々なアクティブなMVPD セッションが可能になり、お客様は必要に応じて、TempPass と通常のMVPD統合を切り替えることができます。

### セキュリティメカニズムの強化 {#rest-api-v2-enhanced-security-mechanism}

動的クライアント登録は、すべてのフローおよび機能で使用されるセキュリティメカニズムです。 これにより、顧客のアプリケーションをより安全かつ詳細に制御でき、すべてのプラットフォームでアプリケーションを登録できます。

### パフォーマンスの向上による応答時間の短縮 {#rest-api-v2-improved-performance}

強化されたキャッシュ メカニズムにより、MVPD へのトラフィックが減少し、応答時間が改善され、遅延が減少します。 全体として、ビデオが開始されるまでの API 呼び出し数は減少しています。

### すべてのフローの拡張エラーコード {#rest-api-v2-enhanced-error-codes}

高度なエラーコードが、同じ形式のすべてのパスフローで使用できるようになりました。アプリケーションの全体的なユーザーエクスペリエンスを向上させるためのガイドを含む追加の詳細が追加されました。

### すべての認証セッションに対する制御の向上 {#rest-api-v2-improved-control}

新しい REST API V2 では、認証された複数のセッションに対するアクションを同時に実行できます。

### メンテナンスコストの削減 {#rest-api-v2-reduce-maintenance-costs}

すべての応答とエラー情報が正規化されました。

## 次の手順 {#rest-api-v2-whats-next}

SDK または REST 呼び出しを通じて現在 API を使用しているすべてのお客様は、引き続き API を使用でき、2025 年末までサポートを提供する予定です。

ただし、今後の開発はすべて REST API V2 上に構築されます。 最新のAdobe Passの機能を活用するために、移行プロセスを開始することを強くお勧めします。

## 詳細情報 {#rest-api-v2-want-to-learn-more}

開始するには、アドビの公開ドキュメントを参照してください。

- [ウェビナー](#rest-api-v2-webinar)
- [用語集](rest-api-v2-glossary.md)
- [Checklist](rest-api-v2-checklist.md)
- [AI ルール](rest-api-v2-ai-rules.md)
- [よくある質問](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [フロー](flows/rest-api-v2-flows-overview.md)
- クックブック
- 付録
- [必要なシステム構成](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

また、アドビの専任サポートチームが、必要な質問や技術サポートをお手伝いします。

### ウェビナー {#rest-api-v2-webinar}

新しい REST API V2 のウェビナーをご覧ください。新機能の概要と、動作する REST API V2 のライブデモを紹介しました。

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## REST API V2 を試してみますか？ {#rest-api-v2-want-to-try}

[Adobe Developer](https://developer.adobe.com/adobe-pass/) web サイトの製品専用ページから REST API V2 を参照できるようになりました。
